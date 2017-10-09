---
title: "aaaGet igång med Azure Notification Hubs genom att använda Baidu | Microsoft Docs"
description: "I kursen får du lära dig hur toouse Azure Notification Hubs toopush meddelanden tooAndroid enheter med hjälp av Baidu."
services: notification-hubs
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 23bde1ea-f978-43b2-9eeb-bfd7b9edc4c1
ms.service: notification-hubs
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: mobile-baidu
ms.workload: mobile
ms.date: 08/19/2016
ms.author: yuaxu
ms.openlocfilehash: 2767fdd3bb04674e7a531634237cc05cd8c21cb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-notification-hubs-using-baidu"></a>Kom igång med Notification Hub genom att använda Baidu
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Översikt
Baidu cloud push är en kinesisk molntjänst som du kan använda toosend push-meddelanden toomobile enheter. Den här tjänsten är användbar i Kina, där leverera push-meddelanden tooAndroid är komplex på grund av hello förekomsten av olika appbutiker och push-tjänsterna, dessutom toohello tillgängligheten för Android-enheter som inte är vanligtvis anslutna tooGCM (Google Cloud Messaging).

## <a name="prerequisites"></a>Krav
För den här kursen behöver du:

* Android SDK (vi antar att du använder Eclipse), som du kan hämta från hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">webbplatsen för Android</a>
* [Mobile Services Android SDK]
* [Baidu Push Android SDK]

> [!NOTE]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den kostnadsfria utvärderingsversionen av Azure finns [här](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).
> 
> 

## <a name="create-a-baidu-account"></a>Skapa ett Baidu-konto
toouse Baidu måste du ha ett Baidu-konto. Om du redan har en logga in toohello [Baidu-portalen] och hoppa över toohello nästa steg. Se annars hello följa anvisningar om hur toocreate ett Baidu-konto.  

1. Gå toohello [Baidu-portalen] och klicka på hello**登录**(**inloggning**) länk. Klicka på**立即注册**toostart hello konto registreringsprocessen.
   
   ![][1]
2. Ange information om hello krävs – telefon/e-postadress, lösenord och Verifieringskod, och klicka på **registreringen**.
   
   ![][2]
3. Du kommer att skickas en e-toohello e-postadress som du angav med en länk tooactivate Baidu-konto.
   
   ![][3]
4. Logga in tooyour e-postkonto, öppna aktiveringsmejlet hello och klicka på hello aktivering länken tooactivate Baidu-konto.
   
   ![][4]

När du har ett aktiverat Baidu-konto kan logga in toohello [Baidu-portalen].

## <a name="register-as-a-baidu-developer"></a>Registrera dig som Baidu-utvecklare.
1. När du har loggat in toohello [Baidu-portalen], klickar du på**更多 >>** (**mer**).
   
      ![][5]
2. Rulla nedåt i hello**站长与开发者服务 (webbadministratör och utvecklare tjänster)** avsnittet och klicka på**百度开放云平台**(**Baidu öppna molnplattform**).
   
      ![][6]
3. Klicka på nästa sida hello**开发者服务**(**Developer Services**) på hello övre högra hörnet.
   
      ![][7]
4. Klicka på nästa sida hello**注册开发者**(**registrerade utvecklare**) hello-menyn på hello övre högra hörnet.
   
      ![][8]
5. Ange ditt namn, en beskrivning och ett mobiltelefonnummer för att få ett textmeddelande för verifiering. Klicka sedan på **送验证码** (**Skicka verifieringskod**). Du måste tooenclose hello landskoden inom parentes för internationella telefonnummer. För ett mobiltelefonnummer i USA är det exempelvis **(1) 1234567890**.
   
      ![][9]
6. Du bör få ett textmeddelande med ett verifieringsnummer, som visas i följande exempel hello:
   
      ![][10]
7. Ange hello verifieringsnummer från hello-meddelande i**验证码**(**bekräftelsekoden**).
8. Slutför hello developer registrering genom när hello Baidu och klicka på**提交**(**skicka**). Följande sida på slutförande av registrering hello visas:
   
      ![][11]

## <a name="create-a-baidu-cloud-push-project"></a>Skapa ett Baidu Cloud Push-projekt
När du skapar ett Baidu Cloud Push-projekt, kommer du att få ett app-ID, en API-nyckel och en hemlig nyckel.

1. När du har loggat in toohello [Baidu-portalen], klickar du på**更多 >>** (**mer**).
   
      ![][5]
2. Rulla nedåt i hello**站长与开发者服务**(**webbadministratör och utvecklare tjänster**) avsnittet och klicka på**百度开放云平台**(**Baidu öppna molnplattform**).
   
      ![][6]
3. Klicka på nästa sida hello**开发者服务**(**Developer Services**) på hello övre högra hörnet.
   
      ![][7]
4. Klicka på nästa sida hello**云推送**(**Cloud Push**) från hello**云服务**(**molntjänster**) avsnitt.
   
      ![][12]
