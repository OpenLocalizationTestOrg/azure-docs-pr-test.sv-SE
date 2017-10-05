---
title: "Komma igång med Notification Hubs för Xamarin.Android-appar | Microsoft Docs"
description: "I den här självstudiekursen beskrivs hur du använder Azure Notification Hubs för att skicka push-meddelanden till en Xamarin-Android-app."
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
ms.openlocfilehash: e0ef1b006a2b202c08a71caaff4ef4d763d50d0a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a><span data-ttu-id="1e8f3-103">Komma igång med Notification Hubs med Xamarin för Android</span><span class="sxs-lookup"><span data-stu-id="1e8f3-103">Get started with Notification Hubs with Xamarin for Android</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="1e8f3-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1e8f3-104">Overview</span></span>
<span data-ttu-id="1e8f3-105">I den här självstudiekursen beskrivs hur du använder Azure Notification Hubs för att skicka push-meddelanden till en Xamarin.Android-app.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-105">This tutorial shows you how to use Azure Notification Hubs to send push notifications to a Xamarin.Android application.</span></span>
<span data-ttu-id="1e8f3-106">Du skapar en tom Xamarin.Android-app som tar emot push-meddelanden genom att använda Google Cloud Messaging (GCM).</span><span class="sxs-lookup"><span data-stu-id="1e8f3-106">You'll create a blank Xamarin.Android app that receives push notifications by using Google Cloud Messaging (GCM).</span></span> <span data-ttu-id="1e8f3-107">När du är klar kan du använda meddelandehubben för att sända push-meddelanden till alla enheter som kör appen.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-107">When you're finished, you'll be able to use your notification hub to broadcast push notifications to all the devices running your app.</span></span> <span data-ttu-id="1e8f3-108">Den färdiga koden finns tillgänglig i exemplet [NotificationHubs-app][GitHub].</span><span class="sxs-lookup"><span data-stu-id="1e8f3-108">The finished code is available in the [NotificationHubs app][GitHub] sample.</span></span>

<span data-ttu-id="1e8f3-109">I den här självstudiekursen visas ett enkelt scenario för sändning med Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-109">This tutorial demonstrates the simple broadcast scenario in using Notification Hubs.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="1e8f3-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="1e8f3-110">Before you begin</span></span>
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="1e8f3-111">Den färdiga koden för den här självstudiekursen hittar du på GitHub [här](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).</span><span class="sxs-lookup"><span data-stu-id="1e8f3-111">The completed code for this tutorial can be found on GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e8f3-112">Krav</span><span class="sxs-lookup"><span data-stu-id="1e8f3-112">Prerequisites</span></span>
<span data-ttu-id="1e8f3-113">För den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-113">This tutorial requires the following:</span></span>

* <span data-ttu-id="1e8f3-114">Visual Studio med Xamarin i Windows eller Xamarin Studio i Mac OS X. Fullständiga installationsanvisningar finns i avsnittet [Konfiguration och installation av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e8f3-114">Visual Studio with Xamarin on Windows or Xamarin Studio on Mac OS X. Complete installation instructions are on [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span>
* <span data-ttu-id="1e8f3-115">Aktivt Google-konto</span><span class="sxs-lookup"><span data-stu-id="1e8f3-115">Active Google account</span></span>
* <span data-ttu-id="1e8f3-116">[Azure Messaging-komponent]</span><span class="sxs-lookup"><span data-stu-id="1e8f3-116">[Azure Messaging Component]</span></span>
* <span data-ttu-id="1e8f3-117">[Google Cloud Messaging-klientkomponent]</span><span class="sxs-lookup"><span data-stu-id="1e8f3-117">[Google Cloud Messaging Client Component]</span></span>

<span data-ttu-id="1e8f3-118">Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Notification Hubs för Xamarin.Android-appar.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-118">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Xamarin.Android apps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e8f3-119">Du måste ha ett aktivt Azure-konto för att slutföra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-119">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="1e8f3-120">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-120">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="1e8f3-121">Mer information om den kostnadsfria utvärderingsversionen av Azure finns [Kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="1e8f3-121">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).</span></span>
> 
> 

## <a name="enable-google-cloud-messaging"></a><span data-ttu-id="1e8f3-122">Aktivera Google Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="1e8f3-122">Enable Google Cloud Messaging</span></span>
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a><span data-ttu-id="1e8f3-123">Konfigurera meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="1e8f3-123">Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li><p><span data-ttu-id="1e8f3-124">Klicka på fliken <b>Konfigurera</b> högst upp, ange <b>API-nyckelvärdet</b> som du fick i det förra avsnittet och klicka sedan på <b>Spara</b>.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-124">Click the <b>Configure</b> tab at the top, enter the <b>API Key</b> value you obtained in the previous section, and then click <b>Save</b>.</span></span></p>
</li>
</ol>
<span data-ttu-id="1e8f3-125">&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)</span><span class="sxs-lookup"><span data-stu-id="1e8f3-125">&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)</span></span>

