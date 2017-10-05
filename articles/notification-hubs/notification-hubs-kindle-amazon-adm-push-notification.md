---
title: "Kom igång med Azure Notification Hubs för Kindle-appar | Microsoft Docs"
description: "I den här självstudiekursen kommer du att få lära dig hur du använder Azure Notification Hubs för att skicka push-meddelanden till en Kindle-app."
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
ms.openlocfilehash: 7206f152ed7270abc62536a9ee164f7227833bcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a><span data-ttu-id="96bcb-103">Kom igång med Notification Hubs för Kindle-appar</span><span class="sxs-lookup"><span data-stu-id="96bcb-103">Get started with Notification Hubs for Kindle apps</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="96bcb-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="96bcb-104">Overview</span></span>
<span data-ttu-id="96bcb-105">I den här självstudiekursen beskrivs hur du använder Azure Notification Hubs för att skicka push-meddelanden till en Kindle-app.</span><span class="sxs-lookup"><span data-stu-id="96bcb-105">This tutorial shows you how to use Azure Notification Hubs to send push notifications to a Kindle application.</span></span>
<span data-ttu-id="96bcb-106">Du skapar en tom Kindle-app som tar emot push-meddelanden genom att använda Amazon Device Messaging (ADM).</span><span class="sxs-lookup"><span data-stu-id="96bcb-106">You'll create a blank Kindle app that receives push notifications by using Amazon Device Messaging (ADM).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="96bcb-107">Krav</span><span class="sxs-lookup"><span data-stu-id="96bcb-107">Prerequisites</span></span>
<span data-ttu-id="96bcb-108">För den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="96bcb-108">This tutorial requires the following:</span></span>

* <span data-ttu-id="96bcb-109">Hämta Android SDK (vi antar att du kommer att använda Eclipse) från <a href="http://go.microsoft.com/fwlink/?LinkId=389797">webbplatsen för Android</a>.</span><span class="sxs-lookup"><span data-stu-id="96bcb-109">Get the Android SDK (we assume that you will use Eclipse) from the <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android site</a>.</span></span>
* <span data-ttu-id="96bcb-110">Följ stegen i <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Konfigurera din utvecklingsmiljö</a> för att ställa in din utvecklingsmiljö för Kindle.</span><span class="sxs-lookup"><span data-stu-id="96bcb-110">Follow the steps in <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Setting Up Your Development Environment</a> to set up your development environment for Kindle.</span></span>

## <a name="add-a-new-app-to-the-developer-portal"></a><span data-ttu-id="96bcb-111">Lägga till en ny app i utvecklarportalen</span><span class="sxs-lookup"><span data-stu-id="96bcb-111">Add a new app to the developer portal</span></span>
1. <span data-ttu-id="96bcb-112">Börja med att skapa en app i [Amazon Developer-portalen].</span><span class="sxs-lookup"><span data-stu-id="96bcb-112">First, create an app in the [Amazon developer portal].</span></span>
   
    ![][0]
2. <span data-ttu-id="96bcb-113">Kopiera **programnyckeln**.</span><span class="sxs-lookup"><span data-stu-id="96bcb-113">Copy the **Application Key**.</span></span>
   
    ![][1]
3. <span data-ttu-id="96bcb-114">Klicka på namnet på din app i portalen och klicka sedan på fliken **Device Messaging**.</span><span class="sxs-lookup"><span data-stu-id="96bcb-114">In the portal, click the name of your app, and then click the **Device Messaging** tab.</span></span>
   
    ![][2]
4. <span data-ttu-id="96bcb-115">Klicka på **Skapa en ny säkerhetsprofil** och skapa sedan en ny säkerhetsprofil (till exempel **säkerhetsprofilen TestAdm**).</span><span class="sxs-lookup"><span data-stu-id="96bcb-115">Click **Create a New Security Profile**, and then create a new security profile (for example, **TestAdm security profile**).</span></span> <span data-ttu-id="96bcb-116">Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="96bcb-116">Then click **Save**.</span></span>
   
    ![][3]
5. <span data-ttu-id="96bcb-117">Klicka på **Säkerhetsprofiler** för att visa den säkerhetsprofil som du nyss har skapat.</span><span class="sxs-lookup"><span data-stu-id="96bcb-117">Click **Security Profiles** to view the security profile that you just created.</span></span> <span data-ttu-id="96bcb-118">Kopiera värdena för **klient-ID** och **klienthemlighet** för användning längre fram.</span><span class="sxs-lookup"><span data-stu-id="96bcb-118">Copy the **Client ID** and **Client Secret** values for later use.</span></span>
   
    ![][4]