5. När du är registrerad utvecklare kan du se**管理控制台**(**hanteringskonsolen**) på hello översta menyn. Klicka på **开发者服务管理** (**Tjänstehantering för utvecklare**).
   
      ![][13]
6. Klicka på nästa sida hello**创建工程**(**skapa projekt**).
   
      ![][14]
7. Ange ett programnamn och klicka på **创建** (**Skapa**).
   
      ![][15]
8. När ett Baidu Cloud Push-projekt har skapats på rätt sätt, visas en sida med **AppID**, **API-nyckeln** och en **hemlig nyckel**. Anteckna hello API-nyckel och hemlig nyckel som ska användas senare.
   
      ![][16]
9. Konfigurera hello projekt för push-meddelanden genom att klicka på**云推送**(**Cloud Push**) hello vänster.
   
      ![][31]
10. Klicka på nästa sida hello hello**推送设置**(**Push inställningar**) knappen.
    
    ![][32]  
11. Lägg till hello paketnamn som du ska använda i din Android-projekt i hello på konfigurationssidan för hello**应用包名**(**programpaket**) fält och klicka på**保存设置**() **Spara**).  
    
    ![][33]

Du ser hello**保存成功!** (**Ändringarna har sparats!**) visas.

## <a name="configure-your-notification-hub"></a>Konfigurera meddelandehubben
1. Logga in toohello [klassiska Azure-portalen], och klicka sedan på **+ ny** längst hello hello-skärmen.
2. Klicka på **Apptjänster**, sedan på**Service Bus** och därefter på **Meddelandehubb**. Slutligen klickar du på **Snabbregistrering**.
3. Ange ett namn för din **Meddelandehubben**väljer hello **Region** och hello **Namespace** där den här meddelandehubben ska skapas och klicka sedan på  **Skapa en ny Meddelandehubb**.  
   
      ![][17]
4. Klicka på hello namnområde där du skapade din meddelandehubb och klicka sedan på **Meddelandehubbar** hello överst.
   
      ![][18]
5. Välj hello meddelandehubb som du skapat och klicka sedan på **konfigurera** hello översta menyn.
   
      ![][19]
6. Bläddra nedåt toohello **meddelandeinställningar för baidu** och ange hello API-nyckel och hemliga nyckel som du fick från konsolen för hello Baidu tidigare för Baidu cloud push-projekt. Klicka på **Spara**.
   
      ![][20]
7. Klicka på hello **instrumentpanelen** fliken hello överst för meddelandehubben hello och klicka sedan på **visa anslutningssträng**.
   
      ![][21]
8. Anteckna hello **DefaultListenSharedAccessSignature** och **DefaultFullSharedAccessSignature** från hello **anslutningsinformation för åtkomst** fönster.
   
    ![][22]

## <a name="connect-your-app-toohello-notification-hub"></a>Ansluta din app toohello notification hub
1. Skapa ett nytt Android-projekt i Eclipse ADT (**Arkiv** > **Ny** > **Android-approjekt**).
   
    ![][23]
2. Ange en **programnamn** och se till att hello **minsta nödvändiga SDK** -versionen är inställd för**API 16: Android 4.1**.
   
    ![][24]
3. Klicka på **nästa** och fortsätta följa hello guiden tills hello **Skapa aktivitet** visas. Se till att **tom aktivitet** är markerad och välj slutligen **Slutför** toocreate ett nytt Android-program.
   
    ![][25]
4. Kontrollera att hello **mål för projektgenerering** är korrekt.
   
    ![][26]
5. Hämta filen för hello notification-hubs-0.4.jar från hello **filer** för hello [Notification-Hubs-Android-SDK på Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4). Lägg till hello filen toohello **libs** i ditt Eclipse-projekt och uppdatera hello *libs* mapp.
6. Hämta och packa upp hello [Baidu Push Android SDK]öppnar hello **libs** mapp och sedan kopiera hello **pushservice x.y.z** jar-filen och hello **armeabi**  &  **mips** mappar i hello **libs** mappen för din Android-App.
7. Öppna hello **AndroidManifest.xml** för ditt Android projekt och Lägg till hello behörigheter som krävs av hello Baidu-SDK.
   
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
8. Lägg till hello **android: name** egenskapen tooyour **programmet** element i **AndroidManifest.xml**och ersätter *yourprojectname* (för exempelvis **com.example.BaiduTest**). Kontrollera att projektnamnet motsvarar hello något som du konfigurerade i hello Baidu-konsolen.
   
        <application android:name="yourprojectname.DemoApplication"
9. Lägg till följande konfiguration i hello programmet elementet efter hello hello **. MainActivity** genom ersätter *yourprojectname* (till exempel **com.example.BaiduTest**):
   
        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>
   
        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>
10. Lägg till en ny klass med namnet **ConfigurationSettings.java** toohello projekt.
    
     ![][28]
    
     ![][29]
