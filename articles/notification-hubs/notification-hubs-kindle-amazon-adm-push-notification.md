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
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a><span data-ttu-id="a531f-103">Kom igång med Notification Hubs för Kindle-appar</span><span class="sxs-lookup"><span data-stu-id="a531f-103">Get started with Notification Hubs for Kindle apps</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="a531f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a531f-104">Overview</span></span>
<span data-ttu-id="a531f-105">Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa Kindle-App.</span><span class="sxs-lookup"><span data-stu-id="a531f-105">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa Kindle application.</span></span>
<span data-ttu-id="a531f-106">Du skapar en tom Kindle-app som tar emot push-meddelanden genom att använda Amazon Device Messaging (ADM).</span><span class="sxs-lookup"><span data-stu-id="a531f-106">You'll create a blank Kindle app that receives push notifications by using Amazon Device Messaging (ADM).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a531f-107">Krav</span><span class="sxs-lookup"><span data-stu-id="a531f-107">Prerequisites</span></span>
<span data-ttu-id="a531f-108">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="a531f-108">This tutorial requires hello following:</span></span>

* <span data-ttu-id="a531f-109">Hämta hello Android SDK (vi antar att du kommer att använda Eclipse) från hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">webbplatsen för Android</a>.</span><span class="sxs-lookup"><span data-stu-id="a531f-109">Get hello Android SDK (we assume that you will use Eclipse) from hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android site</a>.</span></span>
* <span data-ttu-id="a531f-110">Gör så hello i <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">inställningen Konfigurera din utvecklingsmiljö</a> tooset in din utvecklingsmiljö för Kindle.</span><span class="sxs-lookup"><span data-stu-id="a531f-110">Follow hello steps in <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Setting Up Your Development Environment</a> tooset up your development environment for Kindle.</span></span>

## <a name="add-a-new-app-toohello-developer-portal"></a><span data-ttu-id="a531f-111">Lägg till en ny app toohello developer-portalen</span><span class="sxs-lookup"><span data-stu-id="a531f-111">Add a new app toohello developer portal</span></span>
1. <span data-ttu-id="a531f-112">Börja med att skapa en app i hello [Amazon developer-portalen].</span><span class="sxs-lookup"><span data-stu-id="a531f-112">First, create an app in hello [Amazon developer portal].</span></span>
   
    ![][0]
2. <span data-ttu-id="a531f-113">Kopiera hello **Programnyckeln**.</span><span class="sxs-lookup"><span data-stu-id="a531f-113">Copy hello **Application Key**.</span></span>
   
    ![][1]
3. <span data-ttu-id="a531f-114">Klicka på hello namnet på din app i hello-portalen och klicka på hello **Device Messaging** fliken.</span><span class="sxs-lookup"><span data-stu-id="a531f-114">In hello portal, click hello name of your app, and then click hello **Device Messaging** tab.</span></span>
   
    ![][2]
4. <span data-ttu-id="a531f-115">Klicka på **Skapa en ny säkerhetsprofil** och skapa sedan en ny säkerhetsprofil (till exempel **säkerhetsprofilen TestAdm**).</span><span class="sxs-lookup"><span data-stu-id="a531f-115">Click **Create a New Security Profile**, and then create a new security profile (for example, **TestAdm security profile**).</span></span> <span data-ttu-id="a531f-116">Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a531f-116">Then click **Save**.</span></span>
   
    ![][3]
5. <span data-ttu-id="a531f-117">Klicka på **Säkerhetsprofiler** tooview hello säkerhetsprofil som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="a531f-117">Click **Security Profiles** tooview hello security profile that you just created.</span></span> <span data-ttu-id="a531f-118">Kopiera hello **klient-ID** och **Klienthemlighet** värden för senare användning.</span><span class="sxs-lookup"><span data-stu-id="a531f-118">Copy hello **Client ID** and **Client Secret** values for later use.</span></span>
   
    ![][4]

