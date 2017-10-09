---
title: aaaAdd push-meddelanden tooyour Xamarin.Forms-app | Microsoft Docs
description: "Lär dig hur toouse Azure services toosend flera plattformar push-meddelanden tooyour Xamarin.Forms-appar."
services: app-service\mobile
documentationcenter: xamarin
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: d9b1ba9a-b3f2-4d12-affc-2ee34311538b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: 9133a0b6dd99c01def525607c20ce5a9c19b9502
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-xamarinforms-app"></a>Lägg till push-meddelanden tooyour Xamarin.Forms-app
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Översikt
I kursen får du lägga till push-meddelanden tooall hello projekt som härrör från hello [Xamarin.Forms Snabbstart](app-service-mobile-xamarin-forms-get-started.md). Det innebär att ett push-meddelande skickas tooall plattformsoberoende klienter varje gång en post infogas.

Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande. Mer information finns i [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Krav
För iOS, behöver du en [Apple Developer Program medlemskap](https://developer.apple.com/programs/ios/) och en fysisk iOS-enhet. Hej [stöder inte push-meddelanden i iOS-simulatorn](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

## <a name="configure-hub"></a>Konfigurera en meddelandehubb
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-hello-server-project-toosend-push-notifications"></a>Uppdatera hello server projektet toosend push-meddelanden
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-hello-android-project-optional"></a>Konfigurera och köra hello Android-projekt (valfritt)
Slutför det här avsnittet tooenable push-meddelanden för hello Xamarin.Forms Droid-projektet för Android.

### <a name="enable-firebase-cloud-messaging-fcm"></a>Aktivera Firebase Cloud Messaging (FCM)
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-hello-mobile-apps-back-end-toosend-push-requests-by-using-fcm"></a>Konfigurera hello Mobile Apps serverdel toosend push begäranden med hjälp av FCM
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-toohello-android-project"></a>Lägg till push-meddelanden toohello Android-projekt
Hello serverdel konfigurerats med FCM, kan du lägga till komponenter och koder toohello klienten tooregister med FCM. Du kan också registrera för push-meddelanden med Azure Notification Hubs via hello Mobile Apps tillbaka avslutas och ta emot meddelanden.

1. I hello **Droid** projekt, högerklicka på hello **komponenter** mappen och klicka på **få fler komponenter...** . Sök sedan efter hello **Google Cloud Messaging-klienten** komponenten och Lägg till den toohello projekt. Den här komponenten stöder push-meddelanden för en Xamarin Android-projekt.
2. Öppna projektfilen för hello MainActivity.cs och Lägg till följande instruktion hello överst i filen hello hello:

        using Gcm.Client;
3. Lägg till följande kod toohello hello **OnCreate** efter hello anrop för**LoadApplication**:

        try
        {
            // Check tooensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating hello client. Verify hello URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }
4. Lägg till en ny **CreateAndShowDialog** helper-metoden på följande sätt:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }
5. Lägg till följande kod toohello hello **MainActivity** klass:

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return hello current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    Detta visar hello aktuella **MainActivity** instans, så vi kan köra på hello huvudsakliga UI-tråden.
6. Initiera hello `instance` variabeln hello början av hello **OnCreate** metoden på följande sätt.

        // Set hello current instance of MainActivity.
        instance = this;
7. Lägg till en ny klass filen toohello **Droid** projektet med namnet `GcmService.cs`, och se till att hello följande **med** instruktioner finns hello överst i filen hello:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;
8. Lägg till följande behörighetsbegäranden hello överst i hello filen efter hello hello **med** instruktioner och innan hello **namnområde** deklaration.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
9. Lägg till hello efter klass definition toohello namnområde.

       [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
       public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
       {
           public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
       }

   > [!NOTE]
   > Ersätt **< PROJECT_NUMBER >** med din projektnumret som du antecknade tidigare.    
   >
   >
10. Ersätt hello tom **GcmService** klassen med följande kod, som använder hello nya broadcast mottagaren hello:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }
11. Lägg till följande kod toohello hello **GcmService** klass. Detta åsidosätter hello **OnRegistered** händelsehanteraren och implementerar en **registrera** metod.

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

    Observera att den här koden använder hello `messageParam` parameter i hello mallen registrering.
12. Lägg till följande kod som implementerar hello **OnMessage**:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store hello message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent tooshow ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create hello notification
            //we use hello pending intent, passing our ui intent over which will get called
            //when hello notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set hello notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove hello notification once hello user touches it
                    .SetAutoCancel(true).Build();

            //Show hello notification
            notificationManager.Notify(1, notification);
        }

    Detta hanterar inkommande meddelanden och skickar dem toohello notification manager toobe visas.
13. **GcmServiceBase** måste du även tooimplement hello **OnUnRegistered** och **VidFel** hanterare metoder som du kan göra följande:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Nu kan du är klar testa push-meddelanden i hello-app som körs på en Android-enhet eller hello emulatorn.

### <a name="test-push-notifications-in-your-android-app"></a>Testa push-meddelanden i din Android-app
hello två första stegen krävs bara när du testar på en emulator.

1. Se till att du distribuerar tooor felsökning på en virtuell enhet som har Google APIs som hello mål, enligt nedan i hello Android Virtual Device manager.
2. Lägg till en Google-konto toohello Android-enhet genom att klicka på **appar** > **inställningar** > **Lägg till konto**. Följ sedan hello prompter tooadd en befintlig enhet för Google-konto toohello eller toocreate en ny.
3. Högerklicka i Visual Studio eller Xamarin Studio hello **Droid** projektet och klicka på **Ställ in som Startprojekt**.
4. Klicka på **kör** toobuild hello projektet och starta hello appen på din Android-enhet eller emulator.
5. Skriv en aktivitet i hello app och klicka på hello plus (**+**) ikon.
6. Kontrollera att ett meddelande tas emot när ett objekt läggs till.

## <a name="configure-and-run-hello-ios-project-optional"></a>Konfigurera och köra hello iOS-projektet (valfritt)
Det här avsnittet handlar om att köra hello Xamarin iOS-projektet för iOS-enheter. Du kan hoppa över det här avsnittet om du inte arbetar med iOS-enheter.

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-hello-notification-hub-for-apns"></a>Konfigurera hello meddelandehubb för APNS
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Därefter konfigurerar du hello inställningen för iOS-projekt i Xamarin Studio eller Visual Studio.

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-tooyour-ios-app"></a>Lägg till push-meddelanden tooyour iOS-app
1. I hello **iOS** projekt, öppna AppDelegate.cs och Lägg till följande instruktion toohello överkant hello kodfilen hello.

        using Newtonsoft.Json.Linq;
2. I hello **AppDelegate** klassen, lägga till en åsidosättning för hello **RegisteredForRemoteNotifications** händelse tooregister för meddelanden:

        public override void RegisteredForRemoteNotifications(UIApplication application,
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }
3. I **AppDelegate**, även lägga till följande åsidosättning för hello hello **DidReceiveRemoteNotification** händelsehanteraren:

        public override void DidReceiveRemoteNotification(UIApplication application,
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Den här metoden hanterar inkommande meddelanden när hello appen körs.
4. I hello **AppDelegate** klassen och Lägg till följande kod toohello hello **FinishedLaunching** metod:

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Detta möjliggör stöd för remote meddelanden och att registrera push-begäranden.

Appen är nu uppdaterade toosupport push-meddelanden.

#### <a name="test-push-notifications-in-your-ios-app"></a>Testa push-meddelanden i iOS-app
1. Högerklicka på hello iOS-projektet och klicka på **Set as StartUp Project**.
2. Tryck på hello **kör** knappen eller **F5** i Visual Studio toobuild hello projektet och starta hello app i en iOS-enhet. Klicka på **OK** tooaccept push-meddelanden.

   > [!NOTE]
   > Du måste uttryckligen godkänna push-meddelanden från din app. Denna begäran inträffar bara hello första gången som hello appen körs.
   >
   >
3. Skriv en aktivitet i hello app och klicka på hello plus (**+**) ikon.
4. Kontrollera att ett meddelande tas emot och klicka sedan på **OK** toodismiss hello-meddelande.

## <a name="configure-and-run-windows-projects-optional"></a>Konfigurera och köra Windows projekt (valfritt)
Det här avsnittet handlar om att köra hello Xamarin.Forms, WinApp och WinPhone81 projekt för Windows-enheter. De här stegen stöder också universella Windowsplattformen (UWP)-projekt. Du kan hoppa över det här avsnittet om du inte arbetar med Windowsenheter.

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a>Registrera din Windows-app för push-meddelanden med Windows Notification Service (WNS)
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-hello-notification-hub-for-wns"></a>Konfigurera hello meddelandehubb för WNS
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-tooyour-windows-app"></a>Lägg till push-meddelanden tooyour Windows-app
1. Öppna i Visual Studio **App.xaml.cs** i en Windows projekt och Lägg till hello följande instruktioner.

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Ersätt `<your_TodoItemManager_portable_class_namespace>` med hello namnområde för din bärbara projekt som innehåller hello `TodoItemManager` klass.
2. I App.xaml.cs lägger du till följande hello **InitNotificationsAsync** metod:

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS =
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Den här metoden hämtar hello push notification channel och registrerar en mall tooreceive mall meddelanden från din meddelandehubb. Ett meddelande i mallen som stöder *messageParam* levereras toothis klienten.
3. Uppdatera hello i App.xaml.cs **OnLaunched** händelsehanterare metoddefinition genom att lägga till hello `async` modifierare. Lägg sedan till följande kodrad hello slutet av metoden hello hello:

        await InitNotificationsAsync();

    Detta säkerställer att hello registreringen av push-meddelanden skapas eller uppdateras varje gång hello appen startas. Det är viktigt toodo denna tooguarantee som hello WNS push kanal är alltid aktivt.  
4. I Solution Explorer för Visual Studio, öppna hello **Package.appxmanifest** fil och anger **popup-kompatibel** för**Ja** under **meddelanden**.
5. Skapa hello app och kontrollera att du har några fel. Klientappen bör nu registrera för hello mallen meddelanden från hello Mobile Apps tillbaka avslutas. Upprepa det här avsnittet för varje Windows-projekt i din lösning.

#### <a name="test-push-notifications-in-your-windows-app"></a>Testa push-meddelanden i Windows-appen
1. Högerklicka på en Windows-projekt i Visual Studio och klicka på **Ställ in som Startprojekt**.
2. Tryck på hello **kör** knappen toobuild hello projektet och starta hello app.
3. Skriv ett namn för en ny todoitem i hello app och klicka sedan på hello plus (**+**) ikonen tooadd den.
4. Kontrollera att ett meddelande tas emot när hello-objektet har lagts till.

## <a name="next-steps"></a>Nästa steg
Mer information om push-meddelanden:

* [Diagnostisera problem för push-meddelande](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Det finns olika orsaker till varför meddelanden kan hämta bort eller inte hamnar på enheter. Det här avsnittet visas hur tooanalyze och ta reda på hello rot orsakar fel för push-meddelande.

Du kan också fortsätta på tooone av hello följande kurser:

* [Lägg till autentisering tooyour app](app-service-mobile-xamarin-forms-get-started-users.md)  
  Lär dig hur tooauthenticate användare i appen med en identitetsprovider.
* [Aktivera offlinesynkronisering av appen](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Lär dig hur tooadd offline stöd för din app genom att använda en Mobile Apps-serverdel. Med offlinesynkronisering, användare kan interagera med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
