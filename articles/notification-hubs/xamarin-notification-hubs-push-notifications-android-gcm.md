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
# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a><span data-ttu-id="27cfb-103">Komma igång med Notification Hubs med Xamarin för Android</span><span class="sxs-lookup"><span data-stu-id="27cfb-103">Get started with Notification Hubs with Xamarin for Android</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="27cfb-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="27cfb-104">Overview</span></span>
<span data-ttu-id="27cfb-105">Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa Xamarin.Android-program.</span><span class="sxs-lookup"><span data-stu-id="27cfb-105">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa Xamarin.Android application.</span></span>
<span data-ttu-id="27cfb-106">Du skapar en tom Xamarin.Android-app som tar emot push-meddelanden genom att använda Google Cloud Messaging (GCM).</span><span class="sxs-lookup"><span data-stu-id="27cfb-106">You'll create a blank Xamarin.Android app that receives push notifications by using Google Cloud Messaging (GCM).</span></span> <span data-ttu-id="27cfb-107">När du är klar, kommer du att kunna toouse din notification hub toobroadcast push-meddelanden tooall hello enheter som kör appen.</span><span class="sxs-lookup"><span data-stu-id="27cfb-107">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span> <span data-ttu-id="27cfb-108">hello färdiga koden finns i hello [NotificationHubs app] [ GitHub] exempel.</span><span class="sxs-lookup"><span data-stu-id="27cfb-108">hello finished code is available in hello [NotificationHubs app][GitHub] sample.</span></span>

<span data-ttu-id="27cfb-109">Den här kursen visar hello enkelt scenario för sändning med Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="27cfb-109">This tutorial demonstrates hello simple broadcast scenario in using Notification Hubs.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="27cfb-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="27cfb-110">Before you begin</span></span>
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="27cfb-111">hello slutförts koden för den här kursen finns på GitHub [här](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).</span><span class="sxs-lookup"><span data-stu-id="27cfb-111">hello completed code for this tutorial can be found on GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27cfb-112">Krav</span><span class="sxs-lookup"><span data-stu-id="27cfb-112">Prerequisites</span></span>
<span data-ttu-id="27cfb-113">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="27cfb-113">This tutorial requires hello following:</span></span>

* <span data-ttu-id="27cfb-114">Visual Studio med Xamarin i Windows eller Xamarin Studio i Mac OS X. Fullständiga installationsanvisningar finns i avsnittet [Konfiguration och installation av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="27cfb-114">Visual Studio with Xamarin on Windows or Xamarin Studio on Mac OS X. Complete installation instructions are on [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span>
* <span data-ttu-id="27cfb-115">Aktivt Google-konto</span><span class="sxs-lookup"><span data-stu-id="27cfb-115">Active Google account</span></span>
* <span data-ttu-id="27cfb-116">[Azure Messaging-komponent]</span><span class="sxs-lookup"><span data-stu-id="27cfb-116">[Azure Messaging Component]</span></span>
* <span data-ttu-id="27cfb-117">[Google Cloud Messaging-klientkomponent]</span><span class="sxs-lookup"><span data-stu-id="27cfb-117">[Google Cloud Messaging Client Component]</span></span>

<span data-ttu-id="27cfb-118">Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Notification Hubs för Xamarin.Android-appar.</span><span class="sxs-lookup"><span data-stu-id="27cfb-118">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Xamarin.Android apps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="27cfb-119">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="27cfb-119">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="27cfb-120">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="27cfb-120">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="27cfb-121">Mer information om den kostnadsfria utvärderingsversionen av Azure finns [Kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="27cfb-121">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).</span></span>
> 
> 

## <a name="enable-google-cloud-messaging"></a><span data-ttu-id="27cfb-122">Aktivera Google Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="27cfb-122">Enable Google Cloud Messaging</span></span>
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a><span data-ttu-id="27cfb-123">Konfigurera meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="27cfb-123">Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li><p><span data-ttu-id="27cfb-124">Klicka på hello <b>konfigurera</b> hello överst och anger hello <b>API-nyckel</b> du fick i hello föregående avsnitt och klickar sedan på <b>spara</b>.</span><span class="sxs-lookup"><span data-stu-id="27cfb-124">Click hello <b>Configure</b> tab at hello top, enter hello <b>API Key</b> value you obtained in hello previous section, and then click <b>Save</b>.</span></span></p>
</li>
</ol>
<span data-ttu-id="27cfb-125">&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)</span><span class="sxs-lookup"><span data-stu-id="27cfb-125">&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)</span></span>