## <a name="create-an-api-key"></a><span data-ttu-id="a531f-119">Skapa en API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="a531f-119">Create an API key</span></span>
1. <span data-ttu-id="a531f-120">Öppna en kommandotolk med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="a531f-120">Open a command prompt with administrator privileges.</span></span>
2. <span data-ttu-id="a531f-121">Navigera toohello Android SDK-mappen.</span><span class="sxs-lookup"><span data-stu-id="a531f-121">Navigate toohello Android SDK folder.</span></span>
3. <span data-ttu-id="a531f-122">Ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a531f-122">Enter hello following command:</span></span>
   
        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
   
    ![][5]
4. <span data-ttu-id="a531f-123">För hello **keystore** lösenord, skriver **android**.</span><span class="sxs-lookup"><span data-stu-id="a531f-123">For hello **keystore** password, type **android**.</span></span>
5. <span data-ttu-id="a531f-124">Kopiera hello **MD5** fingeravtryck.</span><span class="sxs-lookup"><span data-stu-id="a531f-124">Copy hello **MD5** fingerprint.</span></span>
6. <span data-ttu-id="a531f-125">Tillbaka i hello developer-portalen på hello **Messaging** klickar du på **Android/Kindle** och ange hello hello paketets namn för din app (till exempel **com.sample.notificationhubtest**) och hello **MD5** värdet och klicka sedan på **generera API-nyckel**.</span><span class="sxs-lookup"><span data-stu-id="a531f-125">Back in hello developer portal, on hello **Messaging** tab, click **Android/Kindle** and enter hello name of hello package for your app (for example, **com.sample.notificationhubtest**) and hello **MD5** value, and then click **Generate API Key**.</span></span>

## <a name="add-credentials-toohello-hub"></a><span data-ttu-id="a531f-126">Lägg till autentiseringsuppgifter toohello hub</span><span class="sxs-lookup"><span data-stu-id="a531f-126">Add credentials toohello hub</span></span>
<span data-ttu-id="a531f-127">Lägga till hello klienten hemlighet och klient-ID toohello hello portal **konfigurera** fliken i din meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="a531f-127">In hello portal, add hello client secret and client ID toohello **Configure** tab of your notification hub.</span></span>

## <a name="set-up-your-application"></a><span data-ttu-id="a531f-128">Konfigurera din app</span><span class="sxs-lookup"><span data-stu-id="a531f-128">Set up your application</span></span>
> [!NOTE]
> <span data-ttu-id="a531f-129">När du skapar en app bör du använda minst API-nivå 17.</span><span class="sxs-lookup"><span data-stu-id="a531f-129">When you're creating an application, use at least API Level 17.</span></span>
> 
> 

<span data-ttu-id="a531f-130">Lägg till hello ADM bibliotek tooyour Eclipse-projektet:</span><span class="sxs-lookup"><span data-stu-id="a531f-130">Add hello ADM libraries tooyour Eclipse project:</span></span>

1. <span data-ttu-id="a531f-131">tooobtain hello ADM-biblioteket, [hämta hello SDK].</span><span class="sxs-lookup"><span data-stu-id="a531f-131">tooobtain hello ADM library, [download hello SDK].</span></span> <span data-ttu-id="a531f-132">Extrahera hello SDK zip-filen.</span><span class="sxs-lookup"><span data-stu-id="a531f-132">Extract hello SDK zip file.</span></span>
2. <span data-ttu-id="a531f-133">Högerklicka på projektet i Eclipse och klicka sedan på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="a531f-133">In Eclipse, right-click your project, and then click **Properties**.</span></span> <span data-ttu-id="a531f-134">Välj **Java byggsökväg** på hello vänster och välj sedan hello ** bibliotek ** fliken hello överst.</span><span class="sxs-lookup"><span data-stu-id="a531f-134">Select **Java Build Path** on hello left, and then select hello **Libraries **tab at hello top.</span></span> <span data-ttu-id="a531f-135">Klicka på **Lägg till extern Jar**, och välj hello `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` från hello katalog där du extraherade hello Amazon SDK.</span><span class="sxs-lookup"><span data-stu-id="a531f-135">Click **Add External Jar**, and select hello file `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` from hello directory in which you extracted hello Amazon SDK.</span></span>
3. <span data-ttu-id="a531f-136">Hämta hello NotificationHubs Android SDK (länk).</span><span class="sxs-lookup"><span data-stu-id="a531f-136">Download hello NotificationHubs Android SDK (link).</span></span>
4. <span data-ttu-id="a531f-137">Packa hello paketet och dra sedan filen hello `notification-hubs-sdk.jar` till hello `libs` mappen i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="a531f-137">Unzip hello package, and then drag hello file `notification-hubs-sdk.jar` into hello `libs` folder in Eclipse.</span></span>

