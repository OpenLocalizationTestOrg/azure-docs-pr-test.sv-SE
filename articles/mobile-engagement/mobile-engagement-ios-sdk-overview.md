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
# <a name="ios-sdk-for-azure-mobile-engagement"></a>iOS SDK för Azure Mobile Engagement
Börja här tooget alla hello information om hur toointegrate Azure Mobile Engagement i en iOS-App. Om du vill att toogive den ett försök att först kontrollera att du gör våra [15 minuter att slutföra kursen](mobile-engagement-ios-get-started.md).

Klicka på toosee hello [SDK innehåll](mobile-engagement-ios-sdk-content.md)

## <a name="integration-procedures"></a>Integration procedurer
1. Börja här: [hur toointegrate Mobile Engagement i din iOS-app](mobile-engagement-ios-integrate-engagement.md)
2. För aviseringar: [hur toointegrate Reach (meddelanden) i din iOS-app](mobile-engagement-ios-integrate-engagement-reach.md)
3. Tagga planera implementeringen: [hur toouse hello avancerade Mobile Engagement API-märkning i din iOS-app](mobile-engagement-ios-use-engagement-api.md)

## <a name="release-notes"></a>Viktig information
### <a name="410-07172017"></a>4.1.0 (07/17/2017)
* Fast Aktivitetsikoner avmarkerad i bakgrunden.
* Fast varningar på XCode 9 om API: er som inte anropas i huvudkön.
* Fast en minnesläcka i Reach-avsökningar.
* Bort stöd för iOS 6.X. Från och med den här versionen hello distributionsmålet för ditt program måste vara minst iOS 7.

Tidigare version finns hello [slutföra viktig information](mobile-engagement-ios-release-notes.md)

## <a name="upgrade-procedures"></a>Uppgraderingsprocesser
Om du redan har integrerat en äldre version av Engagement i ditt program, har du tooconsider hello följande punkter när du uppgraderar hello SDK.

Du kan ha toofollow flera procedurer om du har missat flera versioner av hello SDK finns hello fullständig [uppgradera procedurer](mobile-engagement-ios-upgrade-procedure.md).

För varje ny version av hello SDK måste du först ersätta (ta bort och importera på nytt i xcode) hello EngagementSDK och EngagementReach mappar.

### <a name="from-300-too400"></a>Från 3.0.0 too4.0.0
### <a name="xcode-8"></a>XCode 8
XCode 8 är obligatoriskt från version 4.0.0 av hello SDK.

> [!NOTE]
> Om du verkligen är beroende av XCode 7 så att du kan använda hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh). Det finns ett känt fel på hello reach-modulen för den här tidigare version när du kör på iOS 10-enheter: systemmeddelanden är inte åtgärdade. toofix detta behöver tooimplement hello föråldrad API `application:didReceiveRemoteNotification:` i din app delegera på följande sätt:
>
>

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> **Vi rekommenderar inte den här lösningen** som detta beteende kan ändras i kommande (även mindre) iOS version uppgraderar eftersom den här iOS API är inaktuell. Du bör växla tooXCode 8 så snart som möjligt.
>
>

#### <a name="usernotifications-framework"></a>UserNotifications framework
Du behöver tooadd hello `UserNotifications` ramverk i din Build Phases.

Öppna projektet-fönstret och välj hello rätt mål i hello Projektutforskaren. Öppna hello **”Build-faser”** fliken och i hello **”länka binär med bibliotek”** menyn Lägg till framework `UserNotifications.framework` -uppsättningen hello länka som`Optional`

#### <a name="application-push-capability"></a>Programmet push-funktion
XCode 8 kan återställa din app push-funktion, kontrollera den i hello `capability` fliken i ditt valda målet.

#### <a name="add-hello-new-ios-10-notification-registration-code"></a>Lägg till hello nya iOS 10 registrering Meddelandekod
hello äldre kodfragment tooregister hello app toonotifications fungerar fortfarande men använder föråldrad API: er när du kör på iOS 10.

Importera hello `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>

I programdelegaten `application:didFinishLaunchingWithOptions` metoden Ersätt:

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

av:

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

#### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a>UNUserNotificationCenter ombud konfliktlösning

*Om varken ditt program eller en av tredjeparts-biblioteken implementerar en `UNUserNotificationCenterDelegate` och sedan kan du hoppa över den här delen.*

En `UNUserNotificationCenter` delegat som används av hello SDK toomonitor hello livscykel Engagement meddelanden på enheter som kör IOS 10 eller senare. hello SDK har sin egen implementering av hello `UNUserNotificationCenterDelegate` protokoll, men det kan bara finnas ett `UNUserNotificationCenter` delegera per program. Andra ombud läggs toohello `UNUserNotificationCenter` objekt står i konflikt med hello Engagement en. Om hello SDK upptäcker din eller någon annan tredje parts ombud kommer det inte att använda sin egen implementering toogive hello du en chans tooresolve konflikter. Du måste tooadd hello Engagement logik tooyour äger en delegat i ordning tooresolve hello konflikter.

Det finns två sätt tooachieve detta.

Förslag 1, genom att helt enkelt vidarebefordra ombudet anropar toohello SDK:

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

Eller förslag 2, med arv från hello `AEUserNotificationHandler` klass

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
> Du kan fastställa om ett meddelande kommer från Engagement eller inte genom att ange dess `userInfo` ordlista toohello Agent `isEngagementPushPayload:` klassen metoden.

Kontrollera att hello `UNUserNotificationCenter` objektets ombud anges tooyour ombud inom antingen hello `application:willFinishLaunchingWithOptions:` eller hello `application:didFinishLaunchingWithOptions:` metod för programdelegaten.
Till exempel om du har implementerat hello ovan förslag 1:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }
