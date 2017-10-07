---
title: aaaAdd push-meddelanden tooyour Xamarin.iOS-app med Azure App Service
description: "Lär dig hur toouse Azure App Service toosend push-meddelanden tooyour Xamarin.iOS-app"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 2921214a-49f8-45e1-a306-a85ce21defca
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: glenga
ms.openlocfilehash: 3e6439aee4f3fe0f60b9786d0bbfd74c4f5e52d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-xamarinios-app"></a>Lägg till push-meddelanden tooyour Xamarin.iOS-App
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Översikt
I kursen får du lägga till push-meddelanden toohello [Xamarin.iOS Snabbstart](app-service-mobile-xamarin-ios-get-started.md) projekt så att ett push-meddelande skickas toohello enheten varje gång en post infogas.

Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande. Se [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) för mer information.

## <a name="prerequisites"></a>Krav
* Fullständig hello [Xamarin.iOS quickstart](app-service-mobile-xamarin-ios-get-started.md) kursen.
* En fysisk iOS-enhet. Push-meddelanden stöds inte av hello iOS-simulatorn.

## <a name="register-hello-app-for-push-notifications-on-apples-developer-portal"></a>Registrera hello app för push-meddelanden på Apple developer-portalen
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-your-mobile-app-toosend-push-notifications"></a>Konfigurera din Mobilapp toosend push-meddelanden
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-hello-server-project-toosend-push-notifications"></a>Uppdatera hello server projektet toosend push-meddelanden
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-your-xamarinios-project"></a>Konfigurera Xamarin.iOS-projekt
[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push-notifications-tooyour-app"></a>Lägg till push-meddelanden tooyour app
1. I **QSTodoService**, Lägg till följande egenskapen hello så att **AppDelegate** kan hämta hello mobila klienten:
   
            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }
2. Lägg till följande hello `using` instruktionen toohello överkant hello **AppDelegate.cs** fil.
   
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
3. I **AppDelegate**, åsidosätta hello **FinishedLaunching** händelse:
   
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());
   
            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();
   
            return true;
        }
4. I Hej samma fil, åsidosätta hello **RegisteredForRemoteNotifications** händelse. I den här koden registrerar du för en enkel mall-meddelandet som skickas över alla plattformar som stöds av hello-servern.
   
    Mer information om mallar med Notification Hubs finns [mallar](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


1. Sedan kan åsidosätta hello **DidReceivedRemoteNotification** händelse:
   
        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;
   
            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();
   
            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Appen är nu uppdaterade toosupport push-meddelanden.

## <a name="test"></a>Testa push-meddelanden i appen
1. Tryck på hello **kör** knappen toobuild hello projektet och starta hello app i en kompatibla iOS-enhet, och klicka sedan **OK** tooaccept push-meddelanden.
   
   > [!NOTE]
   > Du måste uttryckligen godkänna push-meddelanden från din app. Denna begäran inträffar bara hello första gången som hello appen körs.
   > 
   > 
2. Skriv en aktivitet i hello app och klicka på hello plus (**+**) ikon.
3. Kontrollera att ett meddelande tas emot och klickar sedan **OK** toodismiss hello-meddelande.
4. Upprepa steg 2 och omedelbart Stäng hello appen och sedan kontrollera att ett meddelande visas.

Du har slutfört den här kursen.

<!-- Images. -->

<!-- URLs. -->