<span data-ttu-id="a531f-138">Redigera din app manifestet toosupport ADM:</span><span class="sxs-lookup"><span data-stu-id="a531f-138">Edit your app manifest toosupport ADM:</span></span>

1. <span data-ttu-id="a531f-139">Lägg till hello namnområdet för Amazon i hello rotelementets manifest:</span><span class="sxs-lookup"><span data-stu-id="a531f-139">Add hello Amazon namespace in hello root manifest element:</span></span>

        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

1. <span data-ttu-id="a531f-140">Lägg till behörigheter som hello första elementet under manifestelementet hello.</span><span class="sxs-lookup"><span data-stu-id="a531f-140">Add permissions as hello first element under hello manifest element.</span></span> <span data-ttu-id="a531f-141">Ersätt **[YOUR PACKAGE NAME]** med hello-paket som du använde toocreate din app.</span><span class="sxs-lookup"><span data-stu-id="a531f-141">Substitute **[YOUR PACKAGE NAME]** with hello package that you used toocreate your app.</span></span>
   
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
2. <span data-ttu-id="a531f-142">Infoga följande element som hello hello App-elementets första underordnade hello.</span><span class="sxs-lookup"><span data-stu-id="a531f-142">Insert hello following element as hello first child of hello application element.</span></span> <span data-ttu-id="a531f-143">Kom ihåg toosubstitute **[YOUR SERVICE NAME]** med hello namnet på din meddelandehanterare som du skapar i nästa avsnitt om hello (inklusive hello-paket), och Ersätt **[YOUR PACKAGE NAME]** med hello paketnamnet som du skapade din app.</span><span class="sxs-lookup"><span data-stu-id="a531f-143">Remember toosubstitute **[YOUR SERVICE NAME]** with hello name of your ADM message handler that you create in hello next section (including hello package), and replace **[YOUR PACKAGE NAME]** with hello package name with which you created your app.</span></span>
   
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

## <a name="create-your-adm-message-handler"></a><span data-ttu-id="a531f-144">Skapa en meddelandehanterare för ADM</span><span class="sxs-lookup"><span data-stu-id="a531f-144">Create your ADM message handler</span></span>
1. <span data-ttu-id="a531f-145">Skapa en ny klass som ärver från `com.amazon.device.messaging.ADMMessageHandlerBase` och ger den namnet `MyADMMessageHandler`, enligt följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="a531f-145">Create a new class that inherits from `com.amazon.device.messaging.ADMMessageHandlerBase` and name it `MyADMMessageHandler`, as shown in hello following figure:</span></span>
   
    ![][6]
2. <span data-ttu-id="a531f-146">Lägg till följande hello `import` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="a531f-146">Add hello following `import` statements:</span></span>
   
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub
3. <span data-ttu-id="a531f-147">Lägg till följande kod i hello-klass som du skapade hello.</span><span class="sxs-lookup"><span data-stu-id="a531f-147">Add hello following code in hello class that you created.</span></span> <span data-ttu-id="a531f-148">Kom ihåg toosubstitute hello hubb och anslutningssträngen (lyssna):</span><span class="sxs-lookup"><span data-stu-id="a531f-148">Remember toosubstitute hello hub name and connection string (listen):</span></span>
   
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
4. <span data-ttu-id="a531f-149">Lägg till följande kod toohello hello `OnMessage()` metoden:</span><span class="sxs-lookup"><span data-stu-id="a531f-149">Add hello following code toohello `OnMessage()` method:</span></span>
   
        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);