11. Lägg till följande kod tooit hello:
    
        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }
    
    Värdet för hello **API_KEY** med det du hämtade från Baidu cloud-projekt för hello tidigare **NotificationHubName** med ditt meddelandehubbsnamn från hello klassiska Azure-portalen och  **NotificationHubConnectionString** med DefaultListenSharedAccessSignature från hello klassiska Azure-portalen.
12. Lägg till en ny klass med namnet **DemoApplication.java**, och Lägg till följande kod tooit hello:
    
        import com.baidu.frontia.FrontiaApplication;
    
        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }
13. Lägg till en annan ny klass med namnet **MyPushMessageReceiver.java**, och Lägg till följande kod tooit hello. Det är hello-klassen som hanterar hello push-meddelanden som tas emot från hello Baidu push-servern.
    
        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;
    
        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG tooLog */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();
    
            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;
    
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
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
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
            }
    
            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }
    
            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }
14. Öppna **MainActivity.java**, och Lägg till följande toohello hello **onCreate** metod:
    
            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);
15. Öppna följande importuttryck överst hello hello:
    
            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

## <a name="send-notifications-tooyour-app"></a>Skicka meddelanden tooyour appen
Du kan snabbt testa ta emot meddelanden i appen genom att skicka meddelanden i hello [Azure-portalen](https://portal.azure.com/) med hello **skicka** knappen på hello meddelandehubb som visas i följande skärmbild hello:

![](./media/notification-hubs-baidu-get-started/notification-hub-test-send-baidu.png)

Push-meddelanden skickas vanligtvis i en serverdelstjänst som Mobile Services eller ASP.NET med hjälp av ett kompatibelt bibliotek. Om ett bibliotek inte är tillgänglig för din serverdel kan du använda hello REST API direkt toosend meddelanden.

I den här självstudiekursen kommer vi enkelhet och hur du testar klientappen genom att skicka meddelanden med hello .NET SDK för meddelandehubbar i ett konsolprogram i stället för en serverdelstjänst. Vi rekommenderar hello [använda Notification Hubs toopush meddelanden toousers](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) självstudiekurs som hello nästa steg för att skicka meddelanden från en ASP.NET-serverdel. Hello följande metoder kan dock användas för att skicka meddelanden:

* **REST-gränssnittet**: du kan använda meddelanden på alla backend-plattformar med hello [REST-gränssnittet](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **Microsoft Azure Notification Hubs .NET SDK**: hello Nuget Package Manager för Visual Studio, köra [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* **Node.js**: [hur toouse Notification Hubs från Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* **Mobile Apps**: ett exempel på hur toosend meddelanden från en serverdel för Azure Apptjänst Mobilappar som är integrerad med Notification Hubs finns [Lägg till push-meddelanden tooyour mobilappen](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).
* **Java / PHP**: ett exempel på hur hello toosend meddelanden med hjälp av REST API: er, se ”hur toouse Notification Hubs från Java/PHP” ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

## <a name="optional-send-notifications-from-a-net-console-app"></a>(Valfritt) Skicka meddelanden från en .NET-konsolapp
I det här avsnittet kommer du att lära dig hur du skickar meddelanden med hjälp av en .NET-konsolapp.

1. Skapa en ny Visual C#-konsolapp:
   
    ![][30]
2. Hello fönstret Package Manager-konsolen, ange hello **standardprojektet** tooyour nya konsolen projektet och sedan köra hello följande kommando i konsolfönstret hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Den här instruktionen lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-paketet</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
3. Öppna hello filen **Program.cs** och Lägg till hello följande med instruktionen:
   
        using Microsoft.Azure.NotificationHubs;
4. I din `Program` klassen, Lägg till följande metod hello och ersätta *DefaultFullSharedAccessSignatureSASConnectionString* och *NotificationHubName* med hello-värden som du har.
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }
5. Lägg till följande rader i hello din **Main** metoden:
   
         SendNotificationAsync();
         Console.ReadLine();

## <a name="test-your-app"></a>Testa din app
den här appen med en riktig mobil bara ansluta tootest hello phone tooyour datorn med hjälp av en USB-kabel. Den här åtgärden läser din app till hello kopplade phone.

tootest appen med emulatorn hello hello Eclipse översta verktygsfältet klickar du på **kör**, och välj sedan din app: startar hello emulator, belastning och kör hello app.

hello appen hämtar hello ”userId” och ”channelId” från hello Baidu Push notification service och registrerar hello meddelandehubben.

Du kan använda hello felsökningsfliken av hello Azure klassiska Portal toosend ett testmeddelande. Om du har byggt hello .NET-konsolprogram för Visual Studio bara trycka hello F5 i Visual Studio toorun hello-program. hello skickar ett meddelande som visas i hello övre meddelandefältet i enheten eller emulatorn.

<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu Push Android SDK]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[klassiska Azure-portalen]: https://manage.windowsazure.com/
[Baidu-portalen]: http://www.baidu.com/
