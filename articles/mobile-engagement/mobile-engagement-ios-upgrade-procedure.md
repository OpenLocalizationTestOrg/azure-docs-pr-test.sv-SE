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
# <a name="upgrade-procedures"></a>Uppgraderingsprocesser
Om du redan har integrerat en äldre version av Engagement i ditt program, har du tooconsider hello följande punkter när du uppgraderar hello SDK.

För varje ny version av hello SDK måste du först ersätta (ta bort och importera på nytt i xcode) hello EngagementSDK och EngagementReach mappar.

## <a name="from-300-too400"></a>Från 3.0.0 too4.0.0
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

### <a name="usernotifications-framework"></a>UserNotifications framework
Du behöver tooadd hello `UserNotifications` ramverk i din Build Phases.

Öppna projektet-fönstret och välj hello rätt mål i hello Projektutforskaren. Öppna hello **”Build-faser”** fliken och i hello **”länka binär med bibliotek”** menyn Lägg till framework `UserNotifications.framework` -uppsättningen hello länka som`Optional`

### <a name="application-push-capability"></a>Programmet push-funktion
XCode 8 kan återställa din app push-funktion, kontrollera den i hello `capability` fliken i ditt valda målet.

### <a name="add-hello-new-ios-10-notification-registration-code"></a>Lägg till hello nya iOS 10 registrering Meddelandekod
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

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a>UNUserNotificationCenter ombud konfliktlösning

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

## <a name="from-200-too300"></a>Från 2.0.0 too3.0.0
Bort stöd för iOS 4.X. Från och med den här versionen hello distributionsmålet för ditt program måste vara minst iOS 6.

Om du använder Reach i ditt program, måste du lägga till `remote-notification` värdet toohello `UIBackgroundModes` matris i filen Info.plist i ordning tooreceive remote meddelanden.

Hej metoden `application:didReceiveRemoteNotification:` måste ersättas med toobe `application:didReceiveRemoteNotification:fetchCompletionHandler:` i programdelegaten.

”AEPushDelegate.h” är föråldrad gränssnitt och du behöver tooremove alla referenser. Detta inbegriper att ta bort `[[EngagementAgent shared] setPushDelegate:self]` och hello delegera metoder från programdelegaten:

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

## <a name="from-1160-too200"></a>Från 1.16.0 too2.0.0
hello beskrivs nedan hur toomigrate SDK-integration från hello Capptain tjänsten erbjuds av Capptain SAS i en app med Azure Mobile Engagement.
Om du migrerar från en tidigare version finns hello Capptain webbplats toomigrate too1.16 först sedan tillämpa hello nedan.

> [!IMPORTANT]
> Capptain och Mobile Engagement hello inte samma tjänster och hello proceduren endast anges nedan visar hur toomigrate hello-klientappen. Migrera hello SDK i hello app kommer inte att migrera data från hello Capptain servrar toohello Mobile Engagement-servrar
> 
> 

### <a name="agent"></a>Agent
Hej metoden `registerApp:` har ersatts av hello nya metoden `init:`. Programdelegaten måste uppdateras och Använd anslutningssträng:

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

SmartAd spårning har tagits bort från SDK som du precis har tooremove alla instanser av `AETrackModule` klass

### <a name="class-name-changes"></a>Klassen namnändringar
Som en del av hello rebranding, finns det några klassen/filnamn som behöver toobe ändras.

Alla klasser prefixet ”CP” ändras med ”AE” prefix.

Exempel:

* `CPModule.h`har bytt namn för`AEModule.h`.

Alla klasser prefixet ”Capptain” ändras med prefixet ”Engagement”.

Exempel:

* Hej klassen `CapptainAgent` har bytt namn för`EngagementAgent`.
* Hej klassen `CapptainTableViewController` har bytt namn för`EngagementTableViewController`.
* Hej klassen `CapptainUtils` har bytt namn för`EngagementUtils`.
* Hej klassen `CapptainViewController` har bytt namn för`EngagementViewController`.