5. <span data-ttu-id="a531f-150">Lägg till följande kod toohello hello `OnRegistered` metoden:</span><span class="sxs-lookup"><span data-stu-id="a531f-150">Add hello following code toohello `OnRegistered` method:</span></span>
   
            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }
6. <span data-ttu-id="a531f-151">Lägg till följande kod toohello hello `OnUnregistered` metoden:</span><span class="sxs-lookup"><span data-stu-id="a531f-151">Add hello following code toohello `OnUnregistered` method:</span></span>
   
         try {
             getNotificationHub(getApplicationContext()).unregister();
         } catch (Exception e) {
             Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
         }
7. <span data-ttu-id="a531f-152">I hello `MainActivity` metoden Lägg till följande importuttryck hello:</span><span class="sxs-lookup"><span data-stu-id="a531f-152">In hello `MainActivity` method, add hello following import statement:</span></span>
   
        import com.amazon.device.messaging.ADM;
8. <span data-ttu-id="a531f-153">Lägg till följande kod hello slutet av hello hello `OnCreate` metoden:</span><span class="sxs-lookup"><span data-stu-id="a531f-153">Add hello following code at hello end of hello `OnCreate` method:</span></span>
   
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

## <a name="add-your-api-key-tooyour-app"></a><span data-ttu-id="a531f-154">Lägg till nyckel tooyour API-appen</span><span class="sxs-lookup"><span data-stu-id="a531f-154">Add your API key tooyour app</span></span>
1. <span data-ttu-id="a531f-155">I Eclipse skapar du en ny fil med namnet **api_key.txt** i hello directory tillgångar i ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="a531f-155">In Eclipse, create a new file named **api_key.txt** in hello directory assets of your project.</span></span>
2. <span data-ttu-id="a531f-156">Öppna hello-filen och kopiera hello API-nyckel som du genererade på hello Amazon developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="a531f-156">Open hello file and copy hello API key that you generated in hello Amazon developer portal.</span></span>

## <a name="run-hello-app"></a><span data-ttu-id="a531f-157">Kör hello-appen</span><span class="sxs-lookup"><span data-stu-id="a531f-157">Run hello app</span></span>
1. <span data-ttu-id="a531f-158">Starta hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="a531f-158">Start hello emulator.</span></span>
2. <span data-ttu-id="a531f-159">I hello-emulatorn Svep hello uppifrån och klickar på **inställningar**, och klicka sedan på **mitt konto** och registrera med ett giltigt Amazon-konto.</span><span class="sxs-lookup"><span data-stu-id="a531f-159">In hello emulator, swipe from hello top and click **Settings**, and then click **My account** and register with a valid Amazon account.</span></span>
3. <span data-ttu-id="a531f-160">Kör hello app i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="a531f-160">In Eclipse, run hello app.</span></span>

> [!NOTE]
> <span data-ttu-id="a531f-161">Om ett problem uppstår, kontrollerar du hello tiden för emulatorn hello (eller enhet).</span><span class="sxs-lookup"><span data-stu-id="a531f-161">If a problem occurs, check hello time of hello emulator (or device).</span></span> <span data-ttu-id="a531f-162">hello tidsvärdet måste vara korrekt.</span><span class="sxs-lookup"><span data-stu-id="a531f-162">hello time value must be accurate.</span></span> <span data-ttu-id="a531f-163">toochange hello tid hello Kindle-emulatorns, kan du köra följande kommando från katalogen för plattformsverktygen Android SDK hello:</span><span class="sxs-lookup"><span data-stu-id="a531f-163">toochange hello time of hello Kindle emulator, you can run hello following command from your Android SDK platform-tools directory:</span></span>
> 
> 

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a><span data-ttu-id="a531f-164">Skicka ett meddelande</span><span class="sxs-lookup"><span data-stu-id="a531f-164">Send a message</span></span>
<span data-ttu-id="a531f-165">toosend ett meddelande med hjälp av .NET:</span><span class="sxs-lookup"><span data-stu-id="a531f-165">toosend a message by using .NET:</span></span>

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