## <a name="create-an-api-key"></a><span data-ttu-id="96bcb-119">Skapa en API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="96bcb-119">Create an API key</span></span>
1. <span data-ttu-id="96bcb-120">Öppna en kommandotolk med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="96bcb-120">Open a command prompt with administrator privileges.</span></span>
2. <span data-ttu-id="96bcb-121">Navigera till mappen Android SDK.</span><span class="sxs-lookup"><span data-stu-id="96bcb-121">Navigate to the Android SDK folder.</span></span>
3. <span data-ttu-id="96bcb-122">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="96bcb-122">Enter the following command:</span></span>
   
        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
   
    ![][5]
4. <span data-ttu-id="96bcb-123">Ange **android** som lösenord för **keystore**.</span><span class="sxs-lookup"><span data-stu-id="96bcb-123">For the **keystore** password, type **android**.</span></span>
5. <span data-ttu-id="96bcb-124">Kopiera **MD5**-fingeravtrycket.</span><span class="sxs-lookup"><span data-stu-id="96bcb-124">Copy the **MD5** fingerprint.</span></span>
6. <span data-ttu-id="96bcb-125">När du har återgått till utvecklarportalen klickar du på **Android/Kindle** på fliken **Messaging** och anger paketnamnet för din app (till exempel **com.sample.notificationhubtest**) och **MD5**-värdet. Sedan klickar du på **Generera API-nyckel**.</span><span class="sxs-lookup"><span data-stu-id="96bcb-125">Back in the developer portal, on the **Messaging** tab, click **Android/Kindle** and enter the name of the package for your app (for example, **com.sample.notificationhubtest**) and the **MD5** value, and then click **Generate API Key**.</span></span>

## <a name="add-credentials-to-the-hub"></a><span data-ttu-id="96bcb-126">Lägga till autentiseringsuppgifter i hubben</span><span class="sxs-lookup"><span data-stu-id="96bcb-126">Add credentials to the hub</span></span>
<span data-ttu-id="96bcb-127">Lägg till klienthemligheten och klient-ID:t i portalen på fliken **Konfigurera** i din meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="96bcb-127">In the portal, add the client secret and client ID to the **Configure** tab of your notification hub.</span></span>

## <a name="set-up-your-application"></a><span data-ttu-id="96bcb-128">Konfigurera din app</span><span class="sxs-lookup"><span data-stu-id="96bcb-128">Set up your application</span></span>
> [!NOTE]
> <span data-ttu-id="96bcb-129">När du skapar en app bör du använda minst API-nivå 17.</span><span class="sxs-lookup"><span data-stu-id="96bcb-129">When you're creating an application, use at least API Level 17.</span></span>
> 
> 

<span data-ttu-id="96bcb-130">Lägg till ADM-biblioteket i Eclipse-projektet:</span><span class="sxs-lookup"><span data-stu-id="96bcb-130">Add the ADM libraries to your Eclipse project:</span></span>

1. <span data-ttu-id="96bcb-131">[Ladda ned SDK] för att hämta ADM-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="96bcb-131">To obtain the ADM library, [download the SDK].</span></span> <span data-ttu-id="96bcb-132">Packa upp zip-filen för SDK.</span><span class="sxs-lookup"><span data-stu-id="96bcb-132">Extract the SDK zip file.</span></span>
2. <span data-ttu-id="96bcb-133">Högerklicka på projektet i Eclipse och klicka sedan på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="96bcb-133">In Eclipse, right-click your project, and then click **Properties**.</span></span> <span data-ttu-id="96bcb-134">Välj **Java byggsökväg** till vänster och välj sedan den ** bibliotek ** högst upp.</span><span class="sxs-lookup"><span data-stu-id="96bcb-134">Select **Java Build Path** on the left, and then select the **Libraries **tab at the top.</span></span> <span data-ttu-id="96bcb-135">Klicka på **Lägg till extern Jar** och markera filen `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` i katalogen där du extraherade Amazon SDK.</span><span class="sxs-lookup"><span data-stu-id="96bcb-135">Click **Add External Jar**, and select the file `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` from the directory in which you extracted the Amazon SDK.</span></span>
3. <span data-ttu-id="96bcb-136">Hämta NotificationHubs Android SDK (länk).</span><span class="sxs-lookup"><span data-stu-id="96bcb-136">Download the NotificationHubs Android SDK (link).</span></span>
4. <span data-ttu-id="96bcb-137">Packa upp paketet och dra sedan filen `notification-hubs-sdk.jar` till `libs`-mappen i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="96bcb-137">Unzip the package, and then drag the file `notification-hubs-sdk.jar` into the `libs` folder in Eclipse.</span></span>

