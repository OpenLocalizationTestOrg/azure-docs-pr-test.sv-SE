---
title: aaaSending push-meddelanden tooAndroid med Azure Notification Hubs | Microsoft Docs
description: "I kursen får du lära dig hur toouse Azure Notification Hubs toopush meddelanden tooAndroid enheter."
services: notification-hubs
documentationcenter: android
keywords: push-meddelanden, push-meddelande, push-meddelande i android
author: ysxu
manager: erikre
editor: 
ms.assetid: 8268c6ef-af63-433c-b14e-a20b04a0342a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 07/05/2016
ms.author: yuaxu
ms.openlocfilehash: 6b15a477d86cf1e6efffb21c5bcef0de7761af79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-tooandroid-with-azure-notification-hubs"></a>Skicka push-meddelanden tooAndroid med Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Översikt
> [!IMPORTANT]
> Det här ämnet visar push-meddelanden med Google Cloud Messaging (GCM). Om du använder Googles Firebase Cloud Messaging (FCM), se [skicka push-meddelanden tooAndroid med Azure Notification Hubs och FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).
> 
> 

Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooan Android-program.
Du skapar en tom Android-app som tar emot push-meddelanden genom att använda Google Cloud Messaging (GCM).

[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

hello slutförts koden för den här självstudiekursen kan hämtas från GitHub [här](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).

## <a name="prerequisites"></a>Krav
> [!IMPORTANT]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).
> 
> 

Dessutom tooan aktivt Azure-konto som anges ovan, kräver den här självstudiekursen bara hello senaste versionen av [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).

Du måste slutföra den här kursen innan du börjar någon annan kurs om Notification Hubs för Android-appar.

## <a name="creating-a-project-that-supports-google-cloud-messaging"></a>Skapa ett projekt som har stöd för Google Cloud Messaging
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-a-new-notification-hub"></a>Konfigurera en ny meddelandehubb
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6.   I hello **inställningar** bladet väljer **Notification Services** och sedan **Google (GCM)**. Ange hello API-nyckeln och klickar på **spara**.

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

Din meddelandehubb har nu konfigurerat toowork med GCM och du har hello anslutning strängar tooboth registrera din app tooreceive och skicka push-meddelanden.

## <a id="connecting-app"></a>Ansluta din app toohello notification hub
### <a name="create-a-new-android-project"></a>Skapa ett nytt Android-projekt
1. Starta ett nytt Android Studio-projekt i Android Studio.
   
   ![Android Studio – nytt projekt][13]
2. Välj hello **Telefoner och surfplattor** formuläret faktor och hello **Minimum SDK** som du vill toosupport. Klicka sedan på **Nästa**.
   
   ![Android Studio – arbetsflöde för att skapa projekt][14]
3. Välj **tom aktivitet** hello huvudsaklig aktivitet, klickar du på **nästa**, och klicka sedan på **Slutför**.