<span data-ttu-id="1e8f3-126">Meddelandehubben har nu konfigurerats för att fungera med GCM och du har anslutningssträngar för att registrera din app för att både ta emot meddelanden och skicka push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-126">Your notification hub is now configured to work with GCM, and you have the connection strings to both register your app to receive notifications and to send push notifications.</span></span>

## <a name="connect-your-app-to-the-notification-hub"></a><span data-ttu-id="1e8f3-127">Anslut appen till meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="1e8f3-127">Connect your app to the notification hub</span></span>
### <a name="create-a-new-project"></a><span data-ttu-id="1e8f3-128">Skapa ett nytt projekt</span><span class="sxs-lookup"><span data-stu-id="1e8f3-128">Create a new project</span></span>
1. <span data-ttu-id="1e8f3-129">I Xamarin Studio klickar du på **Ny lösning**, sedan på **Android-app** och till sist på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-129">In Xamarin Studio, click **New Solution**, click **Android App**, and click **Next**.</span></span>
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. <span data-ttu-id="1e8f3-130">Ange **appens namn** och **ID**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-130">Enter your **App Name** and **Identifier**.</span></span> <span data-ttu-id="1e8f3-131">Klicka på de **målplattformar** du vill stödja och klicka sedan på **Nästa** och **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-131">Click the **Target Plaforms** you want to support and then click **Next** and **Create**.</span></span>
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)

    <span data-ttu-id="1e8f3-132">På så sätt skapas ett nytt Android-projekt.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-132">This creates a new Android project.</span></span>

1. <span data-ttu-id="1e8f3-133">Öppna projektegenskaperna genom att högerklicka på det nya projektet i vyn Lösning och välj **Alternativ**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-133">Open the project properties by right-clicking your new project in the Solution view and choosing **Options**.</span></span> <span data-ttu-id="1e8f3-134">Välj objektet för **Android-appen** i avsnittet **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-134">Select the **Android Application** item in the **Build** section.</span></span>
   
    <span data-ttu-id="1e8f3-135">Kontrollera att den första bokstaven i **paketnamnet** är en gemen.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-135">Ensure that the first letter of your **Package name** is lowercase.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="1e8f3-136">Den första bokstaven i paketnamnet måste vara en gemen.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-136">The first letter of the package name must be lowercase.</span></span> <span data-ttu-id="1e8f3-137">Annars visas appmanifestfel när du registrerar **BroadcastReceiver** och **IntentFilter** för push-meddelanden nedan.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-137">Otherwise, you will receive application manifest errors when you register your **BroadcastReceiver** and **IntentFilter** for push notifications below.</span></span>
   > 
   > 
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)
2. <span data-ttu-id="1e8f3-138">Du kan också ställa in den **äldsta tillåtna Android-versionen** till en annan API-nivå.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-138">Optionally, set the **Minimum Android version** to another API Level.</span></span>
3. <span data-ttu-id="1e8f3-139">Du kan också ställa in **målversionen av Android** till en annan API-version som du vill ha som mål (måste vara API-nivå 8 eller senare).</span><span class="sxs-lookup"><span data-stu-id="1e8f3-139">Optionally, set the **Target Android version** to the another API version that you want to target (must be API level 8 or higher).</span></span>

<span data-ttu-id="1e8f3-140">Klicka på **OK** och stäng dialogrutan Projektalternativ.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-140">Click **OK** and close the Project Options dialog.</span></span>