<span data-ttu-id="27cfb-126">Din meddelandehubb har nu konfigurerat toowork med GCM och du har hello anslutning strängar tooboth registrera din app tooreceive meddelanden och toosend push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="27cfb-126">Your notification hub is now configured toowork with GCM, and you have hello connection strings tooboth register your app tooreceive notifications and toosend push notifications.</span></span>

## <a name="connect-your-app-toohello-notification-hub"></a><span data-ttu-id="27cfb-127">Ansluta din app toohello notification hub</span><span class="sxs-lookup"><span data-stu-id="27cfb-127">Connect your app toohello notification hub</span></span>
### <a name="create-a-new-project"></a><span data-ttu-id="27cfb-128">Skapa ett nytt projekt</span><span class="sxs-lookup"><span data-stu-id="27cfb-128">Create a new project</span></span>
1. <span data-ttu-id="27cfb-129">I Xamarin Studio klickar du på **Ny lösning**, sedan på **Android-app** och till sist på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-129">In Xamarin Studio, click **New Solution**, click **Android App**, and click **Next**.</span></span>
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. <span data-ttu-id="27cfb-130">Ange **appens namn** och **ID**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-130">Enter your **App Name** and **Identifier**.</span></span> <span data-ttu-id="27cfb-131">Klicka på hello **målplattformar** du toosupport och klickar sedan på **nästa** och **skapa**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-131">Click hello **Target Plaforms** you want toosupport and then click **Next** and **Create**.</span></span>
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)

    <span data-ttu-id="27cfb-132">På så sätt skapas ett nytt Android-projekt.</span><span class="sxs-lookup"><span data-stu-id="27cfb-132">This creates a new Android project.</span></span>

1. <span data-ttu-id="27cfb-133">Öppna hello projektegenskaperna genom att högerklicka på det nya projektet i hello lösning visa och välja **alternativ**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-133">Open hello project properties by right-clicking your new project in hello Solution view and choosing **Options**.</span></span> <span data-ttu-id="27cfb-134">Välj hello **Android-program** objekt i hello **skapa** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="27cfb-134">Select hello **Android Application** item in hello **Build** section.</span></span>
   
    <span data-ttu-id="27cfb-135">Se till att den första bokstaven hello i din **paketnamnet** gemena.</span><span class="sxs-lookup"><span data-stu-id="27cfb-135">Ensure that hello first letter of your **Package name** is lowercase.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="27cfb-136">hello första bokstaven i paketnamnet hello måste vara gemener.</span><span class="sxs-lookup"><span data-stu-id="27cfb-136">hello first letter of hello package name must be lowercase.</span></span> <span data-ttu-id="27cfb-137">Annars visas appmanifestfel när du registrerar **BroadcastReceiver** och **IntentFilter** för push-meddelanden nedan.</span><span class="sxs-lookup"><span data-stu-id="27cfb-137">Otherwise, you will receive application manifest errors when you register your **BroadcastReceiver** and **IntentFilter** for push notifications below.</span></span>
   > 
   > 
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)
2. <span data-ttu-id="27cfb-138">Du kan också ange hello **lägsta Android-versionen** tooanother API-nivå.</span><span class="sxs-lookup"><span data-stu-id="27cfb-138">Optionally, set hello **Minimum Android version** tooanother API Level.</span></span>
3. <span data-ttu-id="27cfb-139">Du kan också ange hello **Målversionen av Android** toohello en annan API-versionen som du vill tootarget (måste vara API-nivå 8 eller senare).</span><span class="sxs-lookup"><span data-stu-id="27cfb-139">Optionally, set hello **Target Android version** toohello another API version that you want tootarget (must be API level 8 or higher).</span></span>

<span data-ttu-id="27cfb-140">Klicka på **OK** och Stäng hello Projektalternativ.</span><span class="sxs-lookup"><span data-stu-id="27cfb-140">Click **OK** and close hello Project Options dialog.</span></span>

