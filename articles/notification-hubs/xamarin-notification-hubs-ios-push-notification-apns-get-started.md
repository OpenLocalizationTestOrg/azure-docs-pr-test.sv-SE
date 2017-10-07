---
title: "aaaiOS Push-meddelanden med Notification Hubs för Xamarin-appar | Microsoft Docs"
description: "I kursen får du lära dig hur toouse Azure Notification Hubs toosend push-meddelanden tooa Xamarin iOS-program."
services: notification-hubs
keywords: "push-meddelanden för ios, push-meddelanden, push-aviseringar, push-avisering"
documentationcenter: xamarin
author: ysxu
manager: erikre
editor: 
ms.assetid: 4d4dfd42-c5a5-4360-9d70-7812f96924d2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 8db60338047dd53074b4d3d4bb127aa6d9f13a25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>iOS-pushmeddelanden med Notification Hubs för Xamarin-appar
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Översikt
> [!IMPORTANT]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).
> 
> 

Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooan iOS-program.
Du skapar en tom Xamarin.iOS-app som tar emot push-meddelanden med hjälp av hello [Apple Push Notification Service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html). När du är klar, kommer du att kunna toouse din notification hub toobroadcast push-meddelanden tooall hello enheter som kör appen. hello färdiga koden finns i hello [NotificationHubs app] [ GitHub] exempel.

Den här kursen visar hello enkla push meddelandescenario för sändning med Notification Hubs.

## <a name="prerequisites"></a>Krav
Den här kursen kräver hello följande:

* [Xcode 6.0][Install Xcode]
* En enhet som är kompatibel med iOS 7.0 (eller senare version)
* Medlemskap i ett utvecklarprogram för iOS
* [Xamarin Studio]
  
  > [!NOTE]
  > På grund av konfigurationskrav för iOS push-meddelanden, måste du distribuera och testa hello exempelprogrammet på en fysisk iOS-enhet (iPhone eller iPad) i stället för i simulatorn hello.
  > 
  > 

Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Notification Hubs för Xamarin-iOS-appar.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub"></a>Konfigurera meddelandehubben
Det här avsnittet vägleder dig genom att skapa en ny meddelandehubb och konfigurerar autentisering med APNS med hello **.p12** push-certifikat som du skapade. Om du vill toouse en meddelandehubb som du redan har skapat kan du hoppa över toostep 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li>

<p>Vi vill tooconfigure hello APN-anslutningen i hello Azure-portalen, öppna inställningarna för Meddelandehubben, hubs och klicka på <b>Notification Services</b>, och klicka sedan på hello <b>Apple (APNS)</b> objekt i listan om hello. När du har gjort, klicka på <b>överför certifikat</b> och välj hello <b>.p12</b> certifikat som du exporterade tidigare, samt hello lösenordet för hello certifikatet.</p>

<p>Se till att tooselect <b>Sandbox</b> läge eftersom du kommer att skicka push-meddelanden i en utvecklingsmiljö. Använd bara hello <b>produktion</b> inställningen om du vill toosend push-meddelanden toousers som redan har köpt din app hello butiken.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)

Din meddelandehubb är nu konfigurerad toowork med APNS och du har hello anslutning strängar tooregister din app och skicka push-meddelanden.

## <a name="connect-your-app-toohello-notification-hub"></a>Ansluta din app toohello notification hub
#### <a name="create-a-new-project"></a>Skapa ett nytt projekt
1. Skapa ett nytt iOS-projekt i Xamarin Studio och välj hello **enhetligt API** > **program enkel vy** mall.
   
     ![Xamarin Studio – Välj apptyp][31]
2. Lägg till en referens toohello Azure Messaging komponent. I hello vyn lösning högerklickar du på hello **komponenter** för ditt projekt och välj **få fler komponenter**. Sök efter hello **Azure Messaging** komponenten och Lägg till hello komponenten tooyour projekt.
3. I **AppDelegate.cs**, lägga till hello följande med instruktionen:
   
        using WindowsAzure.Messaging;
4. Deklarera en instans av **SBNotificationHub**:
   
        private SBNotificationHub Hub { get; set; }
5. Skapa en **Constants.cs** klass med hello följande variabler:
   
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
6. I **AppDelegate.cs**, uppdatera **FinishedLaunching()** toomatch hello följande:
   
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());
   
                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }
   
            return true;
        }
7. Åsidosätt hello **RegisteredForRemoteNotifications()** metod i **AppDelegate.cs**:
   
        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);
   
            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }
   
                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }
8. Åsidosätt hello **ReceivedRemoteNotification()** metod i **AppDelegate.cs**:
   
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
9. Skapa följande hello **ProcessNotification()** metod i **AppDelegate.cs**:
   
        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check toosee if hello dictionary has hello aps key.  This is hello notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get hello aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
   
                string alert = string.Empty;
   
                //Extract hello alert text
                // NOTE: If you're using hello simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from hello aps dictionary will be another NSDictionary.
                // Basically hello JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();
   
                //If this came from hello ReceivedRemoteNotification while hello app was running,
                // we of course need toomanually process things like hello sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }
   
   > [!NOTE]
   > Du kan välja toooverride **FailedToRegisterForRemoteNotifications()** toohandle situationer, till exempel någon nätverksanslutning. Detta är särskilt viktigt där hello användaren kan starta appen i offline-läge (t.ex. Flygplansläge) och du vill toohandle push-meddelanden scenarier specifika tooyour app.
   > 
   > 
10. Kör hello app på enheten.

