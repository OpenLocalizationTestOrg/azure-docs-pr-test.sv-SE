---
title: "aaaGet igång med Notification Hubs för Xamarin.Android-appar | Microsoft Docs"
description: "I kursen får du lära dig hur toouse Azure Notification Hubs toosend push-meddelanden tooa Xamarin Android-program."
author: ysxu
manager: erikre
editor: 
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: c5c7ead9a9381ab9fb6bbe86e930a8a9254813d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Komma igång med Notification Hubs med Xamarin för Android
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Översikt
Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa Xamarin.Android-program.
Du skapar en tom Xamarin.Android-app som tar emot push-meddelanden genom att använda Google Cloud Messaging (GCM). När du är klar, kommer du att kunna toouse din notification hub toobroadcast push-meddelanden tooall hello enheter som kör appen. hello färdiga koden finns i hello [NotificationHubs app] [ GitHub] exempel.

Den här kursen visar hello enkelt scenario för sändning med Notification Hubs.

## <a name="before-you-begin"></a>Innan du börjar
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

hello slutförts koden för den här kursen finns på GitHub [här](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).

## <a name="prerequisites"></a>Krav
Den här kursen kräver hello följande:

* Visual Studio med Xamarin i Windows eller Xamarin Studio i Mac OS X. Fullständiga installationsanvisningar finns i avsnittet [Konfiguration och installation av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
* Aktivt Google-konto
* [Azure Messaging-komponent]
* [Google Cloud Messaging-klientkomponent]

Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Notification Hubs för Xamarin.Android-appar.

> [!IMPORTANT]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den kostnadsfria utvärderingsversionen av Azure finns [Kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).
> 
> 

## <a name="enable-google-cloud-messaging"></a>Aktivera Google Cloud Messaging
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a>Konfigurera meddelandehubben
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li><p>Klicka på hello <b>konfigurera</b> hello överst och anger hello <b>API-nyckel</b> du fick i hello föregående avsnitt och klickar sedan på <b>spara</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)

Din meddelandehubb har nu konfigurerat toowork med GCM och du har hello anslutning strängar tooboth registrera din app tooreceive meddelanden och toosend push-meddelanden.

## <a name="connect-your-app-toohello-notification-hub"></a>Ansluta din app toohello notification hub
### <a name="create-a-new-project"></a>Skapa ett nytt projekt
1. I Xamarin Studio klickar du på **Ny lösning**, sedan på **Android-app** och till sist på **Nästa**.
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Ange **appens namn** och **ID**. Klicka på hello **målplattformar** du toosupport och klickar sedan på **nästa** och **skapa**.
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)

    På så sätt skapas ett nytt Android-projekt.

1. Öppna hello projektegenskaperna genom att högerklicka på det nya projektet i hello lösning visa och välja **alternativ**. Välj hello **Android-program** objekt i hello **skapa** avsnitt.
   
    Se till att den första bokstaven hello i din **paketnamnet** gemena.
   
   > [!IMPORTANT]
   > hello första bokstaven i paketnamnet hello måste vara gemener. Annars visas appmanifestfel när du registrerar **BroadcastReceiver** och **IntentFilter** för push-meddelanden nedan.
   > 
   > 
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)
2. Du kan också ange hello **lägsta Android-versionen** tooanother API-nivå.
3. Du kan också ange hello **Målversionen av Android** toohello en annan API-versionen som du vill tootarget (måste vara API-nivå 8 eller senare).

Klicka på **OK** och Stäng hello Projektalternativ.

### <a name="add-hello-required-components-tooyour-project"></a>Lägga till hello nödvändiga komponenter tooyour projekt
hello Google Cloud Messaging-klienten på hello Xamarin-Komponentlagret gör hello enklare att stödja push-meddelanden i Xamarin.Android.

1. Högerklicka på mappen för hello komponenter i Xamarin.Android-appen och välj **få fler komponenter**.
2. Sök efter hello **Azure Messaging** komponenten och Lägg till den toohello projekt.
3. Sök efter hello **Google Cloud Messaging-klienten** komponenten och Lägg till den toohello projekt.