### <a name="add-the-required-components-to-your-project"></a><span data-ttu-id="1e8f3-141">Lägg till de nödvändiga komponenterna i projektet</span><span class="sxs-lookup"><span data-stu-id="1e8f3-141">Add the required components to your project</span></span>
<span data-ttu-id="1e8f3-142">Google Cloud Messaging-klienten i Xamarin-komponentlagret gör det enklare att ge stöd för push-meddelanden i Xamarin.Android.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-142">The Google Cloud Messaging Client available on the Xamarin Component Store simplifies the process of supporting push notifications in Xamarin.Android.</span></span>

1. <span data-ttu-id="1e8f3-143">Högerklicka på mappen Komponenter i Xamarin.Android-appen och välj **Get more Components** (Få fler komponenter).</span><span class="sxs-lookup"><span data-stu-id="1e8f3-143">Right-click the Components folder in Xamarin.Android app and choose **Get More Components**.</span></span>
2. <span data-ttu-id="1e8f3-144">Sök efter **Azure Messaging**-komponenten och lägg till den i projektet.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-144">Search for the **Azure Messaging** component and add it to the project.</span></span>
3. <span data-ttu-id="1e8f3-145">Sök efter **Google Cloud Messaging-klienten** och lägg till den i projektet.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-145">Search for the **Google Cloud Messaging Client** component and add it to the project.</span></span>

### <a name="set-up-notification-hubs-in-your-project"></a><span data-ttu-id="1e8f3-146">Konfigurera meddelandehubbar i projektet</span><span class="sxs-lookup"><span data-stu-id="1e8f3-146">Set up notification hubs in your project</span></span>
1. <span data-ttu-id="1e8f3-147">Samla in följande information för Android-appen och meddelandehubben:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-147">Gather the following information for your Android app and notification hub:</span></span>
   
   * <span data-ttu-id="1e8f3-148">**Google-projektnummer**: Hämta värdet för det här projektnumret från översikten över appen på Google Developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-148">**GoogleProjectNumber**:  Get this Project Number value from the overview of your app on the Google Developer Portal.</span></span> <span data-ttu-id="1e8f3-149">Du antecknade värdet tidigare när du skapade appen på portalen.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-149">You made a note of this value earlier when you created the app on the portal.</span></span>
   * <span data-ttu-id="1e8f3-150">**Lyssna anslutningssträng**: På instrumentpanelen på den [Klassisk Azure-portal] klickar du på **Visa anslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-150">**Listen connection string**: On the dashboard in the [Azure Classic Portal], click **View connection strings**.</span></span> <span data-ttu-id="1e8f3-151">Kopiera anslutningssträngen *DefaultListenSharedAccessSignature* för det här värdet.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-151">Copy the *DefaultListenSharedAccessSignature* connection string for this value.</span></span>
   * <span data-ttu-id="1e8f3-152">**Hubbnamn**: Det här är namnet på hubben från den [Klassisk Azure-portal].</span><span class="sxs-lookup"><span data-stu-id="1e8f3-152">**Hub name**: This is the name of your hub from the [Azure Classic Portal].</span></span> <span data-ttu-id="1e8f3-153">Till exempel *mynotificationhub2*.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-153">For example, *mynotificationhub2*.</span></span>
     
     <span data-ttu-id="1e8f3-154">Skapa en **Constants.cs**-klass för Xamarin-projektet och definiera följande konstanta värden i klassen.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-154">Create a **Constants.cs** class for your Xamarin project and define the following constant values in the class.</span></span> <span data-ttu-id="1e8f3-155">Ersätt platshållarna med värdena.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-155">Replace the placeholders with your values.</span></span>
     
       <span data-ttu-id="1e8f3-156">offentlig statisk klass Konstanter   {</span><span class="sxs-lookup"><span data-stu-id="1e8f3-156">public static class Constants   {</span></span>
     
           public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
       <span data-ttu-id="1e8f3-157">}</span><span class="sxs-lookup"><span data-stu-id="1e8f3-157">}</span></span>
2. <span data-ttu-id="1e8f3-158">Lägg till följande using-instruktioner till **MainActivity.cs** :</span><span class="sxs-lookup"><span data-stu-id="1e8f3-158">Add the following using statements to **MainActivity.cs**:</span></span>
   
        using Android.Util;
        using Gcm.Client;
