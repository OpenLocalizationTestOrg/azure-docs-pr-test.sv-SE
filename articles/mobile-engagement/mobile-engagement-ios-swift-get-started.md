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
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>Komma igång med Azure Mobile Engagement för iOS-appar i Swift
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand din app användnings- och skicka push-meddelanden toosegmented användare tooan iOS-program.
I den här självstudiekursen skapar du en tom iOS-app som samlar in grundläggande data och tar emot push-meddelanden via Apple Push Notification System (APNS).

Den här kursen kräver hello följande:

* XCode 8, som du kan installera från Mac App Store
* Hej [Mobile Engagement iOS SDK]
* Certifikat för push-meddelanden (.p12) som kan hämtas från Apple Dev Center

> [!NOTE]
> Den här kursen använder Swift version 3.0. 
> 
> 

Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Mobile Engagement och iOS-appar.

> [!NOTE]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).
> 
> 

## <a id="setup-azme"></a>Konfigurera Mobile Engagement för din iOS-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen
Den här kursen behandlas en ”grundläggande integration”, vilket är hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande. hello fullständiga integrationsdokumentationen finns i hello [Mobile Engagement iOS SDK-integration](mobile-engagement-ios-sdk-overview.md)

Vi skapar en grundläggande app i XCode toodemonstrate hello integrering:

### <a name="create-a-new-ios-project"></a>Skapa ett nytt iOS-projekt
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-toomobile-engagement-backend"></a>Ansluta appen tooMobile Engagement-serverdelen
1. Hämta hello [Mobile Engagement iOS SDK]
2. Extrahera hello. tar.gz tooa mapp på datorn
3. Högerklicka på hello projektet och välj ”Lägg till filer för...”
   
    ![][1]
4. Navigera toohello mapp där du extraherade hello SDK och välj hello `EngagementSDK` mappen och tryck på OK.
   
    ![][2]
5. Öppna hello `Build Phases` fliken och i hello `Link Binary With Libraries` menyn Lägg till hello ramverk enligt nedan:
   
    ![][3]
6. Skapa en bryggning huvud toobe kan toouse hello SDK: ns Objective C-API: er genom att välja Arkiv > Nytt > fil > iOS > källa > huvudfil.
   
    ![][4]
7. Redigera hello bryggning sidhuvud filen tooexpose Mobile Engagement Objective-C-koden tooyour Swift-kod, lägga till hello följande importer:
   
        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"
8. Kontrollera att hello Interimshuvudfilen med Objective-C skapa inställningen under Swift-kompilator – kodgenerering har en sökväg toothis rubrik under Skapa inställningar. Här följer ett exempel på sökvägen: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (beroende på hello sökväg)**
   
   ![][6]
9. Gå tillbaka toohello Azure-portalen via appsidan *anslutningsinformation* sida och kopiera hello anslutningssträngen
   
   ![][5]
10. Klistra in hello anslutningssträngen i hello `didFinishLaunchingWithOptions` delegera
    
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
              [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
              [...]
        }

## <a id="monitor"></a>Aktivera realtidsövervakning
Du måste skicka minst en skärm (aktivitet) toohello Mobile Engagement-serverdelen i ordning toostart skicka data och se till att hello användarna är aktiva.

1. Öppna hello **ViewController.swift** filen och ersätter hello basklassen **ViewController** toobe **EngagementViewController**:
   
    `class ViewController : EngagementViewController {`

## <a id="monitor"></a>Anslut appen med realtidsövervakning
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen
Mobile Engagement kan du toointeract och räckvidd med dina användare med Push-meddelanden och meddelanden i appen hello gäller kampanjer. Denna modul är heter REACH i hello Mobile Engagement-portalen.
hello följande avsnitt konfigurerar din app tooreceive dem.

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a>Aktivera din app tooreceive tysta Push-meddelanden
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-hello-reach-library-tooyour-project"></a>Lägga till hello Reach-biblioteket tooyour projekt
1. Högerklicka på ditt projekt
2. Välj `Add file too...`
3. Navigera toohello mapp där du extraherade hello SDK
4. Välj hello `EngagementReach` mapp
5. Klicka på Lägg till
6. Redigera hello bryggning sidhuvud filen tooexpose Mobile Engagement Objective-C nå sidhuvuden och Lägg till hello följande importer:
   
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

### <a name="modify-your-application-delegate"></a>Ändra programdelegaten
1. I hello `didFinishLaunchingWithOptions` – skapar en räckviddsmodul och skickar den tooyour befintliga initieringsrad:
   
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

### <a name="enable-your-app-tooreceive-apns-push-notifications"></a>Aktivera din app tooreceive APN Push-meddelanden
1. Lägg till följande rad toohello hello `didFinishLaunchingWithOptions` metoden:
   
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
2. Lägg till hello `didRegisterForRemoteNotificationsWithDeviceToken` metoden på följande sätt:
   
        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }
3. Lägg till hello `didReceiveRemoteNotification:fetchCompletionHandler:` metoden på följande sätt:
   
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