### <a name="set-up-notification-hubs-in-your-project"></a>Konfigurera meddelandehubbar i projektet
1. Samla in följande information för din Android-appen och meddelandehubben hello:
   
   * **Google-projektnummer**: hämta värdet för den här projektnumret från hello översikten över appen på hello Google Developer-portalen. Du antecknade värdet tidigare när du skapade hello app på hello-portalen.
   * **Lyssna anslutningssträng**: på hello instrumentpanelen i hello [klassiska Azure-portalen], klickar du på **visa anslutningssträngar**. Kopiera hello *DefaultListenSharedAccessSignature* anslutningssträngen för det här värdet.
   * **Hubbnamn**: är hello namnet på hubben från hello [klassiska Azure-portalen]. Till exempel *mynotificationhub2*.
     
     Skapa en **Constants.cs** klass för Xamarin-projektet och definiera följande konstanta värden i hello klass hello. Ersätt hello platshållarna med värdena.
     
       offentlig statisk klass Konstanter   {
     
           public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
       }
2. Lägg till hello följande using-instruktioner för**MainActivity.cs**:
   
        using Android.Util;
        using Gcm.Client;
3. Lägg till en variabel toohello instans `MainActivity` klass som ska använda tooshow en varningsruta när hello appen körs:
   
        public static MainActivity instance;
4. Skapa följande metod i hello hello **MainActivity** klass:
   
        private void RegisterWithGCM()
        {
            // Check tooensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);
   
            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }
5. I hello `OnCreate` metod för **MainActivity.cs**, initiera hello `instance` variabeln och Lägg till ett anrop för`RegisterWithGCM`:
   
        protected override void OnCreate (Bundle bundle)
        {
            instance = this;
   
            base.OnCreate (bundle);
   
            // Set your view from hello "main" layout resource
            SetContentView (Resource.Layout.Main);
   
            // Get your button from hello layout resource,
            // and attach an event tooit
            Button button = FindViewById<Button> (Resource.Id.myButton);
   
            RegisterWithGCM();
        }
6. Skapa en ny klass, **MyBroadcastReceiver**.
   
   > [!NOTE]
   > Nedan går vi igenom hur du skapar en **BroadcastReceiver**-klass från grunden. Dock skapa en snabb alternativa toomanually **MyBroadcastReceiver.cs** är toorefer toohello **GcmService.cs** filen i hello Xamarin.Android exempelprojektet medföljer hello [NotificationHubs exempel][GitHub]. Duplicera **GcmService.cs** och ändra klassnamn kan vara en bra toostart samt.
   > 
   > 
7. Lägg till hello följande using-instruktioner för**MyBroadcastReceiver.cs** (hänvisar toohello komponenten och sammansättningen som du lade till tidigare):
   
        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;
8. I **MyBroadcastReceiver.cs**, Lägg till följande behörighetsbegäranden mellan hello hello **med** -satser och hello **namnområde** deklaration:
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
9. I **MyBroadcastReceiver.cs**, ändra hello **MyBroadcastReceiver** klassen toomatch hello följande:
   
        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };
   
            public const string TAG = "MyBroadcastReceiver-GCM";
        }
10. Lägg till ytterligare en klass i **MyBroadcastReceiver.cs** med namnet **PushHandlerService**, som kommer från **GcmServiceBase**. Se till att tooapply hello **Service** attributet toohello klass:
    
         [Service] // Must use hello service tag
         public class PushHandlerService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }
             private NotificationHub Hub { get; set; }
    
             public PushHandlerService() : base(Constants.SenderID)
                {
                 Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
             }
         }
