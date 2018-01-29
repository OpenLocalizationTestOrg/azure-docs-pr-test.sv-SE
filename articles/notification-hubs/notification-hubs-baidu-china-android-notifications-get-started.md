---
title: "Kom igång med Azure Notification Hubs genom att använda Baidu | Microsoft Docs"
description: "I den här självstudiekursen får du lära dig hur du använder Azure Notification Hubs för att skicka push-meddelanden till Android-enheter med hjälp av Baidu."
services: notification-hubs
documentationcenter: android
author: kpiteira
manager: erikre
editor: 
ms.assetid: 23bde1ea-f978-43b2-9eeb-bfd7b9edc4c1
ms.service: notification-hubs
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: mobile-baidu
ms.workload: mobile
ms.date: 08/29/2017
ms.author: kapiteir
ms.openlocfilehash: 91f20a6e0ff6c2dd512879e9ab3c9369dab5d8ff
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/16/2017
---
# <a name="get-started-with-notification-hubs-using-baidu"></a>Kom igång med Notification Hub genom att använda Baidu
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Baidu Cloud Push är en kinesisk molntjänst som du kan använda för att skicka push-meddelanden till mobila enheter. 

Eftersom Google Play och FCM (Firebase Cloud Messaging) inte är tillgängliga i Kina, är det nödvändigt att använda andra appbutiker och push-tjänster. Baidu är en av dem och den som för tillfället används av Notification Hub.

## <a name="prerequisites"></a>Krav
För den här kursen behöver du:

* Android SDK (vi antar att du använder Android Studio) som du kan ladda ned från <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android-webbplatsen</a>
* [Baidu Push Android SDK:n]

> [!NOTE]
> Du måste ha ett aktivt Azure-konto för att slutföra den här kursen. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den kostnadsfria utvärderingsversionen av Azure finns [här](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).
> 
> 

## <a name="create-a-baidu-account"></a>Skapa ett Baidu-konto
Om du vill använda Baidu måste du ha ett Baidu-konto. Om du redan har ett sådant, loggar du in på [Baidu-portalen] och sedan hoppar du över nästa steg. Annars läser du följande anvisningarna om hur du skapar ett Baidu-konto.  

1. Gå till [Baidu-portalen] och klicka på länken **登录** (**Logga in**). Klicka på **立即注册** (**registrera dig nu**) för att starta kontoregistreringsprocessen.
   
    ![Baidu-registrering](./media/notification-hubs-baidu-get-started/BaiduRegistration.png)

2. Ange den information som krävs: telefonnummer/e-postadress, lösenord och verifieringskod. Klicka därefter på 注册 (**registrera**).
   
    ![Baidu-registringsinmatning](./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png)

3. Du kommer att få ett e-post till den adress du angav med en länk för att aktivera Baidu-kontot.
   
    ![Baidu-registreringsbekräftelse](./media/notification-hubs-baidu-get-started/BaiduConfirmation.png)

4. Logga in på ditt e-postkonto, öppna aktiveringsmejlet från Baidu och klicka på aktiveringslänken för att aktivera ditt Baidu-konto.
   
    ![Baidu-aktiverings-e-post](./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png)

När du har ett aktiverat Baidu-konto kan du logga in på [Baidu-portalen].

## <a name="create-a-baidu-cloud-push-project"></a>Skapa ett Baidu Cloud Push-projekt
När du skapar ett Baidu Cloud Push-projekt, kommer du att få ett app-ID, en API-nyckel och en hemlig nyckel.

1. När du har loggat in på [Baidu-portalen], klickar du på **更多 >>** (**mer**).
   
    ![Registrering – Mer](./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png)

2. Rulla nedåt i avsnittet **站长与开发者服务** (**tjänster för webbadministratörer och utvecklare**) och klicka på **百度云推送** (**Baidu Cloud Push**).
   
    ![Baidu Open Cloud-plattformen](./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png)

3. På nästa sida, klickar du på **登录** (**Logga in**) i övre högra hörnet.
   
    ![Baidu-inloggning](./media/notification-hubs-baidu-get-started/BaiduLogin.png)

4. Klicka sedan på **创建应用** (**skapa program**) på den här sidan.

    ![Baidu skapa program](./media/notification-hubs-baidu-get-started/BaiduCreateApplication.png)

5. På nästa sida, klickar du på 创建新应用 (**skapa nytt program**).
   
    ![Baidu skapa nytt program](./media/notification-hubs-baidu-get-started/BaiduCreateNewApplication.png)