### <a name="add-hello-required-components-tooyour-project"></a><span data-ttu-id="27cfb-141">Lägga till hello nödvändiga komponenter tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="27cfb-141">Add hello required components tooyour project</span></span>
<span data-ttu-id="27cfb-142">hello Google Cloud Messaging-klienten på hello Xamarin-Komponentlagret gör hello enklare att stödja push-meddelanden i Xamarin.Android.</span><span class="sxs-lookup"><span data-stu-id="27cfb-142">hello Google Cloud Messaging Client available on hello Xamarin Component Store simplifies hello process of supporting push notifications in Xamarin.Android.</span></span>

1. <span data-ttu-id="27cfb-143">Högerklicka på mappen för hello komponenter i Xamarin.Android-appen och välj **få fler komponenter**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-143">Right-click hello Components folder in Xamarin.Android app and choose **Get More Components**.</span></span>
2. <span data-ttu-id="27cfb-144">Sök efter hello **Azure Messaging** komponenten och Lägg till den toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="27cfb-144">Search for hello **Azure Messaging** component and add it toohello project.</span></span>
3. <span data-ttu-id="27cfb-145">Sök efter hello **Google Cloud Messaging-klienten** komponenten och Lägg till den toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="27cfb-145">Search for hello **Google Cloud Messaging Client** component and add it toohello project.</span></span>