11. **GcmServiceBase** implementerar metoderna **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()** och **OnError()**. Vår **PushHandlerService** implementeringsklass måste åsidosätta de här metoderna och metoderna utlöses som svar toointeracting hello meddelandehubben.
12. Åsidosätt hello **OnRegistered()** metod i **PushHandlerService** med hjälp av hello följande kod:
    
         protected override void OnRegistered(Context context, string registrationId)
         {
             Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
             RegistrationID = registrationId;
    
             createNotification("PushHandlerService-GCM Registered...",
                                 "hello device has been Registered!");
    
             Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                         context);
             try
             {
                 Hub.UnregisterAll(registrationId);
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
    
             //var tags = new List<string>() { "falcons" }; // create tags if you want
             var tags = new List<string>() {};
    
             try
             {
                 var hubRegistration = Hub.Register(registrationId, tags.ToArray());
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
         }
    
    > [!NOTE]
    > I hello **OnRegistered()** code ovan, Observera hello möjlighet toospecify taggar tooregister för vissa meddelandekanaler.
    > 
    > 
13. Åsidosätt hello **OnMessage** metod i **PushHandlerService** med hjälp av hello följande kod:
    
        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");
    
            var msg = new StringBuilder();
    
            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }
    
            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }
14. Lägg till följande hello **createNotification** och **dialogNotify** metoder för**PushHandlerService** för att meddela användaren när ett meddelande tas emot.
    
    > [!NOTE]
    > Meddelandedesign i Android-versionen 5.0 eller senare skiljer sig avsevärt från tidigare versioner. Om du testar detta i Android 5.0 eller senare, behöver hello app toobe kör tooreceive hello-meddelande. Mer information om Android-meddelanden finns [här](http://go.microsoft.com/fwlink/?LinkId=615880).
    > 
    > 
    
        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;
    
            //Create an intent tooshow UI
            var uiIntent = new Intent(this, typeof(MainActivity));
    
            //Create hello notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);
    
            //Auto-cancel will remove hello notification once hello user touches it
            notification.Flags = NotificationFlags.AutoCancel;
    
            //Set hello notification info
            //we use hello pending intent, passing our ui intent over, which will get called
            //when hello notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));
    
            //Show hello notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }
    
        protected void dialogNotify(String title, String message)
        {
    
            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }
15. Åsidosätt de abstrakta medlemmarna **OnUnRegistered()**, **OnRecoverableError()** och **OnError()** så att din kod kompileras:
    
        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);
    
            createNotification("GCM Unregistered...", "hello device has been unregistered!");
        }
    
        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);
    
            return base.OnRecoverableError (context, errorId);
        }
    
        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }

## <a name="run-your-app-in-hello-emulator"></a>Kör appen i emulatorn hello
Om du kör den här appen i emulatorn hello, se till att du använder en Android Virtual Device (AVD) som har stöd för Google APIs.