6. Ange ett programnamn och klicka på 创建 (**skapa**).
   
    ![](./media/notification-hubs-baidu-get-started/BaiduCreateApplicationDoCreate.png)

7. När ett Baidu Cloud Push-projekt har skapats på rätt sätt, visas en sida med **AppID**, **API-nyckeln** och en **hemlig nyckel**. Skriv ner API-nyckeln och den hemliga nyckeln, dessa ska användas senare.
   
    ![Baidu Push-hemligheter](./media/notification-hubs-baidu-get-started/BaiduGetSecrets.png)

8. Konfigurera projektet för puch-meddelanden genom att klicka på 创建通知 (**skapa meddelande**)  i den vänstra panelen.
   
    ![](./media/notification-hubs-baidu-get-started/BaiduCreateNotification.png)


## <a name="configure-a-new-notification-hub"></a>Konfigurera en ny meddelandehubb
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. I din meddelandehubb, väljer du **Notification Services** och därefter **Baidu (Android China)**.

&emsp;&emsp;![Azure Notification Hubs – Baidu](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)

&emsp;&emsp;7. Rulla ned till avsnittet Baidu-meddelandeinställningar. Ange API-nyckeln och den hemliga nyckeln som du fick från Baidu-konsolen i Baidu Cloud Push-projektet. Klicka sedan på spara.

&emsp;&emsp;![Azure Notification Hubs – Baidu-hemligheter](./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png)

Din meddelandehubb har nu konfigurerats för att fungera med Baidu. Du har också **anslutningssträngar** för att registrera din app för att både skicka och ta emot push-meddelanden.

Anteckna `DefaultListenSharedAccessSignature` och `DefaultFullSharedAccessSignature` från fönstret åtkomstanslutningsinformation.

## <a name="connect-your-app-to-the-notification-hub"></a>Anslut appen till meddelandehubben
1. Skapa ett nytt Android-projekt i Android Studio (Arkiv > Nytt > Nytt projekt).

    ![Azure Notification Hubs – Baidu nytt projekt](./media/notification-hubs-baidu-get-started/AndroidNewProject.png)

2.  Ange ett programnamn och kontrollera att Minsta nödvändiga SDK-version är inställd på API 16: Android 4.1. **Kontrollera också att ditt paketnamn (应用包名) är samma som i Baidu Cloud Push-portalen**

    ![Azure Notification Hubs – Baidu min SDK1](./media/notification-hubs-baidu-get-started/AndroidMinSDK.png)
    ![Azure Notification Hubs – Baidu min SDK2](./media/notification-hubs-baidu-get-started/AndroidMinSDK2.png)

3.  Klicka på Nästa och fortsätt följa anvisningarna i guiden tills fönstret skapa aktivitet visas. Se till att Tom aktivitet är markerad och välj slutligen Slutför för att skapa en ny Android-app.

    ![Azure Notification Hubs – Baidu lägg till aktivitet](./media/notification-hubs-baidu-get-started/AndroidAddActivity.png)

4.  Kontrollera att mål för projektgenerering är korrekt inställt.

5.  Lägg sedan till Azure Notification Hubs-bibliotek. I `Build.Gradle`-filen för appen, lägger du till följande rader i avsnittet beroenden.

    ```javascript
    compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
    compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```

    Lägg till följande lagringsplats efter avsnittet beroenden.

    ```javascript
    repositories {
        maven {
            url "http://dl.bintray.com/microsoftazuremobile/SDK"
        }
    }
    ```

    För att undvika listkonflikten, måste vi lägga till följande kod i **Manifest.xml**.

    ```xml
    <manifest package="YOUR.PACKAGE.NAME"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:android="http://schemas.android.com/apk/res/android">
    ```

    och i `<application/>`-taggen:

    ```xml
    <application
        tools:replace="android:allowBackup,icon,theme,label">
    ```

6.  Hämta och packa upp [Baidu Push Android SDK:n]. Kopiera `pushservice-x.y.z jar`-filen i libs-mappen. Kopiera sedan `.so`-filerna i `src/main/jniLibs`-mapparna (skapa en ny mapp) på din Android-App.

    ![Azure Notification Hubs – Baidu SDK Libs](./media/notification-hubs-baidu-get-started/BaiduSDKLib.png)

7. Högerklicka på filen pushervice-x.y.z.jar i libs-mappen, klicka på lägg till som bibliotek för att inkludera det här biblioteket i projektet.

    ![Azure Notification Hubs – Baidu lägg till som bibliotek](./media/notification-hubs-baidu-get-started/BaiduAddAsALib.jpg)