3. <span data-ttu-id="1e8f3-159">Lägg till en variabelinstans till den `MainActivity`-klass som ska användas för att visa en varningsruta när appen körs:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-159">Add an instance variable to the `MainActivity` class that will be used to show an alert dialog when the app is running:</span></span>
   
        public static MainActivity instance;
4. <span data-ttu-id="1e8f3-160">Skapa följande metod i klassen **MainActivity**:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-160">Create the following method in the **MainActivity** class:</span></span>
   
        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);
   
            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }
5. <span data-ttu-id="1e8f3-161">I `OnCreate`-metoden för **MainActivity.cs** initierar du `instance`-variabeln och lägger till ett anrop till `RegisterWithGCM`:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-161">In the `OnCreate` method of **MainActivity.cs**, initialize the `instance` variable and add a call to `RegisterWithGCM`:</span></span>
   
        protected override void OnCreate (Bundle bundle)
        {
            instance = this;
   
            base.OnCreate (bundle);
   
            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);
   
            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);
   
            RegisterWithGCM();
        }
6. <span data-ttu-id="1e8f3-162">Skapa en ny klass, **MyBroadcastReceiver**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-162">Create a new class, **MyBroadcastReceiver**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1e8f3-163">Nedan går vi igenom hur du skapar en **BroadcastReceiver**-klass från grunden.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-163">We will walk through creating a **BroadcastReceiver** class from scratch below.</span></span> <span data-ttu-id="1e8f3-164">Ett snabbt alternativ till att skapa **MyBroadcastReceiver.cs** manuellt är att referera till filen **GcmService.cs** som finns i Xamarin.Android-exempelprojektet som ingår i [NotificationHubs-exemplen][GitHub].</span><span class="sxs-lookup"><span data-stu-id="1e8f3-164">However, a quick alternative to manually creating **MyBroadcastReceiver.cs** is to refer to the **GcmService.cs** file found in the sample Xamarin.Android project included with the [NotificationHubs samples][GitHub].</span></span> <span data-ttu-id="1e8f3-165">Att duplicera **GcmService.cs** och ändra klassnamn kan också vara en bra start.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-165">Duplicating **GcmService.cs** and changing class names can be a great place to start as well.</span></span>
   > 
   > 
7. <span data-ttu-id="1e8f3-166">Lägg till följande using-instruktioner till **MyBroadcastReceiver.cs** (hänvisa till komponenten och sammansättningen som du lagt till tidigare):</span><span class="sxs-lookup"><span data-stu-id="1e8f3-166">Add the following using statements to **MyBroadcastReceiver.cs** (referring to the component and assembly that you added earlier):</span></span>
   
        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;
8. <span data-ttu-id="1e8f3-167">I **MyBroadcastReceiver.cs** lägger du till följande behörighetsbegäranden mellan **using**-instruktionen och deklarationen för **namnområdet**:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-167">In **MyBroadcastReceiver.cs**, add the following permission requests between the **using** statements and the **namespace** declaration:</span></span>
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
9. <span data-ttu-id="1e8f3-168">I **MyBroadcastReceiver.cs** ändrar du **MyBroadcastReceiver**-klassen så att den stämmer överens med följande:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-168">In **MyBroadcastReceiver.cs**, change the **MyBroadcastReceiver** class to match the following:</span></span>
   
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
10. <span data-ttu-id="1e8f3-169">Lägg till ytterligare en klass i **MyBroadcastReceiver.cs** med namnet **PushHandlerService**, som kommer från **GcmServiceBase**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-169">Add another class in **MyBroadcastReceiver.cs** named **PushHandlerService**, which derives from **GcmServiceBase**.</span></span> <span data-ttu-id="1e8f3-170">Tillämpa attributet för **tjänsten** på klassen:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-170">Make sure to apply the **Service** attribute to the class:</span></span>
    
         [Service] // Must use the service tag
         public class PushHandlerService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }
             private NotificationHub Hub { get; set; }
    
             public PushHandlerService() : base(Constants.SenderID)
                {
                 Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
             }
         }