<span data-ttu-id="96bcb-138">Redigera ditt app-manifest för att stödja ADM:</span><span class="sxs-lookup"><span data-stu-id="96bcb-138">Edit your app manifest to support ADM:</span></span>

1. <span data-ttu-id="96bcb-139">Lägg till namnområdet för Amazon i rotelementets manifest:</span><span class="sxs-lookup"><span data-stu-id="96bcb-139">Add the Amazon namespace in the root manifest element:</span></span>

        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

1. <span data-ttu-id="96bcb-140">Lägg till behörigheter som det första elementet under manifestelementet.</span><span class="sxs-lookup"><span data-stu-id="96bcb-140">Add permissions as the first element under the manifest element.</span></span> <span data-ttu-id="96bcb-141">Ersätt **[YOUR PACKAGE NAME]** med det paket som du använde för att skapa appen.</span><span class="sxs-lookup"><span data-stu-id="96bcb-141">Substitute **[YOUR PACKAGE NAME]** with the package that you used to create your app.</span></span>
   
        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />
   
        <uses-permission android:name="android.permission.INTERNET"/>
   
        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />
   
        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />
   
        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />
2. <span data-ttu-id="96bcb-142">Infoga följande element som app-elementets första underordnade.</span><span class="sxs-lookup"><span data-stu-id="96bcb-142">Insert the following element as the first child of the application element.</span></span> <span data-ttu-id="96bcb-143">Kom ihåg att ersätta **[YOUR SERVICE NAME]** med namnet på din  meddelandehanterare för ADM som du skapar i nästa avsnitt (inklusive paketet). Ersätt även **[YOUR PACKAGE NAME]** med det paketnamn som du skapade din app med.</span><span class="sxs-lookup"><span data-stu-id="96bcb-143">Remember to substitute **[YOUR SERVICE NAME]** with the name of your ADM message handler that you create in the next section (including the package), and replace **[YOUR PACKAGE NAME]** with the package name with which you created your app.</span></span>
   
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
   
            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />
   
          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a><span data-ttu-id="96bcb-144">Skapa en meddelandehanterare för ADM</span><span class="sxs-lookup"><span data-stu-id="96bcb-144">Create your ADM message handler</span></span>
1. <span data-ttu-id="96bcb-145">Skapa en ny klass som ärver från `com.amazon.device.messaging.ADMMessageHandlerBase` och ge den namnet `MyADMMessageHandler`, som visas på följande bild:</span><span class="sxs-lookup"><span data-stu-id="96bcb-145">Create a new class that inherits from `com.amazon.device.messaging.ADMMessageHandlerBase` and name it `MyADMMessageHandler`, as shown in the following figure:</span></span>
   
    ![][6]
2. <span data-ttu-id="96bcb-146">Lägg till följande `import`-uttryck:</span><span class="sxs-lookup"><span data-stu-id="96bcb-146">Add the following `import` statements:</span></span>
   
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub
3. <span data-ttu-id="96bcb-147">Lägg till följande kod i den klass som du har skapat.</span><span class="sxs-lookup"><span data-stu-id="96bcb-147">Add the following code in the class that you created.</span></span> <span data-ttu-id="96bcb-148">Kom ihåg att ersätta hubbnamnet och anslutningssträngen (lyssna):</span><span class="sxs-lookup"><span data-stu-id="96bcb-148">Remember to substitute the hub name and connection string (listen):</span></span>
   
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
4. <span data-ttu-id="96bcb-149">Lägg till följande kod i `OnMessage()`-metoden:</span><span class="sxs-lookup"><span data-stu-id="96bcb-149">Add the following code to the `OnMessage()` method:</span></span>
   
        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);
5. <span data-ttu-id="96bcb-150">Lägg till följande kod i `OnRegistered`-metoden:</span><span class="sxs-lookup"><span data-stu-id="96bcb-150">Add the following code to the `OnRegistered` method:</span></span>
   
            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }
