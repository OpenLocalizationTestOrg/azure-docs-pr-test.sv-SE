---
title: "aaaGet igång med Azure Mobile Engagement för Xamarin.iOS"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och Push-meddelanden för Xamarin.iOS-appar."
services: mobile-engagement
documentationcenter: xamarin
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0448209e-fff6-47bd-985c-2cf074bac12f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 02340a744753dcc5cd1b6888a5fa87628be47b68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Kom igång med Azure Mobile Engagement för Xamarin.iOS-appar
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand din app användnings- och skicka push-meddelanden toosegmented användare i ett Xamarin.iOS-program.
I den här kursen får du skapa en tom Xamarin.iOS-app som samlar in grundläggande data och tar emot push-meddelanden via Apple Push Notification System (APNS).

> [!NOTE]
> hello Azure Mobile Engagement-tjänsten kommer att dras tillbaka mars 2018 och är för närvarande bara tillgänglig tooexisting kunder. Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Den här kursen kräver hello följande:

* [Xamarin Studio](http://xamarin.com/studio). Du kan även använda Visual Studio med Xamarin men i den här kursen används Xamarin Studio. Det finns installationsanvisningar i avsnittet om [konfiguration och installation för Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 
* [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).
> 
> 

## <a id="setup-azme"></a>Konfigurera Mobile Engagement för din iOS-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen
Den här kursen behandlas en ”grundläggande integration” som hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande.

Vi skapar en grundläggande app i Xamarin toodemonstrate hello integrering:

### <a name="create-a-new-xamarinios-project"></a>Skapa ett nytt Xamarin.iOS-projekt
1. Starta Xamarin Studio. Gå för**filen** -> **ny** -> **lösning** 
   
    ![][1]
2. Välj **enda visa App**, kontrollera att hello valda språket är **C#** och klicka sedan på **nästa**.
   
    ![][2]
3. Fyll i hello **Appnamn** och hello **organisations-ID** och klicka sedan på **nästa**. 
   
    ![][3]
   
   > [!IMPORTANT]
   > Se till att hello Publicera profil som du använder toodeploy iOS-appen använder ett App-ID som matchar exakt med hello paket-ID som du har här. 
   > 
   > 
4. Uppdatera hello **projektnamn**, **lösningsnamn** och **plats** om krävs och klicka på **skapa**.
   
    ![][4]

Xamarin Studio skapar hello demoappen där Mobile Engagement ska integreras. 

### <a name="connect-your-app-toomobile-engagement-backend"></a>Ansluta appen tooMobile Engagement-serverdelen
1. Högerklicka på hello **paket** mapp i hello Lösningsfönstret och välj **lägga till paket...**
   
    ![][5]
2. Sök efter hello **Microsoft Azure Mobile Engagement Xamarin SDK** och lägga till den tooyour lösning.  
   
    ![][6]
3. Öppna **AppDelegate.cs** och Lägg till hello följande med instruktionen:
   
        using Microsoft.Azure.Engagement.Xamarin;
4. I hello **FinishedLaunching** metod, lägga till hello följande tooinitialize hello anslutningen till Mobile Engagement-serverdelen. Se till att tooadd din **ConnectionString**. Den här koden används även en **NotificationIcon** som läggs till av hello Mobile Engagement SDK kan tooreplace. 
   
        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

## <a id="monitor"></a>Aktivera realtidsövervakning
Du måste skicka minst en skärm toohello Mobile Engagement-serverdelen i ordning toostart skicka data och säkerställa hello användarna är aktiva.

1. Öppna **ViewController.cs** och Lägg till hello följande med instruktionen:
   
        using Microsoft.Azure.Engagement.Xamarin;
2. Ersätt hello klassen från vilken `ViewController` ärver från `UIViewController` för`EngagementViewController`. 

## <a id="monitor"></a>Anslut appen med realtidsövervakning
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen
Mobile Engagement kan du toointeract med användarna och räckvidd med push-meddelanden och appmeddelanden i hello kontext kampanjer. Denna modul är heter REACH i hello Mobile Engagement-portalen.
hello följande avsnitt konfigurerar din app tooreceive dem.

### <a name="modify-your-application-delegate"></a>Ändra programdelegaten
1. Öppna hello **AppDelegate.cs** och Lägg till hello följande med instruktionen:
   
        using System; 
2. I hello `FinishedLaunching` metoden Lägg till hello följande tooregister för push-meddelanden efter`EngagementAgent.init(...)`
   
        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }
3. Slutligen - uppdatera eller lägga till hello följande metoder:
   
        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }
   
        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }
   
        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed tooregister for remote notifications: Error '{0}'", error);
        }
4. I din **Info.plist** fil i hello lösning, bekräfta att hello **Paketidentifierare** matchar hello **App-ID** du har i din etableringsprofil i hello Apple Dev Center. 
   
    ![][7]
5. Hej i samma **Info.plist** fil, se till att du har markerat hello **aktivera bakgrundslägen** och **Remote Notifications**. 
   
     ![][8]
6. Köra hello app på hello-enhet som du har associerat med den här publiceringsprofilen. 

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