11. <span data-ttu-id="1e8f3-171">**GcmServiceBase** implementerar metoderna **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()** och **OnError()**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-171">**GcmServiceBase** implements methods **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()**, and **OnError()**.</span></span> <span data-ttu-id="1e8f3-172">Vår implementeringsklass **PushHandlerService** måste åsidosätta de här metoderna, vilka utlöses som svar på interaktionen med meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-172">Our **PushHandlerService** implementation class must override these methods, and these methods will fire in response to interacting with the notification hub.</span></span>
12. <span data-ttu-id="1e8f3-173">Åsidosätt metoden **OnRegistered()** i **PushHandlerService** med följande kod:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-173">Override the **OnRegistered()** method in **PushHandlerService** by using the following code:</span></span>
    
         protected override void OnRegistered(Context context, string registrationId)
         {
             Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
             RegistrationID = registrationId;
    
             createNotification("PushHandlerService-GCM Registered...",
                                 "The device has been Registered!");
    
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
    > <span data-ttu-id="1e8f3-174">Observera möjligheten att specificera taggar för registrering för vissa meddelandekanaler i koden för **OnRegistered()**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-174">In the **OnRegistered()** code above, you should note the ability to specify tags to register for specific messaging channels.</span></span>
    > 
    > 
13. <span data-ttu-id="1e8f3-175">Åsidosätt metoden **OnMessage** i **PushHandlerService** med följande kod:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-175">Override the **OnMessage** method in **PushHandlerService** by using the following code:</span></span>
    
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
14. <span data-ttu-id="1e8f3-176">Lägg till följande **createNotification**- och **dialogNotify**-metoder till **PushHandlerService** för att meddela användarna när ett meddelande tas emot.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-176">Add the following **createNotification** and **dialogNotify** methods to **PushHandlerService** for notifying users when a notification is received.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="1e8f3-177">Meddelandedesign i Android-versionen 5.0 eller senare skiljer sig avsevärt från tidigare versioner.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-177">Notification design in Android version 5.0 and later represents a significant departure from that of previous versions.</span></span> <span data-ttu-id="1e8f3-178">Om du testar detta i Android 5.0 eller senare måste appen köras för att ta emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-178">If you test this on Android 5.0 or later, the app will need to be running to receive the notification.</span></span> <span data-ttu-id="1e8f3-179">Mer information om Android-meddelanden finns [här](http://go.microsoft.com/fwlink/?LinkId=615880).</span><span class="sxs-lookup"><span data-stu-id="1e8f3-179">For more information, see [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880).</span></span>
    > 
    > 
    
        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;
    
            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));
    
            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);
    
            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;
    
            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));
    
            //Show the notification
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
15. <span data-ttu-id="1e8f3-180">Åsidosätt de abstrakta medlemmarna **OnUnRegistered()**, **OnRecoverableError()** och **OnError()** så att din kod kompileras:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-180">Override abstract members **OnUnRegistered()**, **OnRecoverableError()**, and **OnError()** so that your code compiles:</span></span>
    
        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);
    
            createNotification("GCM Unregistered...", "The device has been unregistered!");
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

## <a name="run-your-app-in-the-emulator"></a><span data-ttu-id="1e8f3-181">Kör appen i emulatorn</span><span class="sxs-lookup"><span data-stu-id="1e8f3-181">Run your app in the emulator</span></span>
<span data-ttu-id="1e8f3-182">Se till att använda en virtuell Android-enhet (AVD) som har stöd för Google-API:er om du kör den här appen i emulatorn.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-182">If you run this app in the emulator, make sure that you use an Android Virtual Device (AVD) that supports Google APIs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e8f3-183">Om du vill kunna ta emot push-meddelanden måste du skapa ett Google-konto på din virtuella Android-enhet.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-183">In order to receive push notifications, you must set up a Google account on your Android Virtual Device.</span></span> <span data-ttu-id="1e8f3-184">(I emulatorn navigerar du till **Inställningar** och klickar på **Lägg till konto**.) Se också till att emulatorn är ansluten till Internet.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-184">(In the emulator, navigate to **Settings** and click **Add Account**.) Also, make sure that the emulator is connected to the Internet.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="1e8f3-185">Meddelandedesign i Android-versionen 5.0 eller senare skiljer sig avsevärt från tidigare versioner.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-185">Notification design in Android version 5.0 and later represents a significant departure from that of previous versions.</span></span> <span data-ttu-id="1e8f3-186">Mer information om Android-meddelanden finns [här](http://go.microsoft.com/fwlink/?LinkId=615880).</span><span class="sxs-lookup"><span data-stu-id="1e8f3-186">For more information, see [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880).</span></span>
> 
> 

