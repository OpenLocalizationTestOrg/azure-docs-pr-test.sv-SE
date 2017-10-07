---
title: "aaaGet igång med Azure Notification Hubs för Kindle-appar | Microsoft Docs"
description: "I kursen får du lära dig hur toouse Azure Notification Hubs toosend push-meddelanden tooa Kindle-App."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 346fc8e5-294b-4e4f-9f27-7a82d9626e93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-kindle
ms.devlang: Java
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 7c28d64372cd2d90bab9cd9bf818d333f3478f7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Kom igång med Notification Hubs för Kindle-appar
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Översikt
Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa Kindle-App.
Du skapar en tom Kindle-app som tar emot push-meddelanden genom att använda Amazon Device Messaging (ADM).

## <a name="prerequisites"></a>Krav
Den här kursen kräver hello följande:

* Hämta hello Android SDK (vi antar att du kommer att använda Eclipse) från hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">webbplatsen för Android</a>.
* Gör så hello i <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">inställningen Konfigurera din utvecklingsmiljö</a> tooset in din utvecklingsmiljö för Kindle.

## <a name="add-a-new-app-toohello-developer-portal"></a>Lägg till en ny app toohello developer-portalen
1. Börja med att skapa en app i hello [Amazon developer-portalen].
   
    ![][0]
2. Kopiera hello **Programnyckeln**.
   
    ![][1]
3. Klicka på hello namnet på din app i hello-portalen och klicka på hello **Device Messaging** fliken.
   
    ![][2]
4. Klicka på **Skapa en ny säkerhetsprofil** och skapa sedan en ny säkerhetsprofil (till exempel **säkerhetsprofilen TestAdm**). Klicka sedan på **Spara**.
   
    ![][3]
5. Klicka på **Säkerhetsprofiler** tooview hello säkerhetsprofil som du nyss skapade. Kopiera hello **klient-ID** och **Klienthemlighet** värden för senare användning.
   
    ![][4]

## <a name="create-an-api-key"></a>Skapa en API-nyckel.
1. Öppna en kommandotolk med administratörsbehörighet.
2. Navigera toohello Android SDK-mappen.
3. Ange hello följande kommando:
   
        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
   
    ![][5]
4. För hello **keystore** lösenord, skriver **android**.
5. Kopiera hello **MD5** fingeravtryck.
6. Tillbaka i hello developer-portalen på hello **Messaging** klickar du på **Android/Kindle** och ange hello hello paketets namn för din app (till exempel **com.sample.notificationhubtest**) och hello **MD5** värdet och klicka sedan på **generera API-nyckel**.

## <a name="add-credentials-toohello-hub"></a>Lägg till autentiseringsuppgifter toohello hub
Lägga till hello klienten hemlighet och klient-ID toohello hello portal **konfigurera** fliken i din meddelandehubb.

## <a name="set-up-your-application"></a>Konfigurera din app
> [!NOTE]
> När du skapar en app bör du använda minst API-nivå 17.
> 
> 

Lägg till hello ADM bibliotek tooyour Eclipse-projektet:

1. tooobtain hello ADM-biblioteket, [hämta hello SDK]. Extrahera hello SDK zip-filen.
2. Högerklicka på projektet i Eclipse och klicka sedan på **Egenskaper**. Välj **Java byggsökväg** på hello vänster och välj sedan hello ** bibliotek ** fliken hello överst. Klicka på **Lägg till extern Jar**, och välj hello `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` från hello katalog där du extraherade hello Amazon SDK.
3. Hämta hello NotificationHubs Android SDK (länk).
4. Packa hello paketet och dra sedan filen hello `notification-hubs-sdk.jar` till hello `libs` mappen i Eclipse.

Redigera din app manifestet toosupport ADM:

1. Lägg till hello namnområdet för Amazon i hello rotelementets manifest:

        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

1. Lägg till behörigheter som hello första elementet under manifestelementet hello. Ersätt **[YOUR PACKAGE NAME]** med hello-paket som du använde toocreate din app.
   
        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />
   
        <uses-permission android:name="android.permission.INTERNET"/>
   
        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />
   
        <!-- This permission allows your app access tooreceive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />
   
        <!-- ADM uses WAKE_LOCK tookeep hello processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />
2. Infoga följande element som hello hello App-elementets första underordnade hello. Kom ihåg toosubstitute **[YOUR SERVICE NAME]** med hello namnet på din meddelandehanterare som du skapar i nästa avsnitt om hello (inklusive hello-paket), och Ersätt **[YOUR PACKAGE NAME]** med hello paketnamnet som du skapade din app.
   
        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />
   
        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />
   
            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >
   
            <!-- toointeract with ADM, your app must listen for hello following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />
   
          <!-- Replace hello name in hello category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>Skapa en meddelandehanterare för ADM
1. Skapa en ny klass som ärver från `com.amazon.device.messaging.ADMMessageHandlerBase` och ger den namnet `MyADMMessageHandler`, enligt följande bild hello:
   
    ![][6]
2. Lägg till följande hello `import` instruktioner:
   
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub
3. Lägg till följande kod i hello-klass som du skapade hello. Kom ihåg toosubstitute hello hubb och anslutningssträngen (lyssna):
   
        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
          private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }
   
        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }
   
            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }
   
            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();
   
                mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);
   
            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                  new Intent(ctx, MainActivity.class), 0);
   
            NotificationCompat.Builder mBuilder =
                  new NotificationCompat.Builder(ctx)
                  .setSmallIcon(R.mipmap.ic_launcher)
                  .setContentTitle("Notification Hub Demo")
                  .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                  .setContentText(msg);
   
             mBuilder.setContentIntent(contentIntent);
             mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }
4. Lägg till följande kod toohello hello `OnMessage()` metoden:
   
        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);
5. Lägg till följande kod toohello hello `OnRegistered` metoden:
   
            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }
6. Lägg till följande kod toohello hello `OnUnregistered` metoden:
   
         try {
             getNotificationHub(getApplicationContext()).unregister();
         } catch (Exception e) {
             Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
         }
7. I hello `MainActivity` metoden Lägg till följande importuttryck hello:
   
        import com.amazon.device.messaging.ADM;
8. Lägg till följande kod hello slutet av hello hello `OnCreate` metoden:
   
        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                         MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-tooyour-app"></a>Lägg till nyckel tooyour API-appen
1. I Eclipse skapar du en ny fil med namnet **api_key.txt** i hello directory tillgångar i ditt projekt.
2. Öppna hello-filen och kopiera hello API-nyckel som du genererade på hello Amazon developer-portalen.

## <a name="run-hello-app"></a>Kör hello-appen
1. Starta hello-emulatorn.
2. I hello-emulatorn Svep hello uppifrån och klickar på **inställningar**, och klicka sedan på **mitt konto** och registrera med ett giltigt Amazon-konto.
3. Kör hello app i Eclipse.

> [!NOTE]
> Om ett problem uppstår, kontrollerar du hello tiden för emulatorn hello (eller enhet). hello tidsvärdet måste vara korrekt. toochange hello tid hello Kindle-emulatorns, kan du köra följande kommando från katalogen för plattformsverktygen Android SDK hello:
> 
> 

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Skicka ett meddelande
toosend ett meddelande med hjälp av .NET:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon developer-portalen]: https://developer.amazon.com/home.html
[hämta hello SDK]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