6. <span data-ttu-id="96bcb-151">Lägg till följande kod i `OnUnregistered`-metoden:</span><span class="sxs-lookup"><span data-stu-id="96bcb-151">Add the following code to the `OnUnregistered` method:</span></span>
   
         try {
             getNotificationHub(getApplicationContext()).unregister();
         } catch (Exception e) {
             Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
         }
7. <span data-ttu-id="96bcb-152">Lägg till följande importuttryck i `MainActivity`-metoden:</span><span class="sxs-lookup"><span data-stu-id="96bcb-152">In the `MainActivity` method, add the following import statement:</span></span>
   
        import com.amazon.device.messaging.ADM;
8. <span data-ttu-id="96bcb-153">Lägg till följande kod i slutet av `OnCreate`-metoden:</span><span class="sxs-lookup"><span data-stu-id="96bcb-153">Add the following code at the end of the `OnCreate` method:</span></span>
   
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

## <a name="add-your-api-key-to-your-app"></a><span data-ttu-id="96bcb-154">Lägg till API-nyckeln i din app</span><span class="sxs-lookup"><span data-stu-id="96bcb-154">Add your API key to your app</span></span>
1. <span data-ttu-id="96bcb-155">I Eclipse skapar du en ny fil med namnet **api_key.txt** i ditt projekts katalogtillgångar.</span><span class="sxs-lookup"><span data-stu-id="96bcb-155">In Eclipse, create a new file named **api_key.txt** in the directory assets of your project.</span></span>
2. <span data-ttu-id="96bcb-156">Öppna filen och kopiera den API-nyckel som du genererade på Amazon Developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="96bcb-156">Open the file and copy the API key that you generated in the Amazon developer portal.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="96bcb-157">Kör appen</span><span class="sxs-lookup"><span data-stu-id="96bcb-157">Run the app</span></span>
1. <span data-ttu-id="96bcb-158">Starta emulatorn.</span><span class="sxs-lookup"><span data-stu-id="96bcb-158">Start the emulator.</span></span>
2. <span data-ttu-id="96bcb-159">I emulatorn sveper du uppifrån och klickar på **Inställningar**. Sedan klickar du på **Mitt konto** och registrerar dig med ett giltigt Amazon-konto.</span><span class="sxs-lookup"><span data-stu-id="96bcb-159">In the emulator, swipe from the top and click **Settings**, and then click **My account** and register with a valid Amazon account.</span></span>
3. <span data-ttu-id="96bcb-160">Kör appen i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="96bcb-160">In Eclipse, run the app.</span></span>

> [!NOTE]
> <span data-ttu-id="96bcb-161">Om problem uppstår, kontrollerar du tiden för emulatorn (eller enheten).</span><span class="sxs-lookup"><span data-stu-id="96bcb-161">If a problem occurs, check the time of the emulator (or device).</span></span> <span data-ttu-id="96bcb-162">Tidsvärdet måste vara korrekt.</span><span class="sxs-lookup"><span data-stu-id="96bcb-162">The time value must be accurate.</span></span> <span data-ttu-id="96bcb-163">Om du vill ändra Kindle-emulatorns tid kan du köra följande kommando från katalogen för plattformsverktygen för Android SDK:</span><span class="sxs-lookup"><span data-stu-id="96bcb-163">To change the time of the Kindle emulator, you can run the following command from your Android SDK platform-tools directory:</span></span>
> 
> 

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a><span data-ttu-id="96bcb-164">Skicka ett meddelande</span><span class="sxs-lookup"><span data-stu-id="96bcb-164">Send a message</span></span>
<span data-ttu-id="96bcb-165">Att skicka ett meddelande med hjälp av .NET:</span><span class="sxs-lookup"><span data-stu-id="96bcb-165">To send a message by using .NET:</span></span>

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
<span data-ttu-id="96bcb-166">[Amazon Developer-portalen]: https://developer.amazon.com/home.html</span><span class="sxs-lookup"><span data-stu-id="96bcb-166">[Amazon developer portal]: https://developer.amazon.com/home.html</span></span>
<span data-ttu-id="96bcb-167">[Ladda ned SDK]: https://developer.amazon.com/public/resources/development-tools/sdk</span><span class="sxs-lookup"><span data-stu-id="96bcb-167">[download the SDK]: https://developer.amazon.com/public/resources/development-tools/sdk</span></span>

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