1. <span data-ttu-id="1e8f3-187">I **Verktyg** klickar du på **Open Android Emulator Manager** (Öppna hanteraren för Android-emulator), väljer enheten och klickar sedan på **Redigera**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-187">From **Tools**, click **Open Android Emulator Manager**, select your device, and then click **Edit**.</span></span>
   
      ![][18]
2. <span data-ttu-id="1e8f3-188">Välj **Google APIs** (API:er för Google) i **Mål** och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-188">Select **Google APIs** in **Target**, and then click **OK**.</span></span>
   
      ![][19]
3. <span data-ttu-id="1e8f3-189">Klicka på **Kör** i det översta verktygsfältet och välj appen.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-189">On the top toolbar, click **Run**, and then select your app.</span></span> <span data-ttu-id="1e8f3-190">Då startas emulatorn och kör appen.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-190">This starts the emulator and runs the app.</span></span>
   
   <span data-ttu-id="1e8f3-191">Appen hämtar *registrerings-ID:t* från GCM och registreras med meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-191">The app retrieves the *registrationId* from GCM and registers with the notification hub.</span></span>

## <a name="send-notifications-from-your-backend"></a><span data-ttu-id="1e8f3-192">Skicka meddelanden från serverdelen</span><span class="sxs-lookup"><span data-stu-id="1e8f3-192">Send notifications from your backend</span></span>
<span data-ttu-id="1e8f3-193">Du testar att ta emot meddelanden i appen genom att skicka meddelanden i den [Klassisk Azure-portal] via felsökningsfliken i meddelandehubben, så som visas på skärmen nedan.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-193">You can test receiving notifications in your app by sending notifications in the [Azure Classic Portal] via the debug tab on the notification hub, as shown in the screen below.</span></span>

![][30]

<span data-ttu-id="1e8f3-194">Push-meddelanden skickas vanligtvis via en serverdelstjänst som Mobile Services eller ASP.NET med hjälp av ett kompatibelt bibliotek.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-194">Push notifications are normally sent in a backend service like Mobile Services or ASP.NET through a compatible library.</span></span> <span data-ttu-id="1e8f3-195">Du kan också använda REST-API:et direkt för att skicka meddelanden om ett bibliotek inte är tillgängligt för din serverdel.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-195">You can also use the REST API directly to send notification messages if a library is not available for your backend.</span></span>

<span data-ttu-id="1e8f3-196">Här är en lista med andra självstudiekurser du kan gå igenom som handlar om att skicka meddelanden:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-196">Here is a list of some other tutorials that you may want to review for sending notifications:</span></span>

