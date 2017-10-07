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
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Komma igång med Azure Mobile Engagement för iOS-appar i Objective C
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand din app användnings- och skicka push-meddelanden toosegmented användare tooan iOS-program.
I den här självstudiekursen skapar du en tom iOS-app som samlar in grundläggande data och tar emot push-meddelanden via Apple Push Notification System (APNS).

Den här kursen kräver hello följande:

* XCode 8, som du kan installera från Mac App Store
* Hej [Mobile Engagement iOS SDK]

Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Mobile Engagement och iOS-appar.

> [!NOTE]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).
>
>

## <a id="setup-azme"></a>Konfigurera Mobile Engagement för din iOS-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen
Den här kursen behandlas en ”grundläggande integration”, vilket är hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande. hello fullständiga integrationsdokumentationen finns i hello [Mobile Engagement iOS SDK-integration](mobile-engagement-ios-sdk-overview.md)

Vi skapar en grundläggande app i XCode toodemonstrate hello integrering.

### <a name="create-a-new-ios-project"></a>Skapa ett nytt iOS-projekt
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-toohello-mobile-engagement-backend"></a>Ansluta appen toohello Mobile Engagement-serverdelen
1. Hämta hello [Mobile Engagement iOS SDK].
2. Extrahera hello. tar.gz tooa mapp på datorn.
3. Högerklicka på hello-projektet och välj sedan **Lägg till filer i**.

    ![][1]
4. Navigera toohello mapp där du extraherade hello SDK, markera hello `EngagementSDK` mappen, klicka på **alternativ** hello nedre vänstra hörnet och kontrollera att kryssrutan hello **kopiera objekt vid behov** och hello kryssrutan för mål-kontrolleras och tryck sedan på **OK**.

    ![][2]
5. Öppna hello **Build Phases** fliken och i hello **länka binär med bibliotek** menyn Lägg till hello ramverk enligt nedan:

    ![][3]
6. Gå tillbaka toohello Azure-portalen via appsidan **anslutningsinformation** sida och kopiera hello anslutningssträngen.

    ![][4]
7. Lägg till följande kodrad i hello din **AppDelegate.m** fil.

        #import "EngagementAgent.h"
8. Klistra in hello anslutningssträngen i hello `didFinishLaunchingWithOptions` delegera.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
              [...]   
              [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
              [...]
        }
9. `setTestLogEnabled`är ett valfritt uttryck som aktiverar SDK-loggar du tooidentify problem.

## <a id="monitor"></a>Aktivera realtidsövervakning
Du måste skicka minst en skärm (aktivitet) toohello Mobile Engagement-serverdelen i ordning toostart skicka data och se till att hello användarna är aktiva.

1. Öppna hello **ViewController.h** och importera **EngagementViewController.h**:

    `#import "EngagementViewController.h"`
2. Ersätt nu hello superklassen för hello **ViewController** gränssnittet med `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

## <a id="monitor"></a>Anslut appen med realtidsövervakning
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen
Mobile Engagement kan du toointeract med användarna och räckvidd med push-meddelanden och appmeddelanden i hello kontext kampanjer. Denna modul är heter REACH i hello Mobile Engagement-portalen.
hello följande avsnitt konfigurerar din app tooreceive dem.

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a>Aktivera din app tooreceive tysta Push-meddelanden
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-hello-reach-library-tooyour-project"></a>Lägga till hello Reach-biblioteket tooyour projekt
1. Högerklicka på ditt projekt.
2. Välj **Lägg till fil i**.
3. Navigera toohello mapp där du extraherade hello SDK.
4. Välj hello `EngagementReach` mapp.
5. Klicka på **Lägg till**.

### <a name="modify-your-application-delegate"></a>Ändra programdelegaten
1. Tillbaka i **AppDeletegate.m** fil, importera hello Engagement Reach-modulen.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>
2. I hello `application:didFinishLaunchingWithOptions` metoden skapar du en räckviddsmodul och skickar den tooyour befintliga initieringsrad:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

### <a name="enable-your-app-tooreceive-apns-push-notifications"></a>Aktivera din app tooreceive APN Push-meddelanden
1. Lägg till följande rad toohello hello `application:didFinishLaunchingWithOptions` metoden:

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
2. Lägg till hello `application:didRegisterForRemoteNotificationsWithDeviceToken` metoden på följande sätt:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
             [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }
3. Lägg till hello `didFailToRegisterForRemoteNotificationsWithError` metoden på följande sätt:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           NSLog(@"Failed tooget token, error: %@", error);
        }
4. Lägg till hello `didReceiveRemoteNotification:fetchCompletionHandler` metoden på följande sätt:

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
