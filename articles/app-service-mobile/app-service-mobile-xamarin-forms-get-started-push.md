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
# <a name="add-push-notifications-tooyour-xamarinforms-app"></a><span data-ttu-id="1f572-103">Lägg till push-meddelanden tooyour Xamarin.Forms-app</span><span class="sxs-lookup"><span data-stu-id="1f572-103">Add push notifications tooyour Xamarin.Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="1f572-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1f572-104">Overview</span></span>
<span data-ttu-id="1f572-105">I kursen får du lägga till push-meddelanden tooall hello projekt som härrör från hello [Xamarin.Forms Snabbstart](app-service-mobile-xamarin-forms-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1f572-105">In this tutorial, you add push notifications tooall hello projects that resulted from hello [Xamarin.Forms quick start](app-service-mobile-xamarin-forms-get-started.md).</span></span> <span data-ttu-id="1f572-106">Det innebär att ett push-meddelande skickas tooall plattformsoberoende klienter varje gång en post infogas.</span><span class="sxs-lookup"><span data-stu-id="1f572-106">This means that a push notification is sent tooall cross-platform clients every time a record is inserted.</span></span>

<span data-ttu-id="1f572-107">Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="1f572-107">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="1f572-108">Mer information finns i [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="1f572-108">For more information, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f572-109">Krav</span><span class="sxs-lookup"><span data-stu-id="1f572-109">Prerequisites</span></span>
<span data-ttu-id="1f572-110">För iOS, behöver du en [Apple Developer Program medlemskap](https://developer.apple.com/programs/ios/) och en fysisk iOS-enhet.</span><span class="sxs-lookup"><span data-stu-id="1f572-110">For iOS, you will need an [Apple Developer Program membership](https://developer.apple.com/programs/ios/) and a physical iOS device.</span></span> <span data-ttu-id="1f572-111">Hej [stöder inte push-meddelanden i iOS-simulatorn](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span><span class="sxs-lookup"><span data-stu-id="1f572-111">hello [iOS simulator does not support push notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span></span>

## <span data-ttu-id="1f572-112"><a name="configure-hub"></a>Konfigurera en meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="1f572-112"><a name="configure-hub"></a>Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-hello-server-project-toosend-push-notifications"></a><span data-ttu-id="1f572-113">Uppdatera hello server projektet toosend push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="1f572-113">Update hello server project toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-hello-android-project-optional"></a><span data-ttu-id="1f572-114">Konfigurera och köra hello Android-projekt (valfritt)</span><span class="sxs-lookup"><span data-stu-id="1f572-114">Configure and run hello Android project (optional)</span></span>
<span data-ttu-id="1f572-115">Slutför det här avsnittet tooenable push-meddelanden för hello Xamarin.Forms Droid-projektet för Android.</span><span class="sxs-lookup"><span data-stu-id="1f572-115">Complete this section tooenable push notifications for hello Xamarin.Forms Droid project for Android.</span></span>

### <a name="enable-firebase-cloud-messaging-fcm"></a><span data-ttu-id="1f572-116">Aktivera Firebase Cloud Messaging (FCM)</span><span class="sxs-lookup"><span data-stu-id="1f572-116">Enable Firebase Cloud Messaging (FCM)</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-hello-mobile-apps-back-end-toosend-push-requests-by-using-fcm"></a><span data-ttu-id="1f572-117">Konfigurera hello Mobile Apps serverdel toosend push begäranden med hjälp av FCM</span><span class="sxs-lookup"><span data-stu-id="1f572-117">Configure hello Mobile Apps back end toosend push requests by using FCM</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-toohello-android-project"></a><span data-ttu-id="1f572-118">Lägg till push-meddelanden toohello Android-projekt</span><span class="sxs-lookup"><span data-stu-id="1f572-118">Add push notifications toohello Android project</span></span>
<span data-ttu-id="1f572-119">Hello serverdel konfigurerats med FCM, kan du lägga till komponenter och koder toohello klienten tooregister med FCM.</span><span class="sxs-lookup"><span data-stu-id="1f572-119">With hello back end configured with FCM, you can add components and codes toohello client tooregister with FCM.</span></span> <span data-ttu-id="1f572-120">Du kan också registrera för push-meddelanden med Azure Notification Hubs via hello Mobile Apps tillbaka avslutas och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="1f572-120">You can also register for push notifications with Azure Notification Hubs through hello Mobile Apps back end, and receive notifications.</span></span>

1. <span data-ttu-id="1f572-121">I hello **Droid** projekt, högerklicka på hello **komponenter** mappen och klicka på **få fler komponenter...** . Sök sedan efter hello **Google Cloud Messaging-klienten** komponenten och Lägg till den toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="1f572-121">In hello **Droid** project, right-click hello **Components** folder, and click **Get More Components...**. Then search for hello **Google Cloud Messaging Client** component and add it toohello project.</span></span> <span data-ttu-id="1f572-122">Den här komponenten stöder push-meddelanden för en Xamarin Android-projekt.</span><span class="sxs-lookup"><span data-stu-id="1f572-122">This component supports push notifications for a Xamarin Android project.</span></span>
2. <span data-ttu-id="1f572-123">Öppna projektfilen för hello MainActivity.cs och Lägg till följande instruktion hello överst i filen hello hello:</span><span class="sxs-lookup"><span data-stu-id="1f572-123">Open hello MainActivity.cs project file, and add hello following statement at hello top of hello file:</span></span>

        using Gcm.Client;
3. <span data-ttu-id="1f572-124">Lägg till följande kod toohello hello **OnCreate** efter hello anrop för**LoadApplication**:</span><span class="sxs-lookup"><span data-stu-id="1f572-124">Add hello following code toohello **OnCreate** method after hello call too**LoadApplication**:</span></span>

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
4. <span data-ttu-id="1f572-125">Lägg till en ny **CreateAndShowDialog** helper-metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1f572-125">Add a new **CreateAndShowDialog** helper method, as follows:</span></span>

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }
5. <span data-ttu-id="1f572-126">Lägg till följande kod toohello hello **MainActivity** klass:</span><span class="sxs-lookup"><span data-stu-id="1f572-126">Add hello following code toohello **MainActivity** class:</span></span>

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

    <span data-ttu-id="1f572-127">Detta visar hello aktuella **MainActivity** instans, så vi kan köra på hello huvudsakliga UI-tråden.</span><span class="sxs-lookup"><span data-stu-id="1f572-127">This exposes hello current **MainActivity** instance, so we can execute on hello main UI thread.</span></span>
6. <span data-ttu-id="1f572-128">Initiera hello `instance` variabeln hello början av hello **OnCreate** metoden på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="1f572-128">Initialize hello `instance` variable at hello beginning of hello **OnCreate** method, as follows.</span></span>

        // Set hello current instance of MainActivity.
        instance = this;
7. <span data-ttu-id="1f572-129">Lägg till en ny klass filen toohello **Droid** projektet med namnet `GcmService.cs`, och se till att hello följande **med** instruktioner finns hello överst i filen hello:</span><span class="sxs-lookup"><span data-stu-id="1f572-129">Add a new class file toohello **Droid** project named `GcmService.cs`, and make sure hello following **using** statements are present at hello top of hello file:</span></span>

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
8. <span data-ttu-id="1f572-130">Lägg till följande behörighetsbegäranden hello överst i hello filen efter hello hello **med** instruktioner och innan hello **namnområde** deklaration.</span><span class="sxs-lookup"><span data-stu-id="1f572-130">Add hello following permission requests at hello top of hello file, after hello **using** statements and before hello **namespace** declaration.</span></span>

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
9. <span data-ttu-id="1f572-131">Lägg till hello efter klass definition toohello namnområde.</span><span class="sxs-lookup"><span data-stu-id="1f572-131">Add hello following class definition toohello namespace.</span></span>

       [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
       public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
       {
           public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
       }

   > [!NOTE]
   > <span data-ttu-id="1f572-132">Ersätt **< PROJECT_NUMBER >** med din projektnumret som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="1f572-132">Replace **<PROJECT_NUMBER>** with your project number you noted earlier.</span></span>    
   >
   >
10. <span data-ttu-id="1f572-133">Ersätt hello tom **GcmService** klassen med följande kod, som använder hello nya broadcast mottagaren hello:</span><span class="sxs-lookup"><span data-stu-id="1f572-133">Replace hello empty **GcmService** class with hello following code, which uses hello new broadcast receiver:</span></span>

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }
11. <span data-ttu-id="1f572-134">Lägg till följande kod toohello hello **GcmService** klass.</span><span class="sxs-lookup"><span data-stu-id="1f572-134">Add hello following code toohello **GcmService** class.</span></span> <span data-ttu-id="1f572-135">Detta åsidosätter hello **OnRegistered** händelsehanteraren och implementerar en **registrera** metod.</span><span class="sxs-lookup"><span data-stu-id="1f572-135">This overrides hello **OnRegistered** event handler and implements a **Register** method.</span></span>

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

    <span data-ttu-id="1f572-136">Observera att den här koden använder hello `messageParam` parameter i hello mallen registrering.</span><span class="sxs-lookup"><span data-stu-id="1f572-136">Note that this code uses hello `messageParam` parameter in hello template registration.</span></span>
12. <span data-ttu-id="1f572-137">Lägg till följande kod som implementerar hello **OnMessage**:</span><span class="sxs-lookup"><span data-stu-id="1f572-137">Add hello following code that implements **OnMessage**:</span></span>

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

    <span data-ttu-id="1f572-138">Detta hanterar inkommande meddelanden och skickar dem toohello notification manager toobe visas.</span><span class="sxs-lookup"><span data-stu-id="1f572-138">This handles incoming notifications and sends them toohello notification manager toobe displayed.</span></span>
13. <span data-ttu-id="1f572-139">**GcmServiceBase** måste du även tooimplement hello **OnUnRegistered** och **VidFel** hanterare metoder som du kan göra följande:</span><span class="sxs-lookup"><span data-stu-id="1f572-139">**GcmServiceBase** also requires you tooimplement hello **OnUnRegistered** and **OnError** handler methods, which you can do as follows:</span></span>

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

<span data-ttu-id="1f572-140">Nu kan du är klar testa push-meddelanden i hello-app som körs på en Android-enhet eller hello emulatorn.</span><span class="sxs-lookup"><span data-stu-id="1f572-140">Now, you are ready test push notifications in hello app running on an Android device or hello emulator.</span></span>

### <a name="test-push-notifications-in-your-android-app"></a><span data-ttu-id="1f572-141">Testa push-meddelanden i din Android-app</span><span class="sxs-lookup"><span data-stu-id="1f572-141">Test push notifications in your Android app</span></span>
<span data-ttu-id="1f572-142">hello två första stegen krävs bara när du testar på en emulator.</span><span class="sxs-lookup"><span data-stu-id="1f572-142">hello first two steps are required only when you're testing on an emulator.</span></span>

1. <span data-ttu-id="1f572-143">Se till att du distribuerar tooor felsökning på en virtuell enhet som har Google APIs som hello mål, enligt nedan i hello Android Virtual Device manager.</span><span class="sxs-lookup"><span data-stu-id="1f572-143">Make sure that you are deploying tooor debugging on a virtual device that has Google APIs set as hello target, as shown below in hello Android Virtual Device manager.</span></span>
2. <span data-ttu-id="1f572-144">Lägg till en Google-konto toohello Android-enhet genom att klicka på **appar** > **inställningar** > **Lägg till konto**.</span><span class="sxs-lookup"><span data-stu-id="1f572-144">Add a Google account toohello Android device by clicking **Apps** > **Settings** > **Add account**.</span></span> <span data-ttu-id="1f572-145">Följ sedan hello prompter tooadd en befintlig enhet för Google-konto toohello eller toocreate en ny.</span><span class="sxs-lookup"><span data-stu-id="1f572-145">Then follow hello prompts tooadd an existing Google account toohello device, or toocreate a new one.</span></span>
3. <span data-ttu-id="1f572-146">Högerklicka i Visual Studio eller Xamarin Studio hello **Droid** projektet och klicka på **Ställ in som Startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="1f572-146">In Visual Studio or Xamarin Studio, right-click hello **Droid** project and click **Set as startup project**.</span></span>
4. <span data-ttu-id="1f572-147">Klicka på **kör** toobuild hello projektet och starta hello appen på din Android-enhet eller emulator.</span><span class="sxs-lookup"><span data-stu-id="1f572-147">Click **Run** toobuild hello project and start hello app on your Android device or emulator.</span></span>
5. <span data-ttu-id="1f572-148">Skriv en aktivitet i hello app och klicka på hello plus (**+**) ikon.</span><span class="sxs-lookup"><span data-stu-id="1f572-148">In hello app, type a task, and then click hello plus (**+**) icon.</span></span>
6. <span data-ttu-id="1f572-149">Kontrollera att ett meddelande tas emot när ett objekt läggs till.</span><span class="sxs-lookup"><span data-stu-id="1f572-149">Verify that a notification is received when an item is added.</span></span>

## <a name="configure-and-run-hello-ios-project-optional"></a><span data-ttu-id="1f572-150">Konfigurera och köra hello iOS-projektet (valfritt)</span><span class="sxs-lookup"><span data-stu-id="1f572-150">Configure and run hello iOS project (optional)</span></span>
<span data-ttu-id="1f572-151">Det här avsnittet handlar om att köra hello Xamarin iOS-projektet för iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="1f572-151">This section is for running hello Xamarin iOS project for iOS devices.</span></span> <span data-ttu-id="1f572-152">Du kan hoppa över det här avsnittet om du inte arbetar med iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="1f572-152">You can skip this section if you are not working with iOS devices.</span></span>

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-hello-notification-hub-for-apns"></a><span data-ttu-id="1f572-153">Konfigurera hello meddelandehubb för APNS</span><span class="sxs-lookup"><span data-stu-id="1f572-153">Configure hello notification hub for APNS</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

<span data-ttu-id="1f572-154">Därefter konfigurerar du hello inställningen för iOS-projekt i Xamarin Studio eller Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1f572-154">Next, you will configure hello iOS project setting in Xamarin Studio or Visual Studio.</span></span>

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-tooyour-ios-app"></a><span data-ttu-id="1f572-155">Lägg till push-meddelanden tooyour iOS-app</span><span class="sxs-lookup"><span data-stu-id="1f572-155">Add push notifications tooyour iOS app</span></span>
1. <span data-ttu-id="1f572-156">I hello **iOS** projekt, öppna AppDelegate.cs och Lägg till följande instruktion toohello överkant hello kodfilen hello.</span><span class="sxs-lookup"><span data-stu-id="1f572-156">In hello **iOS** project, open AppDelegate.cs and add hello following statement toohello top of hello code file.</span></span>

        using Newtonsoft.Json.Linq;
2. <span data-ttu-id="1f572-157">I hello **AppDelegate** klassen, lägga till en åsidosättning för hello **RegisteredForRemoteNotifications** händelse tooregister för meddelanden:</span><span class="sxs-lookup"><span data-stu-id="1f572-157">In hello **AppDelegate** class, add an override for hello **RegisteredForRemoteNotifications** event tooregister for notifications:</span></span>

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
3. <span data-ttu-id="1f572-158">I **AppDelegate**, även lägga till följande åsidosättning för hello hello **DidReceiveRemoteNotification** händelsehanteraren:</span><span class="sxs-lookup"><span data-stu-id="1f572-158">In **AppDelegate**, also add hello following override for hello **DidReceiveRemoteNotification** event handler:</span></span>

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

    <span data-ttu-id="1f572-159">Den här metoden hanterar inkommande meddelanden när hello appen körs.</span><span class="sxs-lookup"><span data-stu-id="1f572-159">This method handles incoming notifications while hello app is running.</span></span>
4. <span data-ttu-id="1f572-160">I hello **AppDelegate** klassen och Lägg till följande kod toohello hello **FinishedLaunching** metod:</span><span class="sxs-lookup"><span data-stu-id="1f572-160">In hello **AppDelegate** class, add hello following code toohello **FinishedLaunching** method:</span></span>

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    <span data-ttu-id="1f572-161">Detta möjliggör stöd för remote meddelanden och att registrera push-begäranden.</span><span class="sxs-lookup"><span data-stu-id="1f572-161">This enables support for remote notifications and requests push registration.</span></span>

<span data-ttu-id="1f572-162">Appen är nu uppdaterade toosupport push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="1f572-162">Your app is now updated toosupport push notifications.</span></span>

#### <a name="test-push-notifications-in-your-ios-app"></a><span data-ttu-id="1f572-163">Testa push-meddelanden i iOS-app</span><span class="sxs-lookup"><span data-stu-id="1f572-163">Test push notifications in your iOS app</span></span>
1. <span data-ttu-id="1f572-164">Högerklicka på hello iOS-projektet och klicka på **Set as StartUp Project**.</span><span class="sxs-lookup"><span data-stu-id="1f572-164">Right-click hello iOS project, and click **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="1f572-165">Tryck på hello **kör** knappen eller **F5** i Visual Studio toobuild hello projektet och starta hello app i en iOS-enhet.</span><span class="sxs-lookup"><span data-stu-id="1f572-165">Press hello **Run** button or **F5** in Visual Studio toobuild hello project and start hello app in an iOS device.</span></span> <span data-ttu-id="1f572-166">Klicka på **OK** tooaccept push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="1f572-166">Then click **OK** tooaccept push notifications.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1f572-167">Du måste uttryckligen godkänna push-meddelanden från din app.</span><span class="sxs-lookup"><span data-stu-id="1f572-167">You must explicitly accept push notifications from your app.</span></span> <span data-ttu-id="1f572-168">Denna begäran inträffar bara hello första gången som hello appen körs.</span><span class="sxs-lookup"><span data-stu-id="1f572-168">This request only occurs hello first time that hello app runs.</span></span>
   >
   >
3. <span data-ttu-id="1f572-169">Skriv en aktivitet i hello app och klicka på hello plus (**+**) ikon.</span><span class="sxs-lookup"><span data-stu-id="1f572-169">In hello app, type a task, and then click hello plus (**+**) icon.</span></span>
4. <span data-ttu-id="1f572-170">Kontrollera att ett meddelande tas emot och klicka sedan på **OK** toodismiss hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="1f572-170">Verify that a notification is received, and then click **OK** toodismiss hello notification.</span></span>

## <a name="configure-and-run-windows-projects-optional"></a><span data-ttu-id="1f572-171">Konfigurera och köra Windows projekt (valfritt)</span><span class="sxs-lookup"><span data-stu-id="1f572-171">Configure and run Windows projects (optional)</span></span>
<span data-ttu-id="1f572-172">Det här avsnittet handlar om att köra hello Xamarin.Forms, WinApp och WinPhone81 projekt för Windows-enheter.</span><span class="sxs-lookup"><span data-stu-id="1f572-172">This section is for running hello Xamarin.Forms WinApp and WinPhone81 projects for Windows devices.</span></span> <span data-ttu-id="1f572-173">De här stegen stöder också universella Windowsplattformen (UWP)-projekt.</span><span class="sxs-lookup"><span data-stu-id="1f572-173">These steps also support Universal Windows Platform (UWP) projects.</span></span> <span data-ttu-id="1f572-174">Du kan hoppa över det här avsnittet om du inte arbetar med Windowsenheter.</span><span class="sxs-lookup"><span data-stu-id="1f572-174">You can skip this section if you are not working with Windows devices.</span></span>

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a><span data-ttu-id="1f572-175">Registrera din Windows-app för push-meddelanden med Windows Notification Service (WNS)</span><span class="sxs-lookup"><span data-stu-id="1f572-175">Register your Windows app for push notifications with Windows Notification Service (WNS)</span></span>
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-hello-notification-hub-for-wns"></a><span data-ttu-id="1f572-176">Konfigurera hello meddelandehubb för WNS</span><span class="sxs-lookup"><span data-stu-id="1f572-176">Configure hello notification hub for WNS</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-tooyour-windows-app"></a><span data-ttu-id="1f572-177">Lägg till push-meddelanden tooyour Windows-app</span><span class="sxs-lookup"><span data-stu-id="1f572-177">Add push notifications tooyour Windows app</span></span>
1. <span data-ttu-id="1f572-178">Öppna i Visual Studio **App.xaml.cs** i en Windows projekt och Lägg till hello följande instruktioner.</span><span class="sxs-lookup"><span data-stu-id="1f572-178">In Visual Studio, open **App.xaml.cs** in a Windows project, and add hello following statements.</span></span>

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    <span data-ttu-id="1f572-179">Ersätt `<your_TodoItemManager_portable_class_namespace>` med hello namnområde för din bärbara projekt som innehåller hello `TodoItemManager` klass.</span><span class="sxs-lookup"><span data-stu-id="1f572-179">Replace `<your_TodoItemManager_portable_class_namespace>` with hello namespace of your portable project that contains hello `TodoItemManager` class.</span></span>
2. <span data-ttu-id="1f572-180">I App.xaml.cs lägger du till följande hello **InitNotificationsAsync** metod:</span><span class="sxs-lookup"><span data-stu-id="1f572-180">In App.xaml.cs, add hello following **InitNotificationsAsync** method:</span></span>

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

    <span data-ttu-id="1f572-181">Den här metoden hämtar hello push notification channel och registrerar en mall tooreceive mall meddelanden från din meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="1f572-181">This method gets hello push notification channel, and registers a template tooreceive template notifications from your notification hub.</span></span> <span data-ttu-id="1f572-182">Ett meddelande i mallen som stöder *messageParam* levereras toothis klienten.</span><span class="sxs-lookup"><span data-stu-id="1f572-182">A template notification that supports *messageParam* will be delivered toothis client.</span></span>
3. <span data-ttu-id="1f572-183">Uppdatera hello i App.xaml.cs **OnLaunched** händelsehanterare metoddefinition genom att lägga till hello `async` modifierare.</span><span class="sxs-lookup"><span data-stu-id="1f572-183">In App.xaml.cs, update hello **OnLaunched** event handler method definition by adding hello `async` modifier.</span></span> <span data-ttu-id="1f572-184">Lägg sedan till följande kodrad hello slutet av metoden hello hello:</span><span class="sxs-lookup"><span data-stu-id="1f572-184">Then add hello following line of code at hello end of hello method:</span></span>

        await InitNotificationsAsync();

    <span data-ttu-id="1f572-185">Detta säkerställer att hello registreringen av push-meddelanden skapas eller uppdateras varje gång hello appen startas.</span><span class="sxs-lookup"><span data-stu-id="1f572-185">This ensures that hello push notification registration is created or refreshed every time hello app is launched.</span></span> <span data-ttu-id="1f572-186">Det är viktigt toodo denna tooguarantee som hello WNS push kanal är alltid aktivt.</span><span class="sxs-lookup"><span data-stu-id="1f572-186">It's important toodo this tooguarantee that hello WNS push channel is always active.</span></span>  
4. <span data-ttu-id="1f572-187">I Solution Explorer för Visual Studio, öppna hello **Package.appxmanifest** fil och anger **popup-kompatibel** för**Ja** under **meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="1f572-187">In Solution Explorer for Visual Studio, open hello **Package.appxmanifest** file, and set **Toast Capable** too**Yes** under **Notifications**.</span></span>
5. <span data-ttu-id="1f572-188">Skapa hello app och kontrollera att du har några fel.</span><span class="sxs-lookup"><span data-stu-id="1f572-188">Build hello app and verify you have no errors.</span></span> <span data-ttu-id="1f572-189">Klientappen bör nu registrera för hello mallen meddelanden från hello Mobile Apps tillbaka avslutas.</span><span class="sxs-lookup"><span data-stu-id="1f572-189">Your client app should now register for hello template notifications from hello Mobile Apps back end.</span></span> <span data-ttu-id="1f572-190">Upprepa det här avsnittet för varje Windows-projekt i din lösning.</span><span class="sxs-lookup"><span data-stu-id="1f572-190">Repeat this section for every Windows project in your solution.</span></span>

#### <a name="test-push-notifications-in-your-windows-app"></a><span data-ttu-id="1f572-191">Testa push-meddelanden i Windows-appen</span><span class="sxs-lookup"><span data-stu-id="1f572-191">Test push notifications in your Windows app</span></span>
1. <span data-ttu-id="1f572-192">Högerklicka på en Windows-projekt i Visual Studio och klicka på **Ställ in som Startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="1f572-192">In Visual Studio, right-click a Windows project, and click **Set as startup project**.</span></span>
2. <span data-ttu-id="1f572-193">Tryck på hello **kör** knappen toobuild hello projektet och starta hello app.</span><span class="sxs-lookup"><span data-stu-id="1f572-193">Press hello **Run** button toobuild hello project and start hello app.</span></span>
3. <span data-ttu-id="1f572-194">Skriv ett namn för en ny todoitem i hello app och klicka sedan på hello plus (**+**) ikonen tooadd den.</span><span class="sxs-lookup"><span data-stu-id="1f572-194">In hello app, type a name for a new todoitem, and then click hello plus (**+**) icon tooadd it.</span></span>
4. <span data-ttu-id="1f572-195">Kontrollera att ett meddelande tas emot när hello-objektet har lagts till.</span><span class="sxs-lookup"><span data-stu-id="1f572-195">Verify that a notification is received when hello item is added.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f572-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f572-196">Next steps</span></span>
<span data-ttu-id="1f572-197">Mer information om push-meddelanden:</span><span class="sxs-lookup"><span data-stu-id="1f572-197">You can learn more about push notifications:</span></span>

* [<span data-ttu-id="1f572-198">Diagnostisera problem för push-meddelande</span><span class="sxs-lookup"><span data-stu-id="1f572-198">Diagnose push notification issues</span></span>](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  <span data-ttu-id="1f572-199">Det finns olika orsaker till varför meddelanden kan hämta bort eller inte hamnar på enheter.</span><span class="sxs-lookup"><span data-stu-id="1f572-199">There are various reasons why notifications may get dropped or do not end up on devices.</span></span> <span data-ttu-id="1f572-200">Det här avsnittet visas hur tooanalyze och ta reda på hello rot orsakar fel för push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="1f572-200">This topic shows you how tooanalyze and figure out hello root cause of push notification failures.</span></span>

<span data-ttu-id="1f572-201">Du kan också fortsätta på tooone av hello följande kurser:</span><span class="sxs-lookup"><span data-stu-id="1f572-201">You can also continue on tooone of hello following tutorials:</span></span>

* [<span data-ttu-id="1f572-202">Lägg till autentisering tooyour app</span><span class="sxs-lookup"><span data-stu-id="1f572-202">Add authentication tooyour app </span></span>](app-service-mobile-xamarin-forms-get-started-users.md)  
  <span data-ttu-id="1f572-203">Lär dig hur tooauthenticate användare i appen med en identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="1f572-203">Learn how tooauthenticate users of your app with an identity provider.</span></span>
* [<span data-ttu-id="1f572-204">Aktivera offlinesynkronisering av appen</span><span class="sxs-lookup"><span data-stu-id="1f572-204">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  <span data-ttu-id="1f572-205">Lär dig hur tooadd offline stöd för din app genom att använda en Mobile Apps-serverdel.</span><span class="sxs-lookup"><span data-stu-id="1f572-205">Learn how tooadd offline support for your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="1f572-206">Med offlinesynkronisering, användare kan interagera med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="1f572-206">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