* <span data-ttu-id="1e8f3-197">ASP.NET: Se [Använda Notification Hubs för att skicka push-meddelanden till användare].</span><span class="sxs-lookup"><span data-stu-id="1e8f3-197">ASP.NET: See [Use Notification Hubs to push notifications to users].</span></span>
* <span data-ttu-id="1e8f3-198">Azure Notification Hubs Java SDK: Se [Använda Notification Hubs från Java](notification-hubs-java-push-notification-tutorial.md) för att skicka meddelanden från Java.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-198">Azure Notification Hubs Java SDK: See [How to use Notification Hubs from Java](notification-hubs-java-push-notification-tutorial.md) for sending notifications from Java.</span></span> <span data-ttu-id="1e8f3-199">Det här har testats i Eclipse för Android-utveckling.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-199">This has been tested in Eclipse for Android Development.</span></span>
* <span data-ttu-id="1e8f3-200">PHP: Se [Använda Notification Hubs från PHP](notification-hubs-php-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="1e8f3-200">PHP: See [How to use Notification Hubs from PHP](notification-hubs-php-push-notification-tutorial.md).</span></span>

<span data-ttu-id="1e8f3-201">I nästa underavsnitt i självstudiekursen skickar du meddelanden med hjälp av en app för .NET-konsolen och genom att använda en mobiltjänst via ett nod-skript.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-201">In the next subsections of the tutorial, you send notifications by using a .NET console app, and by using a mobile service through a node script.</span></span>

#### <a name="optional-send-notifications-by-using-a-net-app"></a><span data-ttu-id="1e8f3-202">(Valfritt) Skicka meddelanden med hjälp av en .NET-app</span><span class="sxs-lookup"><span data-stu-id="1e8f3-202">(Optional) Send notifications by using a .NET app</span></span>
<span data-ttu-id="1e8f3-203">I det här avsnittet skickar du meddelanden med hjälp av en .NET-konsolapp</span><span class="sxs-lookup"><span data-stu-id="1e8f3-203">In this section, we will send notifications by using a .NET console app</span></span>

1. <span data-ttu-id="1e8f3-204">Skapa en ny Visual C#-konsolapp:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-204">Create a new Visual C# console application:</span></span>
   
      ![][20]
2. <span data-ttu-id="1e8f3-205">I Visual Studio klickar du på **Verktyg**, **NuGet Package Manager** och sedan på **Package Manager-konsolen**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-205">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="1e8f3-206">Då visas Package Manager-konsolen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-206">This displays the Package Manager Console in Visual Studio.</span></span>
3. <span data-ttu-id="1e8f3-207">I fönstret Package Manager-konsol ställer du in **standardprojektet** till det nya projektet för konsolprogrammet. Sedan kör du följande kommando i konsolfönstret:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-207">In the Package Manager Console window, set the **Default project** to your new console application project, and then in the console window, execute the following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="1e8f3-208">Då läggs en referens till i Azure Notification Hubs SDK med hjälp av <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs-NuGet-paketet</a>.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-208">This adds a reference to the Azure Notification Hubs SDK using the <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="1e8f3-209">Öppna Program.cs-filen och lägg till följande `using`-instruktion:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-209">Open the Program.cs file and add the following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="1e8f3-210">Lägg till följande metod i `Program`-klassen.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-210">In your `Program` class, add the following method.</span></span> <span data-ttu-id="1e8f3-211">Uppdatera platshållartexten med anslutningssträngen *DefaultFullSharedAccessSignature* och hubbens namn från den [Klassisk Azure-portal].</span><span class="sxs-lookup"><span data-stu-id="1e8f3-211">Update the placeholder text with your *DefaultFullSharedAccessSignature* connection string and hub name from the [Azure Classic Portal].</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }
6. <span data-ttu-id="1e8f3-212">Lägg till följande rader i **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="1e8f3-212">Add the following lines in your **Main** method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="1e8f3-213">Kör appen genom att trycka på F5.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-213">Press the F5 key to run the app.</span></span> <span data-ttu-id="1e8f3-214">Du bör få ett meddelande i appen.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-214">You should receive a notification in the app.</span></span>
   
      ![][21]

#### <a name="optional-send-notifications-by-using-a-mobile-service"></a><span data-ttu-id="1e8f3-215">(Valfritt) Skicka meddelanden med hjälp av en mobiltjänst</span><span class="sxs-lookup"><span data-stu-id="1e8f3-215">(Optional) Send notifications by using a mobile service</span></span>
1. <span data-ttu-id="1e8f3-216">Följ [Komma igång med Mobile Services].</span><span class="sxs-lookup"><span data-stu-id="1e8f3-216">Follow [Get started with Mobile Services].</span></span>
2. <span data-ttu-id="1e8f3-217">Logga in på den [Klassisk Azure-portal] och välj mobiltjänst.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-217">Sign in to the [Azure Classic Portal], and select your mobile service.</span></span>
3. <span data-ttu-id="1e8f3-218">Välj fliken **Schemaläggaren** högst upp.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-218">Select the **Scheduler** tab on the top.</span></span>
   
      ![][22]
4. <span data-ttu-id="1e8f3-219">Skapa ett nytt schemalagt jobb, infoga ett namn och välj **På begäran**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-219">Create a new scheduled job, insert a name, and select **On demand**.</span></span>
   
      ![][23]