8. Öppna filen **AndroidManifest.xml** för ditt Android-projekt och lägg till de behörigheter som krävs för Baidu-SDK:et. **Ersätt `YOURPACKAGENAME` med ditt paketnamn**.

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    <uses-permission android:name="android.permission.WRITE_SETTINGS" />
    <uses-permission android:name="android.permission.VIBRATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
    <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
    <uses-permission android:name="android.permission.EXPAND_STATUS_BAR" />
    !! <uses-permission android:name="baidu.push.permission.WRITE_PUSHINFOPROVIDER.YOURPACKAGENAME" />
    !!<permission android:name="baidu.push.permission.WRITE_PUSHINFOPROVIDER.YOURPACKAGENAME"android:protectionLevel="normal" />

    ```

9. Lägg till följande konfiguration inom programelementet efter aktivitetselementet `.MainActivity`, ersätt *dittprojektnamn* (till exempel, `com.example.BaiduTest`):

    ```xml
    <activity
        android:name="com.baidu.android.pushservice.richmedia.MediaViewActivity"
        android:configChanges="orientation|keyboardHidden"
        android:label="MediaViewActivity" />
    <activity
        android:name="com.baidu.android.pushservice.richmedia.MediaListActivity"
        android:configChanges="orientation|keyboardHidden"
        android:label="MediaListActivity"
        android:launchMode="singleTask" />
 
    <!-- Push application definition message -->
    <receiver android:name=".MyPushMessageReceiver">
        <intent-filter>

            <!-- receive push message-->
            <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
            <!-- receive bind,unbind,fetch,delete.. message-->
            <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
            <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
        </intent-filter>
    </receiver>

    <receiver
        android:name="com.baidu.android.pushservice.PushServiceReceiver"
        android:process=":bdservice_v1">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
            <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
            <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            <action android:name="com.baidu.android.pushservice.action.media.CLICK" />
            <action android:name="android.intent.action.MEDIA_MOUNTED" />
            <action android:name="android.intent.action.USER_PRESENT" />
            <action android:name="android.intent.action.ACTION_POWER_CONNECTED" />
            <action android:name="android.intent.action.ACTION_POWER_DISCONNECTED" />
        </intent-filter>
    </receiver>

    <receiver
        android:name="com.baidu.android.pushservice.RegistrationReceiver"
        android:process=":bdservice_v1">
        <intent-filter>
            <action android:name="com.baidu.android.pushservice.action.METHOD" />
            <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
        </intent-filter>
        <intent-filter>
            <action android:name="android.intent.action.PACKAGE_REMOVED" />

            <data android:scheme="package" />
        </intent-filter>
    </receiver>

    <service
        android:name="com.baidu.android.pushservice.PushService"
        android:exported="true"
        android:process=":bdservice_v1">
        <intent-filter>
            <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
        </intent-filter>
    </service>

    <service
        android:name="com.baidu.android.pushservice.CommandService"
        android:exported="true" />

    <!-- Adapt the ContentProvider declaration required for the Android N system, and the write permissions include the application package name-->
    <provider
        android:name="com.baidu.android.pushservice.PushInfoProvider"
        android:authorities="com.baidu.push.example.bdpush"
        android:exported="true"
        android:protectionLevel="signature"
        android:writePermission="baidu.push.permission.WRITE_PUSHINFOPROVIDER. yourprojectname  " />

    <!-- API Key of the Baidu application -->
    <meta-data
        android:name="api_key"
        !!   android:value="api_key" />
    </application>
    ```

10. Lägg till en ny klass med namnet `ConfigurationSettings.java` till projektet.

    ```java
    public class ConfigurationSettings {
        public static String API_KEY = "...";
        public static String NotificationHubName = "...";
        public static String NotificationHubConnectionString = "...";
    }
    ```
    
    Ställ in värdet för strängen `API_KEY` med API_KEY från Baidu Cloud-projektet.
    
    Ställ in värdet för strängen `NotificationHubName` med ditt meddelandehubbsnamn från [Azure-portalen] och därefter `NotificationHubConnectionString` med `DefaultListenSharedAccessSignature` från [Azure-portalen].

11. Öppna MainActivity.java och lägg till följande till onCreate-metoden:

    ```java
    PushManager.startWork(this, PushConstants.LOGIN_TYPE_API_KEY,  API_KEY );
    ```

12. Lägg till en ny klass som heter `MyPushMessageReceiver.java` och lägg till följande kod till den. Det är klassen som hanterar de push-meddelanden som tas emot från push-servern i Baidu.

    ```java
    package your.package.name;

    import android.content.Context;
    import android.content.Intent;
    import android.os.AsyncTask;
    import android.text.TextUtils;
    import android.util.Log;

    import com.baidu.android.pushservice.PushMessageReceiver;
    import com.microsoft.windowsazure.messaging.NotificationHub;
    import org.json.JSONException;
    import org.json.JSONObject;

    import java.util.List;

    public class MyPushMessageReceiver extends PushMessageReceiver {

        public static final String TAG = MyPushMessageReceiver.class
                .getSimpleName();
        public static NotificationHub hub = null;
        public static String mChannelId, mUserId;

        @Override
        public void onBind(Context context, int errorCode, String appid,
                        String userId, String channelId, String requestId) {
            String responseString = "onBind errorCode=" + errorCode + " appid="
                    + appid + " userId=" + userId + " channelId=" + channelId
                    + " requestId=" + requestId;
            Log.d(TAG, responseString);

            if (errorCode == 0) {
                // Binding successful
                Log.d(TAG, " Binding successful");
            }
            try {
                if (hub == null) {
                    hub = new NotificationHub(
                            ConfigurationSettings.NotificationHubName,
                            ConfigurationSettings.NotificationHubConnectionString,
                            context);
                    Log.i(TAG, "Notification hub initialized");
                }
            } catch (Exception e) {
                Log.e(TAG, e.getMessage());
            }
            mChannelId = channelId;
            mUserId = userId;

            registerWithNotificationHubs();
        }
        private void registerWithNotificationHubs() {

            new AsyncTask<Void, Void, Void>() {
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        hub.registerBaidu(mUserId, mChannelId);
                        Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                    } catch (Exception e) {
                        Log.e(TAG, e.getMessage());
                    }
                    return null;
                }
            }.execute(null, null, null);
        }

        @Override
        public void onMessage(Context context, String message,
                            String customContentString) {
            String messageString = " onMessage=\"" + message
                    + "\" customContentString=" + customContentString;
            Log.d(TAG, messageString);
            if (!TextUtils.isEmpty(customContentString)) {
                JSONObject customJson = null;
                try {
                    customJson = new JSONObject(customContentString);
                    String myvalue = null;
                    if (!customJson.isNull("mykey")) {
                        myvalue = customJson.getString("mykey");
                    }
                } catch (JSONException e) {
                    e.printStackTrace();
                }
            }

        }

        @Override
        public void onNotificationArrived(Context context, String title, String description, String customContentString) {
            String notifyString = " Notice Arrives onNotificationArrived  title=\"" + title
                    + "\" description=\"" + description + "\" customContent="
                    + customContentString;
            Log.d(TAG, notifyString);
            if (!TextUtils.isEmpty(customContentString)) {
                JSONObject customJson = null;
                try {
                    customJson = new JSONObject(customContentString);
                    String myvalue = null;
                    if (!customJson.isNull("mykey")) {
                        myvalue = customJson.getString("mykey");
                    }
                } catch (JSONException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            }
        }

        @Override
        public void onNotificationClicked(Context context, String title, String description, String customContentString) {
            String notifyString = " onNotificationClicked title=\"" + title + "\" description=\""
                    + description + "\" customContent=" + customContentString;
            Log.d(TAG, notifyString);
            Intent intent = new Intent(context.getApplicationContext(),MainActivity.class);
            intent.putExtra("title",title);
            intent.putExtra("description",description);
            intent.putExtra("isFromNotify",true);
            intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            context.getApplicationContext().startActivity(intent);

        }

        @Override
        public void onSetTags(Context context, int errorCode,
                            List<String> successTags, List<String> failTags, String requestId) {
            String responseString = "onSetTags errorCode=" + errorCode
                    + " successTags=" + successTags + " failTags=" + failTags
                    + " requestId=" + requestId;
            Log.d(TAG, responseString);

        }

        @Override
        public void onDelTags(Context context, int errorCode,
                            List<String> successTags, List<String> failTags, String requestId) {
            String responseString = "onDelTags errorCode=" + errorCode
                    + " successTags=" + successTags + " failTags=" + failTags
                    + " requestId=" + requestId;
            Log.d(TAG, responseString);

        }

        @Override
        public void onListTags(Context context, int errorCode, List<String> tags,
                            String requestId) {
            String responseString = "onListTags errorCode=" + errorCode + " tags="
                    + tags;
            Log.d(TAG, responseString);

        }

        @Override
        public void onUnbind(Context context, int errorCode, String requestId) {
            String responseString = "onUnbind errorCode=" + errorCode
                    + " requestId = " + requestId;
            Log.d(TAG, responseString);

            if (errorCode == 0) {
                // Unbinding is successful
                Log.d(TAG, " Unbinding is successful ");
            }
        }
    }
    ```

## <a name="send-notifications-to-your-app"></a>Skicka meddelanden till din app

Du kan snabbt testa att ta emot meddelanden från [Azure-portalen]: använd knappen **Skicka** på konfigurationsskärmen i meddelandehubben, som det visas på följande skärmar:

![](./media/notification-hubs-baidu-get-started/BaiduTestSendButton.png)
![](./media/notification-hubs-baidu-get-started/BaiduTestSend.png)

Push-meddelanden skickas vanligtvis i en serverdelstjänst som Mobile Services eller ASP.NET med hjälp av ett kompatibelt bibliotek. Om ett bibliotek inte är tillgängligt för din serverdel, kan du använda REST-API:et direkt för att skicka meddelanden.

Den här guiden använder sig för enkelhetens skull av en konsolapp för att demonstrera hur du skickar ett meddelande med .NET SDK. Vi rekommenderar dock guiden [använd Notification Hubs för att skicka push-meddelanden till användare](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) som nästa steg i att skicka meddelanden från en ASP.NET-serverdel. 

Här följer olika metoder för att skicka meddelanden:
* **REST-gränssnitt**: Du kan använda meddelanden på alla serverdelsplattformar med [REST-gränssnittet](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **Microsoft Azure Notification Hubs .NET SDK**: I Nuget Package Manager för Visual Studio kör du [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* **Node.js**: [Använda Notification Hubs från Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* **Mobile Apps**: För ett exempel på hur man skickar meddelanden från en Azure App Service Mobile Apps-serverdel som är integrerad med Notification Hubs, kan du gå till [Lägg till push-meddelanden i din mobilapp](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).
* **Java/PHP**: Ett exempel på hur du skickar meddelanden med hjälp av REST-API finns i avsnittet Använda Notification Hubs från Java/PHP ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

## <a name="optional-send-notifications-from-a-net-console-app"></a>(Valfritt) Skicka meddelanden från en .NET-konsolapp
I det här avsnittet kommer du att lära dig hur du skickar meddelanden med hjälp av en .NET-konsolapp.

1. Skapa en ny Visual C#-konsolapp:
   
    ![](./media/notification-hubs-baidu-get-started/ConsoleProject.png)

2. I fönstret Package Manager-konsol ställer du in **standardprojektet** till det nya projektet för konsolprogrammet. Sedan kör du följande kommando i konsolfönstret:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Med den här instruktionen läggs en referens till i Azure Notification Hubs SDK med hjälp av <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs-NuGet-paketet</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. Öppna filen `Program.cs` och lägg till följande med hjälp av uttryck:
   
    ```csharp
    using Microsoft.Azure.NotificationHubs;
    ```

4. I din `Program`-klass, lägger du till följande metod och ersätter `DefaultFullSharedAccessSignatureSASConnectionString` och `NotificationHubName` med de värden du har.
   
    ```csharp
    private static async void SendNotificationAsync()
    {
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
        string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
        var result = await hub.SendBaiduNativeNotificationAsync(message);
    }
    ```

5. Lägg till följande rader i `Main`-metoden:

    ```csharp
    SendNotificationAsync();
    Console.ReadLine();
    ```

## <a name="test-your-app"></a>Testa din app

Om du vill testa den här appen med en riktig mobil behöver du bara ansluta den till datorn med en USB-kabel. Med den här åtgärden läses din app in på den anslutna mobilen.

Om du vill testa appen med emulatorn klickar du på **Kör** i verktygsfältet högst upp i Android Studio och väljer sedan din app: emulatorn startas, läser in och kör appen.

Appen hämtar `userId` och `channelId` från Baidu Push-meddelandetjänsten och registrerar sig med meddelandehubben.

Du kan använda felsökningsfliken i [Azure-portalen] för att skicka ett testmeddelande. Om du har byggt .NET-konsolappen för Visual Studio, behöver du bara trycka på tangenten F5 i Visual Studio för att köra appen. Appen skickar ett meddelande som visas i det övre meddelandefältet i enheten eller emulatorn.

<!-- URLs. -->
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu Push Android SDK:n]: http://push.baidu.com/sdk/push_client_sdk_for_android
[Azure-portalen]: https://portal.azure.com/
[Baidu-portalen]: http://www.baidu.com/