### <a name="add-google-play-services-toohello-project"></a>Lägga till Google Play services toohello projekt
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Lägga till bibliotek för Azure Notification Hubs
1. I hello `Build.Gradle` -filen för hello **app**, Lägg till följande rader i hello hello **beroenden** avsnitt.
   
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
2. Lägg till följande lagringsplats efter hello hello **beroenden** avsnitt.
   
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-hello-androidmanifestxml"></a>Uppdaterar hello AndroidManifest.xml.
1. toosupport GCM måste vi implementera en lyssnartjänst för instans-ID i vår kod som används för[hämta registreringstoken](https://developers.google.com/cloud-messaging/android/client#sample-register) med [Googles instans-ID API](https://developers.google.com/instance-id/). I den här självstudiekursen kommer vi att kalla klassen hello `MyInstanceIDService`. 
   
    Lägg till följande toohello AndroidManifest.xml tjänstdefinitionsfilen inuti hello hello `<application>` tagg. Ersätt hello `<your package>` med hello faktiska paketnamnet som visas överst hello i hello `AndroidManifest.xml` fil.
   
        <service android:name="<your package>.MyInstanceIDService" android:exported="false">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID"/>
            </intent-filter>
        </service>
2. När vi har tagit emot vår registreringstoken från hello API för instans-ID, kan den användas för[registrera med hello Azure Notification Hub](notification-hubs-push-notification-registration-management.md). Vi stöder denna registrering i hello bakgrunden med en `IntentService` med namnet `RegistrationIntentService`. Den här tjänsten kommer också att användas för [uppdatering av vår registreringstoken för GCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).
   
    Lägg till följande toohello AndroidManifest.xml tjänstdefinitionsfilen inuti hello hello `<application>` tagg. Ersätt hello `<your package>` med hello faktiska paketnamnet som visas överst hello i hello `AndroidManifest.xml` fil. 
   
        <service
            android:name="<your package>.RegistrationIntentService"
            android:exported="false">
        </service>
3. Vi kommer också att definiera en mottagare tooreceive meddelanden. Lägg till följande mottagare toohello AndroidManifest.xml definitionsfilen inuti hello hello `<application>` tagg. Ersätt hello `<your package>` med hello faktiska paketnamnet som visas överst hello i hello `AndroidManifest.xml` fil.
   
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
4. Lägg till hello följande nödvändiga GCM-relaterade behörigheter under hello `</application>` tagg. Se till att tooreplace `<your package>` med hello paketnamnet som visas överst hello i hello `AndroidManifest.xml` fil.
   
    Mer information om dessa behörigheter finns i [Konfigurera en GCM-klientapp för Android](https://developers.google.com/cloud-messaging/android/client#manifest).
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
   
        <permission android:name="<your package>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
        <uses-permission android:name="<your package>.permission.C2D_MESSAGE"/>

### <a name="adding-code"></a>Lägga till kod
1. Expandera i hello projektvyn **app** > **src** > **huvudsakliga** > **java**. Högerklicka på paketmappen under **java**, klicka på **Ny** och sedan klickar du på **Java-klass**. Lägg till en ny klass med namnet `NotificationSettings`. 
   
    ![Android Studio – ny Java-klass][6]
   
    Kontrollera att tooupdate hello dessa tre platshållare i följande kod för hello hello `NotificationSettings` klass:
   
   * **SenderId**: hello projektnumret som du hämtade tidigare i hello [Google Cloud-konsolen](http://cloud.google.com/console).
   * **HubListenConnectionString**: hello **DefaultListenAccessSignature** anslutningssträngen för din hubb. Du kan kopiera denna anslutningssträng genom att klicka på **åtkomstprinciper** på hello **inställningar** bladet för din hubb på hello [Azure Portal].
   * **HubName**: Använd hello namnet på din meddelandehubb som visas i hello hubbladet på hello [Azure Portal].
     
     `NotificationSettings` kod:
     
       offentlig klass NotificationSettings {
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Your default listen connection string>";
       }
2. Med hello stegen ovan kan lägga till ytterligare en ny klass med namnet `MyInstanceIDService`. Det här kommer att bli vår implementering av lyssnartjänsten för instans-ID.
   
    hello-koden för den här klassen anropar våra `IntentService` för[uppdateringstoken hello GCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) i hello bakgrund.
   
        import android.content.Intent;
        import android.util.Log;
        import com.google.android.gms.iid.InstanceIDListenerService;

        public class MyInstanceIDService extends InstanceIDListenerService {

            private static final String TAG = "MyInstanceIDService";

            @Override
            public void onTokenRefresh() {

                Log.i(TAG, "Refreshing GCM Registration Token");

                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


1. Lägg till en annan ny klass tooyour-projektet med namnet `RegistrationIntentService`. Detta blir hello implementeringen för vår `IntentService` som hanterar [uppdaterar hello GCM-token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) och [registreras med meddelandehubben hello](notification-hubs-push-notification-registration-management.md).
   
    Använd hello följande kod för den här klassen.
   
        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;
   
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.google.android.gms.iid.InstanceID;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class RegistrationIntentService extends IntentService {
   
            private static final String TAG = "RegIntentService";
   
            private NotificationHub hub;
   
            public RegistrationIntentService() {
                super(TAG);
            }
   
            @Override
            protected void onHandleIntent(Intent intent) {        
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
   
                try {
                    InstanceID instanceID = InstanceID.getInstance(this);
                    String token = instanceID.getToken(NotificationSettings.SenderId,
                            GoogleCloudMessaging.INSTANCE_ID_SCOPE);        
                    Log.i(TAG, "Got GCM Registration Token: " + token);
   
                    // Storing hello registration id that indicates whether hello generated token has been
                    // sent tooyour server. If it is not stored, send hello token tooyour server,
                    // otherwise your server should have already received hello token.
                    if ((regID=sharedPreferences.getString("registrationID", null)) == null) {        
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.i(TAG, "Attempting tooregister with NH using token : " + token);
   
                        regID = hub.register(token).getRegistrationId();
   
                        // If you want toouse tags...
                        // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1", "tag2").getRegistrationId();
   
                        resultString = "Registered Successfully - RegId : " + regID;
                        Log.i(TAG, resultString);        
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                    } else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed toocomplete token refresh", e);
                    // If an exception happens while fetching hello new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt hello update at a later time.
                }
   
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }
2. I din `MainActivity` klassen och Lägg till följande hello `import` uttryck ovanför hello klassen deklaration.
   
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.google.android.gms.gcm.*;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
3. Lägg till följande privata medlemmar hello överst i hello klassen hello. Vi använder dessa [Kontrollera hello tillgängligheten för Google Play-tjänster som rekommenderas av Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).
   
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private GoogleCloudMessaging gcm;
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
4. I din `MainActivity` klassen, lägga till hello följa metoden toohello tillgängligheten för Google Play-tjänster. 
   
        /**
         * Check hello device toomake sure it has hello Google Play Services APK. If
         * it doesn't, display a dialog that allows users toodownload hello APK from
         * hello Google Play Store or enable it in hello device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }
5. I din `MainActivity` klassen, Lägg till följande kod som kommer att söka efter Google Play Services innan du anropar hello din `IntentService` tooget dina registreringstoken och registrera med meddelandehubben.
   
        public void registerWithNotificationHubs()
        {
            Log.i(TAG, " Registering with Notification Hubs");
   
            if (checkPlayServices()) {
                // Start IntentService tooregister this application with GCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
6. I hello `OnCreate` metod för hello `MainActivity` klassen, Lägg till följande kod toostart hello registreringsprocessen när aktivitet skapas hello.
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
7. Lägg till dessa ytterligare metoder toohello `MainActivity` tooverify appens status och rapportera status i din app.
   
        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
   
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
   
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
   
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
   
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }
8. Hej `ToastNotify` metoden använder hello *”Hello World”* `TextView` styra tooreport status och meddelanden beständigt i hello app. Lägg till hello följande id för den kontrollen i activity_main.XML.
   
       android:id="@+id/text_hello"
9. Nu kommer vi lägga till en underklass för den mottagare som vi har definierat i hello AndroidManifest.xml. Lägg till en annan ny klass tooyour-projektet med namnet `MyHandler`.
10. Lägg till följande importuttryck överst hello i hello `MyHandler.java`:
    
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
11. Lägg till följande kod för hello hello `MyHandler` klass gör den till en underklass till `com.microsoft.windowsazure.notifications.NotificationsHandler`.
    
    Den här koden åsidosätter hello `OnReceive` metoden så hello hanteraren rapporterar meddelanden som tas emot. hello hanteraren skickar även hello push notification toohello Android notification manager genom att använda hello `sendNotification()` metod. Hej `sendNotification()` metod som ska köras när hello programmet körs inte och ett meddelande tas emot.
    
        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
    
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
    
            private void sendNotification(String msg) {
    
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
    
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
    
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
    
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
    
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }
12. I Android Studio hello menyraden klickar du på **skapa** > **återskapa projekt** toomake till att inga fel visas i din kod.

## <a name="sending-push-notifications"></a>Skicka push-meddelanden
Du kan testa att ta emot push-meddelanden i din app genom att skicka dem via hello [Azure Portal] -leta efter hello **felsökning** under hello hubbladet, som visas nedan.

![Azure Notification Hubs – Prova att skicka](./media/notification-hubs-android-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-hello-app"></a>(Valfritt) Skicka push-meddelanden direkt från hello app
Normalt sett skickar du meddelanden med hjälp av en backend-server. I vissa fall kanske du vill toobe kan toosend push-meddelanden direkt från hello-klientprogrammet. Det här avsnittet beskrivs hur toosend meddelanden från hello-klient som använder hello [Azure Notification Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).

1. Expandera **App** > **src** > **main** > **res** > **layout** i projektvyn för Android Studio. Öppna hello `activity_main.xml` layout och klicka på hello **Text** fliken tooupdate hello textinnehållet i hello-filen. Uppdatera den med hello koden nedan, som lägger till nya `Button` och `EditText` kontroller för att skicka push-meddelande meddelanden toohello meddelandehubben. Lägg till den här koden längst ned hello, precis före `</RelativeLayout>`.
   
        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />
   
        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />
2. Expandera **App** > **src** > **main** > **res** > **värden** i projektvyn för Android Studio. Öppna hello `strings.xml` och Lägg till hello strängvärden som refereras av hello nya `Button` och `EditText` kontroller. Lägg till dessa längst ned hello hello-fil, precis före `</resources>`.
   
        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>
3. I din `NotificationSetting.java` lägger du till följande inställning toohello hello `NotificationSettings` klass.
   
    Uppdatera `HubFullAccess` med hello **DefaultFullSharedAccessSignature** anslutningssträngen för din hubb. Den här anslutningssträngen kan kopieras från hello [Azure Portal] genom att klicka på **åtkomstprinciper** på hello **inställningar** bladet för din meddelandehubb.
   
        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccess Connection string>";
4. I din `MainActivity.java` lägger du till följande hello `import` uttryck ovanför hello `MainActivity` klass.
   
        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;
5. I din `MainActivity.java` lägger du till följande medlemmar hello överst i hello hello `MainActivity` klass.    
   
        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;
6. Du måste skapa en token tooauthenticate Software Access Signature (SaS) en POST-begäran toosend meddelanden tooyour notification hub. Detta görs genom parsning hello nyckeldata från hello anslutningssträngen och sedan skapa hello SaS-token som anges i hello [vanliga koncept](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API-referensen. hello följande kod är ett exempel på implementering.
   
    I `MainActivity.java`, Lägg till följande metod toohello hello `MainActivity` klassen tooparse anslutningssträngen.
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * tooparse hello connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be hello DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
   
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }
7. I `MainActivity.java`, Lägg till följande metod toohello hello `MainActivity` klassen toocreate en SaS-token för autentisering.
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from hello access key tooauthenticate a request.
         *
         * @param uri hello unencoded resource URI string for this operation. hello resource
         *            URI is hello full URI of hello Service Bus resource toowhich access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
   
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
   
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
   
                // Get an hmac_sha1 key from hello raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
   
                // Get an hmac_sha1 Mac instance and initialize with hello signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
   
                // Compute hello hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
   
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
   
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
   
            return token;
        }
8. I `MainActivity.java`, Lägg till följande metod toohello hello `MainActivity` klassen toohandle hello **skicka meddelande** Klicka på och skicka push-meddelande för hello meddelandet toohello hubb med hjälp av hello inbyggda REST-API.
   
        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added toohello Authorization header on hello POST request toothe
         * notification hub. hello text in hello editTextNotificationMessage control
         * is added as hello JSON body for hello request tooadd a GCM message toohello hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
   
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
   
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
   
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
   
                            // Authenticate hello POST request with hello SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
   
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
   
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //        "tag1 || tag2 || tag3");
   
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
   
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }
   
                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }

## <a name="testing-your-app"></a>Testa din app
#### <a name="push-notifications-in-hello-emulator"></a>Push-meddelanden i emulatorn hello
Om du vill tootest push-meddelanden inne i en emulator, se till att emulatorbilden stöder hello Google API-nivå som du har valt för din app. Om bilden inte stöder interna Google APIs, du kommer att få hello **SERVICE\_inte\_tillgänglig** undantag.

Dessutom toohello ovan, se till att du har lagt till ditt Google-konto tooyour kör emulatorns under **inställningar** > **konton**. I annat fall din försök tooregister med GCM leda till hello **AUTENTISERING\_misslyckades** undantag.

#### <a name="running-hello-application"></a>Kör hello program
1. Kör hello appen och Lägg märke till att hello registrerings-ID rapporteras vid en lyckad registrering.
   
      ![Tester på Android – Kanalregistrering][18]
2. Ange ett meddelande meddelandet toobe skickas tooall Android-enheter som har registrerats med hello-hubben.
   
      ![Tester på Android – skicka ett meddelande][19]

3. Tryck på **Skicka meddelande**. Alla enheter som har hello appen körs visas en `AlertDialog` -instans med hello push-meddelandet. Enheter som inte har hello app körs men som tidigare hade registrerats för push-meddelanden får ett meddelande i hello Android Notification Manager. Dessa kan visas genom att svepa nedåt från hello övre vänstra hörnet.
   
      ![Tester på Android – meddelanden][21]

## <a name="next-steps"></a>Nästa steg
Vi rekommenderar hello [använda Notification Hubs toopush meddelanden toousers] självstudiekurs som hello nästa steg. Den visar hur toosend meddelanden från en ASP.NET-backend med taggar tootarget specifika användare.

Om du vill toosegment in användarna efter intressegrupper, kolla hello [använda Notification Hubs toosend senaste nytt] kursen.

toolearn mer allmän information om Notification Hubs finns i vår [riktlinjer om Notification Hubs].

<!-- Images. -->
[6]: ./media/notification-hubs-android-get-started/notification-hub-android-new-class.png

[12]: ./media/notification-hubs-android-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-new-project.png
[14]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-choose-form-factor.png
[15]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png
[16]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app5.png
[17]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app6.png

[18]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-registered.png
[19]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-set-message.png

[20]: ./media/notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-received-message.png
[22]: ./media/notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/notification-hubs-android-get-started/notification-hub-scheduler2.png
[29]: ./media/mobile-services-android-get-started-push/mobile-eclipse-import-Play-library.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png

[31]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-add-ui.png


<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[riktlinjer om Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[använda Notification Hubs toopush meddelanden toousers]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[använda Notification Hubs toosend senaste nytt]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure Portal]: https://portal.azure.com