5. <span data-ttu-id="1e8f3-220">Klicka på jobbnamnet när jobbet skapats.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-220">When the job is created, click the job name.</span></span> <span data-ttu-id="1e8f3-221">Klicka på fliken **Skript** i det översta fältet.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-221">Then click the **Script** tab on the top bar.</span></span>
6. <span data-ttu-id="1e8f3-222">Infoga följande skript i schemaläggarfunktionen.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-222">Insert the following script inside your scheduler function.</span></span> <span data-ttu-id="1e8f3-223">Ersätt platshållarna med namnet på din meddelandehubb och anslutningssträngen för *DefaultFullSharedAccessSignature* som du fick tidigare.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-223">Make sure to replace the placeholders with your notification hub name and the connection string for *DefaultFullSharedAccessSignature* that you obtained earlier.</span></span> <span data-ttu-id="1e8f3-224">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-224">Click **Save**.</span></span>
   
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
7. <span data-ttu-id="1e8f3-225">Klicka på **Kör en gång** i det nedre fältet.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-225">Click **Run Once** on the bottom bar.</span></span> <span data-ttu-id="1e8f3-226">Du bör få ett popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-226">You should receive a toast notification.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e8f3-227">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e8f3-227">Next steps</span></span>
<span data-ttu-id="1e8f3-228">I det här enkla exemplet skickade du meddelanden till alla dina Android-enheter.</span><span class="sxs-lookup"><span data-stu-id="1e8f3-228">In this simple example, you broadcasted notifications to all your Android devices.</span></span> <span data-ttu-id="1e8f3-229">Mer information om hur du riktar in dig på specifika användare finns i självstudiekursen [Använda Notification Hubs för att skicka push-meddelanden till användare].</span><span class="sxs-lookup"><span data-stu-id="1e8f3-229">In order to target specific users, refer to the tutorial [Use Notification Hubs to push notifications to users].</span></span> <span data-ttu-id="1e8f3-230">Om du vill dela in användarna efter intressegrupper läser du [Använda Notification Hubs för att skicka de senaste nyheterna].</span><span class="sxs-lookup"><span data-stu-id="1e8f3-230">If you want to segment your users by interest groups, you can read [Use Notification Hubs to send breaking news].</span></span> <span data-ttu-id="1e8f3-231">Mer information om hur du använder Notification Hubs finns i [Riktlinjer för Notification Hubs] och [Notification Hubs-instruktioner för Android].</span><span class="sxs-lookup"><span data-stu-id="1e8f3-231">Learn more about how to use Notification Hubs in [Notification Hubs Guidance] and in the [Notification Hubs How-To for Android].</span></span>

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
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
<span data-ttu-id="1e8f3-232">[Komma igång med Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service</span><span class="sxs-lookup"><span data-stu-id="1e8f3-232">[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service</span></span>
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

<span data-ttu-id="1e8f3-233">[Klassisk Azure-portal]: https://manage.windowsazure.com/</span><span class="sxs-lookup"><span data-stu-id="1e8f3-233">[Azure Classic Portal]: https://manage.windowsazure.com/</span></span>
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
<span data-ttu-id="1e8f3-234">[Riktlinjer för Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx</span><span class="sxs-lookup"><span data-stu-id="1e8f3-234">[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx</span></span>
<span data-ttu-id="1e8f3-235">[Notification Hubs-instruktioner för Android]: http://msdn.microsoft.com/library/dn282661.aspx</span><span class="sxs-lookup"><span data-stu-id="1e8f3-235">[Notification Hubs How-To for Android]: http://msdn.microsoft.com/library/dn282661.aspx</span></span>

<span data-ttu-id="1e8f3-236">[Använda Notification Hubs för att skicka push-meddelanden till användare]: /manage/services/notification-hubs/notify-users-aspnet</span><span class="sxs-lookup"><span data-stu-id="1e8f3-236">[Use Notification Hubs to push notifications to users]: /manage/services/notification-hubs/notify-users-aspnet</span></span>
<span data-ttu-id="1e8f3-237">[Använda Notification Hubs för att skicka de senaste nyheterna]: /manage/services/notification-hubs/breaking-news-dotnet</span><span class="sxs-lookup"><span data-stu-id="1e8f3-237">[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet</span></span>
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
<span data-ttu-id="1e8f3-238">[Google Cloud Messaging-klientkomponent]: http://components.xamarin.com/view/GCMClient/</span><span class="sxs-lookup"><span data-stu-id="1e8f3-238">[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/</span></span>
<span data-ttu-id="1e8f3-239">[Azure Messaging-komponent]: http://components.xamarin.com/view/azure-messaging</span><span class="sxs-lookup"><span data-stu-id="1e8f3-239">[Azure Messaging Component]: http://components.xamarin.com/view/azure-messaging</span></span>