### <a name="set-up-notification-hubs-in-your-project"></a><span data-ttu-id="27cfb-146">Konfigurera meddelandehubbar i projektet</span><span class="sxs-lookup"><span data-stu-id="27cfb-146">Set up notification hubs in your project</span></span>
1. <span data-ttu-id="27cfb-147">Samla in följande information för din Android-appen och meddelandehubben hello:</span><span class="sxs-lookup"><span data-stu-id="27cfb-147">Gather hello following information for your Android app and notification hub:</span></span>
   
   * <span data-ttu-id="27cfb-148">**Google-projektnummer**: hämta värdet för den här projektnumret från hello översikten över appen på hello Google Developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="27cfb-148">**GoogleProjectNumber**:  Get this Project Number value from hello overview of your app on hello Google Developer Portal.</span></span> <span data-ttu-id="27cfb-149">Du antecknade värdet tidigare när du skapade hello app på hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="27cfb-149">You made a note of this value earlier when you created hello app on hello portal.</span></span>
   * <span data-ttu-id="27cfb-150">**Lyssna anslutningssträng**: på hello instrumentpanelen i hello [klassiska Azure-portalen], klickar du på **visa anslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-150">**Listen connection string**: On hello dashboard in hello [Azure Classic Portal], click **View connection strings**.</span></span> <span data-ttu-id="27cfb-151">Kopiera hello *DefaultListenSharedAccessSignature* anslutningssträngen för det här värdet.</span><span class="sxs-lookup"><span data-stu-id="27cfb-151">Copy hello *DefaultListenSharedAccessSignature* connection string for this value.</span></span>
   * <span data-ttu-id="27cfb-152">**Hubbnamn**: är hello namnet på hubben från hello [klassiska Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="27cfb-152">**Hub name**: This is hello name of your hub from hello [Azure Classic Portal].</span></span> <span data-ttu-id="27cfb-153">Till exempel *mynotificationhub2*.</span><span class="sxs-lookup"><span data-stu-id="27cfb-153">For example, *mynotificationhub2*.</span></span>
     
     <span data-ttu-id="27cfb-154">Skapa en **Constants.cs** klass för Xamarin-projektet och definiera följande konstanta värden i hello klass hello.</span><span class="sxs-lookup"><span data-stu-id="27cfb-154">Create a **Constants.cs** class for your Xamarin project and define hello following constant values in hello class.</span></span> <span data-ttu-id="27cfb-155">Ersätt hello platshållarna med värdena.</span><span class="sxs-lookup"><span data-stu-id="27cfb-155">Replace hello placeholders with your values.</span></span>
     
       <span data-ttu-id="27cfb-156">offentlig statisk klass Konstanter   {</span><span class="sxs-lookup"><span data-stu-id="27cfb-156">public static class Constants   {</span></span>
     
           public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
       <span data-ttu-id="27cfb-157">}</span><span class="sxs-lookup"><span data-stu-id="27cfb-157">}</span></span>
2. <span data-ttu-id="27cfb-158">Lägg till hello följande using-instruktioner för**MainActivity.cs**:</span><span class="sxs-lookup"><span data-stu-id="27cfb-158">Add hello following using statements too**MainActivity.cs**:</span></span>
   
        using Android.Util;
        using Gcm.Client;
3. <span data-ttu-id="27cfb-159">Lägg till en variabel toohello instans `MainActivity` klass som ska använda tooshow en varningsruta när hello appen körs:</span><span class="sxs-lookup"><span data-stu-id="27cfb-159">Add an instance variable toohello `MainActivity` class that will be used tooshow an alert dialog when hello app is running:</span></span>
   
        public static MainActivity instance;
4. <span data-ttu-id="27cfb-160">Skapa följande metod i hello hello **MainActivity** klass:</span><span class="sxs-lookup"><span data-stu-id="27cfb-160">Create hello following method in hello **MainActivity** class:</span></span>
   
        private void RegisterWithGCM()
        {
            // Check tooensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);
   
            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }
5. <span data-ttu-id="27cfb-161">I hello `OnCreate` metod för **MainActivity.cs**, initiera hello `instance` variabeln och Lägg till ett anrop för`RegisterWithGCM`:</span><span class="sxs-lookup"><span data-stu-id="27cfb-161">In hello `OnCreate` method of **MainActivity.cs**, initialize hello `instance` variable and add a call too`RegisterWithGCM`:</span></span>
   
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
6. <span data-ttu-id="27cfb-162">Skapa en ny klass, **MyBroadcastReceiver**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-162">Create a new class, **MyBroadcastReceiver**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="27cfb-163">Nedan går vi igenom hur du skapar en **BroadcastReceiver**-klass från grunden.</span><span class="sxs-lookup"><span data-stu-id="27cfb-163">We will walk through creating a **BroadcastReceiver** class from scratch below.</span></span> <span data-ttu-id="27cfb-164">Dock skapa en snabb alternativa toomanually **MyBroadcastReceiver.cs** är toorefer toohello **GcmService.cs** filen i hello Xamarin.Android exempelprojektet medföljer hello [NotificationHubs exempel][GitHub].</span><span class="sxs-lookup"><span data-stu-id="27cfb-164">However, a quick alternative toomanually creating **MyBroadcastReceiver.cs** is toorefer toohello **GcmService.cs** file found in hello sample Xamarin.Android project included with hello [NotificationHubs samples][GitHub].</span></span> <span data-ttu-id="27cfb-165">Duplicera **GcmService.cs** och ändra klassnamn kan vara en bra toostart samt.</span><span class="sxs-lookup"><span data-stu-id="27cfb-165">Duplicating **GcmService.cs** and changing class names can be a great place toostart as well.</span></span>
   > 
   > 
7. <span data-ttu-id="27cfb-166">Lägg till hello följande using-instruktioner för**MyBroadcastReceiver.cs** (hänvisar toohello komponenten och sammansättningen som du lade till tidigare):</span><span class="sxs-lookup"><span data-stu-id="27cfb-166">Add hello following using statements too**MyBroadcastReceiver.cs** (referring toohello component and assembly that you added earlier):</span></span>
   
        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;
8. <span data-ttu-id="27cfb-167">I **MyBroadcastReceiver.cs**, Lägg till följande behörighetsbegäranden mellan hello hello **med** -satser och hello **namnområde** deklaration:</span><span class="sxs-lookup"><span data-stu-id="27cfb-167">In **MyBroadcastReceiver.cs**, add hello following permission requests between hello **using** statements and hello **namespace** declaration:</span></span>
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
9. <span data-ttu-id="27cfb-168">I **MyBroadcastReceiver.cs**, ändra hello **MyBroadcastReceiver** klassen toomatch hello följande:</span><span class="sxs-lookup"><span data-stu-id="27cfb-168">In **MyBroadcastReceiver.cs**, change hello **MyBroadcastReceiver** class toomatch hello following:</span></span>
   
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
10. <span data-ttu-id="27cfb-169">Lägg till ytterligare en klass i **MyBroadcastReceiver.cs** med namnet **PushHandlerService**, som kommer från **GcmServiceBase**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-169">Add another class in **MyBroadcastReceiver.cs** named **PushHandlerService**, which derives from **GcmServiceBase**.</span></span> <span data-ttu-id="27cfb-170">Se till att tooapply hello **Service** attributet toohello klass:</span><span class="sxs-lookup"><span data-stu-id="27cfb-170">Make sure tooapply hello **Service** attribute toohello class:</span></span>
    
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
11. <span data-ttu-id="27cfb-171">**GcmServiceBase** implementerar metoderna **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()** och **OnError()**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-171">**GcmServiceBase** implements methods **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()**, and **OnError()**.</span></span> <span data-ttu-id="27cfb-172">Vår **PushHandlerService** implementeringsklass måste åsidosätta de här metoderna och metoderna utlöses som svar toointeracting hello meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="27cfb-172">Our **PushHandlerService** implementation class must override these methods, and these methods will fire in response toointeracting with hello notification hub.</span></span>
12. <span data-ttu-id="27cfb-173">Åsidosätt hello **OnRegistered()** metod i **PushHandlerService** med hjälp av hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="27cfb-173">Override hello **OnRegistered()** method in **PushHandlerService** by using hello following code:</span></span>
    
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
    > <span data-ttu-id="27cfb-174">I hello **OnRegistered()** code ovan, Observera hello möjlighet toospecify taggar tooregister för vissa meddelandekanaler.</span><span class="sxs-lookup"><span data-stu-id="27cfb-174">In hello **OnRegistered()** code above, you should note hello ability toospecify tags tooregister for specific messaging channels.</span></span>
    > 
    > 
13. <span data-ttu-id="27cfb-175">Åsidosätt hello **OnMessage** metod i **PushHandlerService** med hjälp av hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="27cfb-175">Override hello **OnMessage** method in **PushHandlerService** by using hello following code:</span></span>
    
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
14. <span data-ttu-id="27cfb-176">Lägg till följande hello **createNotification** och **dialogNotify** metoder för**PushHandlerService** för att meddela användaren när ett meddelande tas emot.</span><span class="sxs-lookup"><span data-stu-id="27cfb-176">Add hello following **createNotification** and **dialogNotify** methods too**PushHandlerService** for notifying users when a notification is received.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="27cfb-177">Meddelandedesign i Android-versionen 5.0 eller senare skiljer sig avsevärt från tidigare versioner.</span><span class="sxs-lookup"><span data-stu-id="27cfb-177">Notification design in Android version 5.0 and later represents a significant departure from that of previous versions.</span></span> <span data-ttu-id="27cfb-178">Om du testar detta i Android 5.0 eller senare, behöver hello app toobe kör tooreceive hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="27cfb-178">If you test this on Android 5.0 or later, hello app will need toobe running tooreceive hello notification.</span></span> <span data-ttu-id="27cfb-179">Mer information om Android-meddelanden finns [här](http://go.microsoft.com/fwlink/?LinkId=615880).</span><span class="sxs-lookup"><span data-stu-id="27cfb-179">For more information, see [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880).</span></span>
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
15. <span data-ttu-id="27cfb-180">Åsidosätt de abstrakta medlemmarna **OnUnRegistered()**, **OnRecoverableError()** och **OnError()** så att din kod kompileras:</span><span class="sxs-lookup"><span data-stu-id="27cfb-180">Override abstract members **OnUnRegistered()**, **OnRecoverableError()**, and **OnError()** so that your code compiles:</span></span>
    
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

## <a name="run-your-app-in-hello-emulator"></a><span data-ttu-id="27cfb-181">Kör appen i emulatorn hello</span><span class="sxs-lookup"><span data-stu-id="27cfb-181">Run your app in hello emulator</span></span>
<span data-ttu-id="27cfb-182">Om du kör den här appen i emulatorn hello, se till att du använder en Android Virtual Device (AVD) som har stöd för Google APIs.</span><span class="sxs-lookup"><span data-stu-id="27cfb-182">If you run this app in hello emulator, make sure that you use an Android Virtual Device (AVD) that supports Google APIs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="27cfb-183">I ordning tooreceive push-meddelanden, måste du ställa in ett Google-konto på din Android-enhet.</span><span class="sxs-lookup"><span data-stu-id="27cfb-183">In order tooreceive push notifications, you must set up a Google account on your Android Virtual Device.</span></span> <span data-ttu-id="27cfb-184">(I hello emulatorn navigerar för**inställningar** och på **Lägg till konto**.) Kontrollera också att hello-emulatorn är anslutna toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="27cfb-184">(In hello emulator, navigate too**Settings** and click **Add Account**.) Also, make sure that hello emulator is connected toohello Internet.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="27cfb-185">Meddelandedesign i Android-versionen 5.0 eller senare skiljer sig avsevärt från tidigare versioner.</span><span class="sxs-lookup"><span data-stu-id="27cfb-185">Notification design in Android version 5.0 and later represents a significant departure from that of previous versions.</span></span> <span data-ttu-id="27cfb-186">Mer information om Android-meddelanden finns [här](http://go.microsoft.com/fwlink/?LinkId=615880).</span><span class="sxs-lookup"><span data-stu-id="27cfb-186">For more information, see [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880).</span></span>
> 
> 

1. <span data-ttu-id="27cfb-187">I **Verktyg** klickar du på **Open Android Emulator Manager** (Öppna hanteraren för Android-emulator), väljer enheten och klickar sedan på **Redigera**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-187">From **Tools**, click **Open Android Emulator Manager**, select your device, and then click **Edit**.</span></span>
   
      ![][18]
2. <span data-ttu-id="27cfb-188">Välj **Google APIs** (API:er för Google) i **Mål** och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-188">Select **Google APIs** in **Target**, and then click **OK**.</span></span>
   
      ![][19]
3. <span data-ttu-id="27cfb-189">På hello översta verktygsfältet **kör**, och välj sedan din app.</span><span class="sxs-lookup"><span data-stu-id="27cfb-189">On hello top toolbar, click **Run**, and then select your app.</span></span> <span data-ttu-id="27cfb-190">Detta startar hello-emulatorn och kör hello-app.</span><span class="sxs-lookup"><span data-stu-id="27cfb-190">This starts hello emulator and runs hello app.</span></span>
   
   <span data-ttu-id="27cfb-191">hello appen hämtar hello *registrationId* från GCM och registreras med hello notification hub.</span><span class="sxs-lookup"><span data-stu-id="27cfb-191">hello app retrieves hello *registrationId* from GCM and registers with hello notification hub.</span></span>

## <a name="send-notifications-from-your-backend"></a><span data-ttu-id="27cfb-192">Skicka meddelanden från serverdelen</span><span class="sxs-lookup"><span data-stu-id="27cfb-192">Send notifications from your backend</span></span>
<span data-ttu-id="27cfb-193">Du kan testa att ta emot meddelanden i appen genom att skicka meddelanden i hello [klassiska Azure-portalen] via hello debug fliken på hello meddelandehubb som visas i hello-skärmen nedan.</span><span class="sxs-lookup"><span data-stu-id="27cfb-193">You can test receiving notifications in your app by sending notifications in hello [Azure Classic Portal] via hello debug tab on hello notification hub, as shown in hello screen below.</span></span>

![][30]

<span data-ttu-id="27cfb-194">Push-meddelanden skickas vanligtvis via en serverdelstjänst som Mobile Services eller ASP.NET med hjälp av ett kompatibelt bibliotek.</span><span class="sxs-lookup"><span data-stu-id="27cfb-194">Push notifications are normally sent in a backend service like Mobile Services or ASP.NET through a compatible library.</span></span> <span data-ttu-id="27cfb-195">Du kan också använda hello REST API direkt toosend meddelande meddelanden om ett bibliotek inte är tillgänglig för din serverdel.</span><span class="sxs-lookup"><span data-stu-id="27cfb-195">You can also use hello REST API directly toosend notification messages if a library is not available for your backend.</span></span>

<span data-ttu-id="27cfb-196">Här följer en lista med andra självstudiekurser som du kan ha tooreview för att skicka meddelanden:</span><span class="sxs-lookup"><span data-stu-id="27cfb-196">Here is a list of some other tutorials that you may want tooreview for sending notifications:</span></span>

* <span data-ttu-id="27cfb-197">ASP.NET: Se [använda Notification Hubs toopush meddelanden toousers].</span><span class="sxs-lookup"><span data-stu-id="27cfb-197">ASP.NET: See [Use Notification Hubs toopush notifications toousers].</span></span>
* <span data-ttu-id="27cfb-198">Azure Notification Hubs Java SDK: se [hur toouse Notification Hubs från Java](notification-hubs-java-push-notification-tutorial.md) för att skicka meddelanden från Java.</span><span class="sxs-lookup"><span data-stu-id="27cfb-198">Azure Notification Hubs Java SDK: See [How toouse Notification Hubs from Java](notification-hubs-java-push-notification-tutorial.md) for sending notifications from Java.</span></span> <span data-ttu-id="27cfb-199">Det här har testats i Eclipse för Android-utveckling.</span><span class="sxs-lookup"><span data-stu-id="27cfb-199">This has been tested in Eclipse for Android Development.</span></span>
* <span data-ttu-id="27cfb-200">PHP: Se [hur toouse Notification Hubs från PHP](notification-hubs-php-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="27cfb-200">PHP: See [How toouse Notification Hubs from PHP](notification-hubs-php-push-notification-tutorial.md).</span></span>

<span data-ttu-id="27cfb-201">I hello nästa underavsnitt i självstudiekursen hello skickar du meddelanden med hjälp av en .NET-konsolapp och genom att använda en Mobiltjänst via ett nod-skript.</span><span class="sxs-lookup"><span data-stu-id="27cfb-201">In hello next subsections of hello tutorial, you send notifications by using a .NET console app, and by using a mobile service through a node script.</span></span>

#### <a name="optional-send-notifications-by-using-a-net-app"></a><span data-ttu-id="27cfb-202">(Valfritt) Skicka meddelanden med hjälp av en .NET-app</span><span class="sxs-lookup"><span data-stu-id="27cfb-202">(Optional) Send notifications by using a .NET app</span></span>
<span data-ttu-id="27cfb-203">I det här avsnittet skickar du meddelanden med hjälp av en .NET-konsolapp</span><span class="sxs-lookup"><span data-stu-id="27cfb-203">In this section, we will send notifications by using a .NET console app</span></span>

1. <span data-ttu-id="27cfb-204">Skapa en ny Visual C#-konsolapp:</span><span class="sxs-lookup"><span data-stu-id="27cfb-204">Create a new Visual C# console application:</span></span>
   
      ![][20]
2. <span data-ttu-id="27cfb-205">I Visual Studio klickar du på **Verktyg**, **NuGet Package Manager** och sedan på **Package Manager-konsolen**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-205">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="27cfb-206">Detta visar hello Package Manager-konsolen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="27cfb-206">This displays hello Package Manager Console in Visual Studio.</span></span>
3. <span data-ttu-id="27cfb-207">Hello fönstret Package Manager-konsolen, ange hello **standardprojektet** tooyour nya konsolen projektet och sedan köra hello följande kommando i konsolfönstret hello:</span><span class="sxs-lookup"><span data-stu-id="27cfb-207">In hello Package Manager Console window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="27cfb-208">Detta lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-paketet</a>.</span><span class="sxs-lookup"><span data-stu-id="27cfb-208">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="27cfb-209">Öppna hello Program.cs-filen och Lägg till följande hello `using` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="27cfb-209">Open hello Program.cs file and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="27cfb-210">I din `Program` klassen, lägga till hello följande metod.</span><span class="sxs-lookup"><span data-stu-id="27cfb-210">In your `Program` class, add hello following method.</span></span> <span data-ttu-id="27cfb-211">Uppdatera hello platshållartexten med din *DefaultFullSharedAccessSignature* anslutning sträng och hubbens namn från hello [klassiska Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="27cfb-211">Update hello placeholder text with your *DefaultFullSharedAccessSignature* connection string and hub name from hello [Azure Classic Portal].</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }
6. <span data-ttu-id="27cfb-212">Lägg till följande rader i hello din **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="27cfb-212">Add hello following lines in your **Main** method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="27cfb-213">Tryck på hello F5 viktiga toorun hello app.</span><span class="sxs-lookup"><span data-stu-id="27cfb-213">Press hello F5 key toorun hello app.</span></span> <span data-ttu-id="27cfb-214">Du bör få ett meddelande i hello app.</span><span class="sxs-lookup"><span data-stu-id="27cfb-214">You should receive a notification in hello app.</span></span>
   
      ![][21]

#### <a name="optional-send-notifications-by-using-a-mobile-service"></a><span data-ttu-id="27cfb-215">(Valfritt) Skicka meddelanden med hjälp av en mobiltjänst</span><span class="sxs-lookup"><span data-stu-id="27cfb-215">(Optional) Send notifications by using a mobile service</span></span>
1. <span data-ttu-id="27cfb-216">Följ [Komma igång med Mobile Services].</span><span class="sxs-lookup"><span data-stu-id="27cfb-216">Follow [Get started with Mobile Services].</span></span>
2. <span data-ttu-id="27cfb-217">Logga in toohello [klassiska Azure-portalen], och välj mobiltjänst.</span><span class="sxs-lookup"><span data-stu-id="27cfb-217">Sign in toohello [Azure Classic Portal], and select your mobile service.</span></span>
3. <span data-ttu-id="27cfb-218">Välj hello **Scheduler** fliken hello längst upp.</span><span class="sxs-lookup"><span data-stu-id="27cfb-218">Select hello **Scheduler** tab on hello top.</span></span>
   
      ![][22]
4. <span data-ttu-id="27cfb-219">Skapa ett nytt schemalagt jobb, infoga ett namn och välj **På begäran**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-219">Create a new scheduled job, insert a name, and select **On demand**.</span></span>
   
      ![][23]
5. <span data-ttu-id="27cfb-220">Klicka på hello Jobbnamnet när hello jobb skapas.</span><span class="sxs-lookup"><span data-stu-id="27cfb-220">When hello job is created, click hello job name.</span></span> <span data-ttu-id="27cfb-221">Klicka på hello **skriptet** fliken på hello översta raden.</span><span class="sxs-lookup"><span data-stu-id="27cfb-221">Then click hello **Script** tab on hello top bar.</span></span>
6. <span data-ttu-id="27cfb-222">Infoga följande skript i schemaläggarfunktionen hello.</span><span class="sxs-lookup"><span data-stu-id="27cfb-222">Insert hello following script inside your scheduler function.</span></span> <span data-ttu-id="27cfb-223">Se till att tooreplace hello-platshållare med notification hub namn och hello anslutningssträngen för *DefaultFullSharedAccessSignature* som du fick tidigare.</span><span class="sxs-lookup"><span data-stu-id="27cfb-223">Make sure tooreplace hello placeholders with your notification hub name and hello connection string for *DefaultFullSharedAccessSignature* that you obtained earlier.</span></span> <span data-ttu-id="27cfb-224">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="27cfb-224">Click **Save**.</span></span>
   
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
7. <span data-ttu-id="27cfb-225">Klicka på **kör en gång** på hello nedre fältet.</span><span class="sxs-lookup"><span data-stu-id="27cfb-225">Click **Run Once** on hello bottom bar.</span></span> <span data-ttu-id="27cfb-226">Du bör få ett popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="27cfb-226">You should receive a toast notification.</span></span>

## <a name="next-steps"></a><span data-ttu-id="27cfb-227">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="27cfb-227">Next steps</span></span>
<span data-ttu-id="27cfb-228">I det här enkla exemplet skickade du meddelanden tooall dina Android-enheter.</span><span class="sxs-lookup"><span data-stu-id="27cfb-228">In this simple example, you broadcasted notifications tooall your Android devices.</span></span> <span data-ttu-id="27cfb-229">I ordning tootarget specifika användare finns i självstudiekursen toohello [använda Notification Hubs toopush meddelanden toousers].</span><span class="sxs-lookup"><span data-stu-id="27cfb-229">In order tootarget specific users, refer toohello tutorial [Use Notification Hubs toopush notifications toousers].</span></span> <span data-ttu-id="27cfb-230">Om du vill att toosegment användarna efter intressegrupper, kan du läsa [använda Notification Hubs toosend senaste nytt].</span><span class="sxs-lookup"><span data-stu-id="27cfb-230">If you want toosegment your users by interest groups, you can read [Use Notification Hubs toosend breaking news].</span></span> <span data-ttu-id="27cfb-231">Läs mer om hur toouse Meddelandehubbar i [riktlinjer om Notification Hubs] och i hello [Meddelandehubbar hur toofor Android].</span><span class="sxs-lookup"><span data-stu-id="27cfb-231">Learn more about how toouse Notification Hubs in [Notification Hubs Guidance] and in hello [Notification Hubs How-toofor Android].</span></span>

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