## <a name="sending-push-notifications"></a>Skicka push-meddelanden
Du kan testa att ta emot push-meddelanden i appen genom att skicka meddelanden i hello [Azure Portal] via hello **prova att skicka** funktionen hello **felsökning** verktygsuppsättningen, höger hello notification hub på sidan som visas i hello-skärmen nedan.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Push-meddelanden skickas vanligtvis via en serverdelstjänst som Mobile Services eller ASP.NET med hjälp av ett kompatibelt bibliotek. Du kan också använda hello REST API direkt toosend push-meddelanden om ett bibliotek inte är tillgängligt i ditt scenario. 

I den här självstudiekursen kommer vi enkelhet och hur du testar klientappen genom att skicka meddelanden med hello .NET SDK för meddelandehubbar i ett konsolprogram i stället för en serverdelstjänst. Vi rekommenderar hello [använda Notification Hubs toopush meddelanden toousers](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) självstudiekurs som hello nästa steg för att skicka meddelanden från en ASP.NET-serverdel. Hello följande metoder kan dock användas för att skicka meddelanden:

* **REST-gränssnittet**: du kan använda push-meddelanden på alla backend-plattformar med hello [REST-gränssnittet](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **Microsoft Azure Notification Hubs .NET SDK**: hello Nuget Package Manager för Visual Studio, köra [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* **Node.js** : [hur toouse Notification Hubs från Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

**Mobile Apps**: ett exempel på hur toosend meddelanden från en serverdel för Azure Apptjänst Mobilappar som är integrerad med Notification Hubs finns [Lägg till push-meddelanden tooyour mobilappen](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java / PHP**: ett exempel på hur hello toosend push-meddelanden med hjälp av REST API: er, se ”hur toouse Notification Hubs från Java/PHP” ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

#### <a name="optional-send-push-notifications-from-a-net-console-app"></a>(Valfritt) Skicka push-meddelanden från en .NET-konsolapp
I det här avsnittet skickar du meddelanden med en .NET-konsolapp. Hello enligt det här exemplet växlar vi tooa Windows-utvecklingsmiljö där Visual Studio som redan har installerats.

1. Skapa en ny Visual C#-konsolapp i Visual Studio:
   
       ![Visual Studio - Create a new console application][213]
2. I Visual Studio klickar du på **Verktyg**, **NuGet Package Manager** och sedan på **Package Manager-konsolen**.
   
    Hej pakethanterarkonsolen bör visas dockad toohello längst ned i Visual Studio-arbetsytan.
3. Hello fönstret Package Manager-konsolen, ange hello **standardprojektet** tooyour nya konsolen projektet och sedan köra hello följande kommando i konsolfönstret hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Detta lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-paketet</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Öppna hello `Program.cs` och Lägg till följande hello `using` instruktionen, se till att du kan använda Azure-klasser och funktioner i huvudklassen:
   
        using Microsoft.Azure.NotificationHubs;
5. I din `Program` klassen, Lägg till följande metod hello (Glöm inte tooreplace hello **anslutningssträngen** och **hubbnamn**):
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }
6. Lägg till följande rader i hello din `Main` metoden:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Tryck på hello F5 viktiga toorun hello app. Inom några sekunder bör du se ett push-meddelande på enheten. Om du använder Wi-Fi eller ett mobilt nätverk, se till att du har en aktiv Internetanslutning på hello enhet.

Du hittar alla möjliga nyttolaster för hello i hello Apple [Push Notification Programmeringsguide för lokala och].

#### <a name="optional-send-notifications-from-a-mobile-service"></a>(Valfritt) Skicka meddelanden med en mobiltjänst
I det här avsnittet skickar du push-meddelanden med en mobiltjänst via ett nodskript.

Följ toosend ett meddelande med hjälp av en Mobiltjänst [komma igång med Mobile Services], och sedan:

1. Logga in toohello [klassiska Azure-portalen], och välj mobiltjänst.
2. Välj hello **Scheduler** fliken hello längst upp.
   
       ![Azure Classic Portal - Scheduler][215]
3. Skapa ett nytt schemalagt jobb, infoga ett namn och välj **På begäran**.
   
       ![Azure Classic Portal - Create new job][216]
4. Klicka på hello Jobbnamnet när hello jobb skapas. Klicka på hello **skriptet** fliken på hello översta raden.
5. Infoga följande skript i schemaläggarfunktionen hello. Se till att tooreplace hello-platshållare med notification hub namn och hello anslutningssträngen för *DefaultFullSharedAccessSignature* som du fick tidigare. Klicka på **Spara**.
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                  "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );
6. Klicka på **kör en gång** på hello nedre fältet. Du bör få en avisering på enheten.

## <a name="next-steps"></a>Nästa steg
I det här enkla exemplet skickade du push-meddelanden tooall iOS-enheter. I ordning tootarget specifika användare finns i självstudiekursen toohello [använda Notification Hubs toopush meddelanden toousers]. Om du vill att toosegment användarna efter intressegrupper, kan du läsa [använda Notification Hubs toosend senaste nytt]. Läs mer om hur toouse Meddelandehubbar i [riktlinjer om Notification Hubs] och i hello [Meddelandehubbar hur-toofor iOS].

<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[komma igång med Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[klassiska Azure-portalen]: https://manage.windowsazure.com/
[riktlinjer om Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Meddelandehubbar hur-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[använda Notification Hubs toopush meddelanden toousers]: /manage/services/notification-hubs/notify-users-aspnet
[använda Notification Hubs toosend senaste nytt]: /manage/services/notification-hubs/breaking-news-dotnet

[Push Notification Programmeringsguide för lokala och]:https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure Portal]: https://portal.azure.com