> [!IMPORTANT]
> I ordning tooreceive push-meddelanden, måste du ställa in ett Google-konto på din Android-enhet. (I hello emulatorn navigerar för**inställningar** och på **Lägg till konto**.) Kontrollera också att hello-emulatorn är anslutna toohello Internet.
> 
> [!NOTE]
> Meddelandedesign i Android-versionen 5.0 eller senare skiljer sig avsevärt från tidigare versioner. Mer information om Android-meddelanden finns [här](http://go.microsoft.com/fwlink/?LinkId=615880).
> 
> 

1. I **Verktyg** klickar du på **Open Android Emulator Manager** (Öppna hanteraren för Android-emulator), väljer enheten och klickar sedan på **Redigera**.
   
      ![][18]
2. Välj **Google APIs** (API:er för Google) i **Mål** och klicka på **OK**.
   
      ![][19]
3. På hello översta verktygsfältet **kör**, och välj sedan din app. Detta startar hello-emulatorn och kör hello-app.
   
   hello appen hämtar hello *registrationId* från GCM och registreras med hello notification hub.

## <a name="send-notifications-from-your-backend"></a>Skicka meddelanden från serverdelen
Du kan testa att ta emot meddelanden i appen genom att skicka meddelanden i hello [klassiska Azure-portalen] via hello debug fliken på hello meddelandehubb som visas i hello-skärmen nedan.

![][30]

Push-meddelanden skickas vanligtvis via en serverdelstjänst som Mobile Services eller ASP.NET med hjälp av ett kompatibelt bibliotek. Du kan också använda hello REST API direkt toosend meddelande meddelanden om ett bibliotek inte är tillgänglig för din serverdel.

Här följer en lista med andra självstudiekurser som du kan ha tooreview för att skicka meddelanden:

* ASP.NET: Se [använda Notification Hubs toopush meddelanden toousers].
* Azure Notification Hubs Java SDK: se [hur toouse Notification Hubs från Java](notification-hubs-java-push-notification-tutorial.md) för att skicka meddelanden från Java. Det här har testats i Eclipse för Android-utveckling.
* PHP: Se [hur toouse Notification Hubs från PHP](notification-hubs-php-push-notification-tutorial.md).

I hello nästa underavsnitt i självstudiekursen hello skickar du meddelanden med hjälp av en .NET-konsolapp och genom att använda en Mobiltjänst via ett nod-skript.

#### <a name="optional-send-notifications-by-using-a-net-app"></a>(Valfritt) Skicka meddelanden med hjälp av en .NET-app
I det här avsnittet skickar du meddelanden med hjälp av en .NET-konsolapp

1. Skapa en ny Visual C#-konsolapp:
   
      ![][20]
2. I Visual Studio klickar du på **Verktyg**, **NuGet Package Manager** och sedan på **Package Manager-konsolen**.
   
    Detta visar hello Package Manager-konsolen i Visual Studio.
3. Hello fönstret Package Manager-konsolen, ange hello **standardprojektet** tooyour nya konsolen projektet och sedan köra hello följande kommando i konsolfönstret hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Detta lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-paketet</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Öppna hello Program.cs-filen och Lägg till följande hello `using` instruktionen:
   
        using Microsoft.Azure.NotificationHubs;
5. I din `Program` klassen, lägga till hello följande metod. Uppdatera hello platshållartexten med din *DefaultFullSharedAccessSignature* anslutning sträng och hubbens namn från hello [klassiska Azure-portalen].
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }
6. Lägg till följande rader i hello din **Main** metoden:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Tryck på hello F5 viktiga toorun hello app. Du bör få ett meddelande i hello app.
   
      ![][21]

#### <a name="optional-send-notifications-by-using-a-mobile-service"></a>(Valfritt) Skicka meddelanden med hjälp av en mobiltjänst
1. Följ [Komma igång med Mobile Services].
2. Logga in toohello [klassiska Azure-portalen], och välj mobiltjänst.
3. Välj hello **Scheduler** fliken hello längst upp.
   
      ![][22]
4. Skapa ett nytt schemalagt jobb, infoga ett namn och välj **På begäran**.
   
      ![][23]
5. Klicka på hello Jobbnamnet när hello jobb skapas. Klicka på hello **skriptet** fliken på hello översta raden.
6. Infoga följande skript i schemaläggarfunktionen hello. Se till att tooreplace hello-platshållare med notification hub namn och hello anslutningssträngen för *DefaultFullSharedAccessSignature* som du fick tidigare. Klicka på **Spara**.
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );
7. Klicka på **kör en gång** på hello nedre fältet. Du bör få ett popup-meddelande.

## <a name="next-steps"></a>Nästa steg
I det här enkla exemplet skickade du meddelanden tooall dina Android-enheter. I ordning tootarget specifika användare finns i självstudiekursen toohello [använda Notification Hubs toopush meddelanden toousers]. Om du vill att toosegment användarna efter intressegrupper, kan du läsa [använda Notification Hubs toosend senaste nytt]. Läs mer om hur toouse Meddelandehubbar i [riktlinjer om Notification Hubs] och i hello [Meddelandehubbar hur toofor Android].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app toohello Notification Hub]: #connecting-app
[Run your app with hello emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Komma igång med Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[klassiska Azure-portalen]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[riktlinjer om Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Meddelandehubbar hur toofor Android]: http://msdn.microsoft.com/library/dn282661.aspx

[använda Notification Hubs toopush meddelanden toousers]: /manage/services/notification-hubs/notify-users-aspnet
[använda Notification Hubs toosend senaste nytt]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Google Cloud Messaging-klientkomponent]: http://components.xamarin.com/view/GCMClient/
[Azure Messaging-komponent]: http://components.xamarin.com/view/azure-messaging
