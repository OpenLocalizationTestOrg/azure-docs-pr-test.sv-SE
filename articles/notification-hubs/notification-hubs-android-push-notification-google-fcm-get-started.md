---
title: aaaSending push-meddelanden tooAndroid med Azure Notification Hubs och Firebase Cloud Messaging | Microsoft Docs
description: "I kursen får du lära dig hur toouse Azure Notification Hub och Firebase Cloud Messaging toopush meddelanden tooAndroid enheter."
services: notification-hubs
documentationcenter: android
keywords: "push-meddelanden, push-meddelande, push-meddelande för android, fcm, firebase cloud messaging"
author: ysxu
manager: erikre
editor: 
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 07/14/2016
ms.author: yuaxu
ms.openlocfilehash: d2e57437ac7b0ef77abf048f991043620621e58d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-tooandroid-with-azure-notification-hubs"></a><span data-ttu-id="db65a-104">Skicka push-meddelanden tooAndroid med Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="db65a-104">Sending push notifications tooAndroid with Azure Notification Hubs</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="db65a-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="db65a-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="db65a-106">Det här ämnet visar push-meddelanden med Google Firebase Cloud Messaging (FCM).</span><span class="sxs-lookup"><span data-stu-id="db65a-106">This topic demonstrates push notifications with Google Firebase Cloud Messaging (FCM).</span></span> <span data-ttu-id="db65a-107">Om du fortfarande använder Google Cloud Messaging (GCM), se [skicka push-meddelanden tooAndroid med Azure Notification Hubs och GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="db65a-107">If you are still using Google Cloud Messaging (GCM), see [Sending push notifications tooAndroid with Azure Notification Hubs and GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).</span></span>
> 
> 

<span data-ttu-id="db65a-108">Den här kursen visar hur toouse Azure Notification Hubs och Firebase Cloud Messaging toosend push-meddelanden tooan Android-program.</span><span class="sxs-lookup"><span data-stu-id="db65a-108">This tutorial shows you how toouse Azure Notification Hubs and Firebase Cloud Messaging toosend push notifications tooan Android application.</span></span>
<span data-ttu-id="db65a-109">Du skapar en tom Android-app som tar emot push-meddelanden via Firebase Cloud Messaging (FCM).</span><span class="sxs-lookup"><span data-stu-id="db65a-109">You'll create a blank Android app that receives push notifications by using Firebase Cloud Messaging (FCM).</span></span>

[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="db65a-110">hello slutförts koden för den här självstudiekursen kan hämtas från GitHub [här](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).</span><span class="sxs-lookup"><span data-stu-id="db65a-110">hello completed code for this tutorial can be downloaded from GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db65a-111">Krav</span><span class="sxs-lookup"><span data-stu-id="db65a-111">Prerequisites</span></span>
> [!IMPORTANT]
> <span data-ttu-id="db65a-112">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="db65a-112">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="db65a-113">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="db65a-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="db65a-114">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="db65a-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).</span></span>
> 
> 

* <span data-ttu-id="db65a-115">Dessutom tooan aktivt Azure-konto som anges ovan, kursen krävs hello senaste versionen av [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).</span><span class="sxs-lookup"><span data-stu-id="db65a-115">In addition tooan active Azure account mentioned above, this tutorial requires hello latest version of [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).</span></span>
* <span data-ttu-id="db65a-116">Android 2.3 eller högre för Firebase Cloud Messaging.</span><span class="sxs-lookup"><span data-stu-id="db65a-116">Android 2.3 or higher for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="db65a-117">Google Repository revision 27 eller högre krävs för Firebase Cloud Messaging.</span><span class="sxs-lookup"><span data-stu-id="db65a-117">Google Repository revision 27 or higher is required for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="db65a-118">Google Play Services 9.0.2 eller högre för Firebase Cloud Messaging.</span><span class="sxs-lookup"><span data-stu-id="db65a-118">Google Play Services 9.0.2 or higher for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="db65a-119">Du måste slutföra den här kursen innan du börjar någon annan kurs om Notification Hubs för Android-appar.</span><span class="sxs-lookup"><span data-stu-id="db65a-119">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Android apps.</span></span>

## <a name="create-a-new-android-studio-project"></a><span data-ttu-id="db65a-120">Skapa ett nytt Android Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="db65a-120">Create a new Android Studio Project</span></span>
1. <span data-ttu-id="db65a-121">Starta ett nytt Android Studio-projekt i Android Studio.</span><span class="sxs-lookup"><span data-stu-id="db65a-121">In Android Studio, start a new Android Studio project.</span></span>
   
       ![Android Studio - new project](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)
2. <span data-ttu-id="db65a-122">Välj hello **Telefoner och surfplattor** formuläret faktor och hello **Minimum SDK** som du vill toosupport.</span><span class="sxs-lookup"><span data-stu-id="db65a-122">Choose hello **Phone and Tablet** form factor and hello **Minimum SDK** that you want toosupport.</span></span> <span data-ttu-id="db65a-123">Klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="db65a-123">Then click **Next**.</span></span>
   
       ![Android Studio - project creation workflow](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)
3. <span data-ttu-id="db65a-124">Välj **tom aktivitet** hello huvudsaklig aktivitet, klickar du på **nästa**, och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="db65a-124">Choose **Empty Activity** for hello main activity, click **Next**, and then click **Finish**.</span></span>

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a><span data-ttu-id="db65a-125">Skapa ett projekt som har stöd för Firebase Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="db65a-125">Create a project that supports Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-new-notification-hub"></a><span data-ttu-id="db65a-126">Konfigurera en ny meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="db65a-126">Configure a new notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<span data-ttu-id="db65a-127">&emsp;&emsp;6.</span><span class="sxs-lookup"><span data-stu-id="db65a-127">&emsp;&emsp;6.</span></span> <span data-ttu-id="db65a-128">I hello **inställningar** bladet för din meddelandehubb, Välj **Notification Services** och sedan **Google (GCM)**.</span><span class="sxs-lookup"><span data-stu-id="db65a-128">In hello **Settings** blade of your notification hub, select **Notification Services** and then **Google (GCM)**.</span></span> <span data-ttu-id="db65a-129">Ange hello FCM servernyckeln som du kopierade tidigare från hello [Firebase konsolen](https://firebase.google.com/console/) och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="db65a-129">Enter hello FCM server key you copied earlier from hello [Firebase console](https://firebase.google.com/console/) and click **Save**.</span></span>

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

<span data-ttu-id="db65a-131">Din meddelandehubb är nu konfigurerad toowork med Firebase moln Messagin och du har hello anslutning strängar tooboth registrera din app tooreceive och skicka push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="db65a-131">Your notification hub is now configured toowork with Firebase Cloud Messagin, and you have hello connection strings tooboth register your app tooreceive and send push notifications.</span></span>

## <span data-ttu-id="db65a-132"><a id="connecting-app"></a>Ansluta din app toohello notification hub</span><span class="sxs-lookup"><span data-stu-id="db65a-132"><a id="connecting-app"></a>Connect your app toohello notification hub</span></span>
### <a name="add-google-play-services-toohello-project"></a><span data-ttu-id="db65a-133">Lägga till Google Play services toohello projekt</span><span class="sxs-lookup"><span data-stu-id="db65a-133">Add Google Play services toohello project</span></span>
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a><span data-ttu-id="db65a-134">Lägga till bibliotek för Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="db65a-134">Adding Azure Notification Hubs libraries</span></span>
1. <span data-ttu-id="db65a-135">I hello `Build.Gradle` -filen för hello **app**, Lägg till följande rader i hello hello **beroenden** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="db65a-135">In hello `Build.Gradle` file for hello **app**, add hello following lines in hello **dependencies** section.</span></span>
   
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
2. <span data-ttu-id="db65a-136">Lägg till följande lagringsplats efter hello hello **beroenden** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="db65a-136">Add hello following repository after hello **dependencies** section.</span></span>
   
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-hello-androidmanifestxml"></a><span data-ttu-id="db65a-137">Uppdaterar hello AndroidManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="db65a-137">Updating hello AndroidManifest.xml.</span></span>
1. <span data-ttu-id="db65a-138">toosupport FCM måste vi implementera en lyssnartjänst för instans-ID i vår kod som används för[hämta registreringstoken](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) med [Googles FirebaseInstanceId API](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId).</span><span class="sxs-lookup"><span data-stu-id="db65a-138">toosupport FCM, we must implement a Instance ID listener service in our code which is used too[obtain registration tokens](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) using [Google's FirebaseInstanceId API](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId).</span></span> <span data-ttu-id="db65a-139">I den här självstudiekursen kommer vi att kalla klassen hello `MyInstanceIDService`.</span><span class="sxs-lookup"><span data-stu-id="db65a-139">In this tutorial we will name hello class `MyInstanceIDService`.</span></span> 
   
    <span data-ttu-id="db65a-140">Lägg till följande toohello AndroidManifest.xml tjänstdefinitionsfilen inuti hello hello `<application>` tagg.</span><span class="sxs-lookup"><span data-stu-id="db65a-140">Add hello following service definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> 
   
        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>
2. <span data-ttu-id="db65a-141">När vi har tagit emot vår FCM registrering token från hello FirebaseInstanceId API, kan den användas för[registrera med hello Azure Notification Hub](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="db65a-141">Once we have received our FCM registration token from hello FirebaseInstanceId API, we will use it too[register with hello Azure Notification Hub](notification-hubs-push-notification-registration-management.md).</span></span> <span data-ttu-id="db65a-142">Vi stöder denna registrering i hello bakgrunden med en `IntentService` med namnet `RegistrationIntentService`.</span><span class="sxs-lookup"><span data-stu-id="db65a-142">We will support this registration in hello background using an `IntentService` named `RegistrationIntentService`.</span></span> <span data-ttu-id="db65a-143">Den här tjänsten kommer också att ansvara för att uppdatera vårt FCM-registreringstoken.</span><span class="sxs-lookup"><span data-stu-id="db65a-143">This service will also be responsible for refreshing our FCM registration token.</span></span>
   
    <span data-ttu-id="db65a-144">Lägg till följande toohello AndroidManifest.xml tjänstdefinitionsfilen inuti hello hello `<application>` tagg.</span><span class="sxs-lookup"><span data-stu-id="db65a-144">Add hello following service definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> 
   
        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>
3. <span data-ttu-id="db65a-145">Vi kommer också att definiera en mottagare tooreceive meddelanden.</span><span class="sxs-lookup"><span data-stu-id="db65a-145">We will also define a receiver tooreceive notifications.</span></span> <span data-ttu-id="db65a-146">Lägg till följande mottagare toohello AndroidManifest.xml definitionsfilen inuti hello hello `<application>` tagg.</span><span class="sxs-lookup"><span data-stu-id="db65a-146">Add hello following receiver definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> <span data-ttu-id="db65a-147">Ersätt hello `<your package>` med hello faktiska paketnamnet som visas överst hello i hello `AndroidManifest.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="db65a-147">Replace hello `<your package>` placeholder with hello your actual package name shown at hello top of hello `AndroidManifest.xml` file.</span></span>
   
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
4. <span data-ttu-id="db65a-148">Lägg till följande nödvändiga FCM hello-relaterade behörigheter under hello `</application>` tagg.</span><span class="sxs-lookup"><span data-stu-id="db65a-148">Add hello following necessary FCM related permissions below hello  `</application>` tag.</span></span> <span data-ttu-id="db65a-149">Se till att tooreplace `<your package>` med hello paketnamnet som visas överst hello i hello `AndroidManifest.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="db65a-149">Make sure tooreplace `<your package>` with hello package name shown at hello top of hello `AndroidManifest.xml` file.</span></span>
   
    <span data-ttu-id="db65a-150">Mer information om dessa behörigheter finns [konfigurera en GCM-klientapp för Android](https://developers.google.com/cloud-messaging/android/client#manifest) och [migrera en GCM-Klientapp för Android tooFirebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).</span><span class="sxs-lookup"><span data-stu-id="db65a-150">For more information on these permissions, see [Setup a GCM Client app for Android](https://developers.google.com/cloud-messaging/android/client#manifest) and [Migrate a GCM Client App for Android tooFirebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).</span></span>
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

### <a name="adding-code"></a><span data-ttu-id="db65a-151">Lägga till kod</span><span class="sxs-lookup"><span data-stu-id="db65a-151">Adding code</span></span>
1. <span data-ttu-id="db65a-152">Expandera i hello projektvyn **app** > **src** > **huvudsakliga** > **java**.</span><span class="sxs-lookup"><span data-stu-id="db65a-152">In hello Project View, expand **app** > **src** > **main** > **java**.</span></span> <span data-ttu-id="db65a-153">Högerklicka på paketmappen under **java**, klicka på **Ny** och sedan klickar du på **Java-klass**.</span><span class="sxs-lookup"><span data-stu-id="db65a-153">Right-click your package folder under **java**, click **New**, and then click **Java Class**.</span></span> <span data-ttu-id="db65a-154">Lägg till en ny klass med namnet `NotificationSettings`.</span><span class="sxs-lookup"><span data-stu-id="db65a-154">Add a new class named `NotificationSettings`.</span></span> 
   
    ![Android Studio – ny Java-klass](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)
   
    <span data-ttu-id="db65a-156">Kontrollera att tooupdate hello dessa tre platshållare i följande kod för hello hello `NotificationSettings` klass:</span><span class="sxs-lookup"><span data-stu-id="db65a-156">Make sure tooupdate hello these three placeholders in hello following code for hello `NotificationSettings` class:</span></span>
   
   * <span data-ttu-id="db65a-157">**SenderId**: hello avsändar-Id som du hämtade tidigare i hello **Cloud Messaging** fliken av dina Projektinställningar i hello [Firebase konsolen](https://firebase.google.com/console/).</span><span class="sxs-lookup"><span data-stu-id="db65a-157">**SenderId**: hello Sender Id you obtained earlier in hello **Cloud Messaging** tab of your project settings in hello [Firebase console](https://firebase.google.com/console/).</span></span>
   * <span data-ttu-id="db65a-158">**HubListenConnectionString**: hello **DefaultListenAccessSignature** anslutningssträngen för din hubb.</span><span class="sxs-lookup"><span data-stu-id="db65a-158">**HubListenConnectionString**: hello **DefaultListenAccessSignature** connection string for your hub.</span></span> <span data-ttu-id="db65a-159">Du kan kopiera denna anslutningssträng genom att klicka på **åtkomstprinciper** på hello **inställningar** bladet för din hubb på hello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="db65a-159">You can copy that connection string by clicking **Access Policies** on hello **Settings** blade of your hub on hello [Azure Portal].</span></span>
   * <span data-ttu-id="db65a-160">**HubName**: Använd hello namnet på din meddelandehubb som visas i hello hubbladet på hello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="db65a-160">**HubName**: Use hello name of your notification hub that appears in hello hub blade in hello [Azure Portal].</span></span>
     
     <span data-ttu-id="db65a-161">`NotificationSettings` kod:</span><span class="sxs-lookup"><span data-stu-id="db65a-161">`NotificationSettings` code:</span></span>
     
       <span data-ttu-id="db65a-162">offentlig klass NotificationSettings {</span><span class="sxs-lookup"><span data-stu-id="db65a-162">public class NotificationSettings {</span></span>
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
       <span data-ttu-id="db65a-163">}</span><span class="sxs-lookup"><span data-stu-id="db65a-163">}</span></span>
2. <span data-ttu-id="db65a-164">Med hello stegen ovan kan lägga till ytterligare en ny klass med namnet `MyInstanceIDService`.</span><span class="sxs-lookup"><span data-stu-id="db65a-164">Using hello steps above, add another new class named `MyInstanceIDService`.</span></span> <span data-ttu-id="db65a-165">Det här kommer att bli vår implementering av lyssnartjänsten för instans-ID.</span><span class="sxs-lookup"><span data-stu-id="db65a-165">This will be our Instance ID listener service implementation.</span></span>
   
    <span data-ttu-id="db65a-166">hello-koden för den här klassen anropar våra `IntentService` för[uppdateringstoken hello FCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="db65a-166">hello code for this class will call our `IntentService` too[refresh hello FCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) in hello background.</span></span>
   
        import android.content.Intent;
        import android.util.Log;
        import com.google.firebase.iid.FirebaseInstanceIdService;

        public class MyInstanceIDService extends FirebaseInstanceIdService {

            private static final String TAG = "MyInstanceIDService";

            @Override
            public void onTokenRefresh() {

                Log.d(TAG, "Refreshing GCM Registration Token");

                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


1. <span data-ttu-id="db65a-167">Lägg till en annan ny klass tooyour-projektet med namnet `RegistrationIntentService`.</span><span class="sxs-lookup"><span data-stu-id="db65a-167">Add another new class tooyour project named, `RegistrationIntentService`.</span></span> <span data-ttu-id="db65a-168">Detta blir hello implementeringen för vår `IntentService` som hanterar [uppdaterar hello FCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) och [registreras med meddelandehubben hello](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="db65a-168">This will be hello implementation for our `IntentService` that will handle [refreshing hello FCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) and [registering with hello notification hub](notification-hubs-push-notification-registration-management.md).</span></span>
   
    <span data-ttu-id="db65a-169">Använd hello följande kod för den här klassen.</span><span class="sxs-lookup"><span data-stu-id="db65a-169">Use hello following code for this class.</span></span>
   
        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;        
        import com.google.firebase.iid.FirebaseInstanceId;
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
                String storedToken = null;
   
                try {
                    String FCM_token = FirebaseInstanceId.getInstance().getToken();
                    Log.d(TAG, "FCM Registration Token: " + FCM_token);
   
                    // Storing hello registration id that indicates whether hello generated token has been
                    // sent tooyour server. If it is not stored, send hello token tooyour server,
                    // otherwise your server should have already received hello token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want toouse tags...
                        // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    // Check if hello token may have been compromised and needs refreshing.
                    else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want toouse tags...
                        // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed toocomplete registration", e);
                    // If an exception happens while fetching hello new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt hello update at a later time.
                }
   
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }
2. <span data-ttu-id="db65a-170">I din `MainActivity` klassen och Lägg till följande hello `import` uttryck ovanför hello klassen deklaration.</span><span class="sxs-lookup"><span data-stu-id="db65a-170">In your `MainActivity` class, add hello following `import` statements above hello class declaration.</span></span>
   
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
3. <span data-ttu-id="db65a-171">Lägg till följande privata medlemmar hello överst i hello klassen hello.</span><span class="sxs-lookup"><span data-stu-id="db65a-171">Add hello following private members at hello top of hello class.</span></span> <span data-ttu-id="db65a-172">Vi använder dessa [Kontrollera hello tillgängligheten för Google Play-tjänster som rekommenderas av Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).</span><span class="sxs-lookup"><span data-stu-id="db65a-172">We will use these [check hello availability of Google Play Services as recommended by Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).</span></span>
   
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
4. <span data-ttu-id="db65a-173">I din `MainActivity` klassen, lägga till hello följa metoden toohello tillgängligheten för Google Play-tjänster.</span><span class="sxs-lookup"><span data-stu-id="db65a-173">In your `MainActivity` class, add hello following method toohello availability of Google Play Services.</span></span> 
   
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
5. <span data-ttu-id="db65a-174">I din `MainActivity` klassen, Lägg till följande kod som kommer att söka efter Google Play Services innan du anropar hello din `IntentService` tooget din FCM registrering token och registrera med meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="db65a-174">In your `MainActivity` class, add hello following code that will check for Google Play Services before calling your `IntentService` tooget your FCM registration token and register with your notification hub.</span></span>
   
        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService tooregister this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
6. <span data-ttu-id="db65a-175">I hello `OnCreate` metod för hello `MainActivity` klassen, Lägg till följande kod toostart hello registreringsprocessen när aktivitet skapas hello.</span><span class="sxs-lookup"><span data-stu-id="db65a-175">In hello `OnCreate` method of hello `MainActivity` class, add hello following code toostart hello registration process when activity is created.</span></span>
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
7. <span data-ttu-id="db65a-176">Lägg till dessa ytterligare metoder toohello `MainActivity` tooverify appens status och rapportera status i din app.</span><span class="sxs-lookup"><span data-stu-id="db65a-176">Add these additional methods toohello `MainActivity` tooverify app state and report status in your app.</span></span>
   
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
8. <span data-ttu-id="db65a-177">Hej `ToastNotify` metoden använder hello *”Hello World”* `TextView` styra tooreport status och meddelanden beständigt i hello app.</span><span class="sxs-lookup"><span data-stu-id="db65a-177">hello `ToastNotify` method uses hello *"Hello World"* `TextView` control tooreport status and notifications persistently in hello app.</span></span> <span data-ttu-id="db65a-178">Lägg till hello följande id för den kontrollen i activity_main.XML.</span><span class="sxs-lookup"><span data-stu-id="db65a-178">In your activity_main.xml layout, add hello following id for that control.</span></span>
   
       android:id="@+id/text_hello"
9. <span data-ttu-id="db65a-179">Nu kommer vi lägga till en underklass för den mottagare som vi har definierat i hello AndroidManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="db65a-179">Next we will add a subclass for our receiver we defined in hello AndroidManifest.xml.</span></span> <span data-ttu-id="db65a-180">Lägg till en annan ny klass tooyour-projektet med namnet `MyHandler`.</span><span class="sxs-lookup"><span data-stu-id="db65a-180">Add another new class tooyour project named `MyHandler`.</span></span>
10. <span data-ttu-id="db65a-181">Lägg till följande importuttryck överst hello i hello `MyHandler.java`:</span><span class="sxs-lookup"><span data-stu-id="db65a-181">Add hello following import statements at hello top of `MyHandler.java`:</span></span>
    
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
11. <span data-ttu-id="db65a-182">Lägg till följande kod för hello hello `MyHandler` klass gör den till en underklass till `com.microsoft.windowsazure.notifications.NotificationsHandler`.</span><span class="sxs-lookup"><span data-stu-id="db65a-182">Add hello following code for hello `MyHandler` class making it a subclass of `com.microsoft.windowsazure.notifications.NotificationsHandler`.</span></span>
    
    <span data-ttu-id="db65a-183">Den här koden åsidosätter hello `OnReceive` metoden så hello hanteraren rapporterar meddelanden som tas emot.</span><span class="sxs-lookup"><span data-stu-id="db65a-183">This code overrides hello `OnReceive` method, so hello handler will report notifications that are received.</span></span> <span data-ttu-id="db65a-184">hello hanteraren skickar även hello push notification toohello Android notification manager genom att använda hello `sendNotification()` metod.</span><span class="sxs-lookup"><span data-stu-id="db65a-184">hello handler also sends hello push notification toohello Android notification manager by using hello `sendNotification()` method.</span></span> <span data-ttu-id="db65a-185">Hej `sendNotification()` metod som ska köras när hello programmet körs inte och ett meddelande tas emot.</span><span class="sxs-lookup"><span data-stu-id="db65a-185">hello `sendNotification()` method should be executed when hello app is not running and a notification is received.</span></span>
    
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
12. <span data-ttu-id="db65a-186">I Android Studio hello menyraden klickar du på **skapa** > **återskapa projekt** toomake till att inga fel visas i din kod.</span><span class="sxs-lookup"><span data-stu-id="db65a-186">In Android Studio on hello menu bar, click **Build** > **Rebuild Project** toomake sure that no errors are present in your code.</span></span>
13. <span data-ttu-id="db65a-187">Kör hello app på enheten och kontrollera att den har registreras i meddelandehubben hello.</span><span class="sxs-lookup"><span data-stu-id="db65a-187">Run hello app on your device and verify it registers successfully with hello notification hub.</span></span> 
    
    > [!NOTE]
    > <span data-ttu-id="db65a-188">Registreringen misslyckas hello inledande startas förrän hello `onTokenRefresh()` instans-ID-tjänst-metoden anropas.</span><span class="sxs-lookup"><span data-stu-id="db65a-188">Registration may fail on hello initial launch until hello `onTokenRefresh()` method of instance Id service is called.</span></span> <span data-ttu-id="db65a-189">hello uppdatering ska intiate en lyckad registrering med hello notification hub.</span><span class="sxs-lookup"><span data-stu-id="db65a-189">hello refresh should intiate a successful registration with hello notification hub.</span></span>
    > 
    > 

## <a name="sending-push-notifications"></a><span data-ttu-id="db65a-190">Skicka push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="db65a-190">Sending push notifications</span></span>
<span data-ttu-id="db65a-191">Du kan testa att ta emot push-meddelanden i din app genom att skicka dem via hello [Azure Portal] -leta efter hello **felsökning** under hello hubbladet, som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="db65a-191">You can test receiving push notifications in your app by sending them via hello [Azure Portal] - look for hello **Troubleshooting** Section in hello hub blade, as shown below.</span></span>

![Azure Notification Hubs – Prova att skicka](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-hello-app"></a><span data-ttu-id="db65a-193">(Valfritt) Skicka push-meddelanden direkt från hello app</span><span class="sxs-lookup"><span data-stu-id="db65a-193">(Optional) Send push notifications directly from hello app</span></span>
> [!IMPORTANT]
> <span data-ttu-id="db65a-194">Det här exemplet för att skicka meddelanden från hello-klientappen tillhandahålls endast för.</span><span class="sxs-lookup"><span data-stu-id="db65a-194">This example of sending notifications from hello client app is provided for learning purposes only.</span></span> <span data-ttu-id="db65a-195">Eftersom detta kräver hello `DefaultFullSharedAccessSignature` toobe finns på hello-klientappen, visar notification hub toohello risken att en användare kan få åtkomst till toosend obehörig meddelanden tooyour klienter.</span><span class="sxs-lookup"><span data-stu-id="db65a-195">Since this will require hello `DefaultFullSharedAccessSignature` toobe present on hello client app, it exposes your notification hub toohello risk that a user may gain access toosend unauthorized notifications tooyour clients.</span></span>
> 
> 

<span data-ttu-id="db65a-196">Normalt sett skickar du meddelanden med hjälp av en backend-server.</span><span class="sxs-lookup"><span data-stu-id="db65a-196">Normally, you would send notifications using a backend server.</span></span> <span data-ttu-id="db65a-197">I vissa fall kanske du vill toobe kan toosend push-meddelanden direkt från hello-klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="db65a-197">For some cases, you might want toobe able toosend push notifications directly from hello client application.</span></span> <span data-ttu-id="db65a-198">Det här avsnittet beskrivs hur toosend meddelanden från hello-klient som använder hello [Azure Notification Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="db65a-198">This section explains how toosend notifications from hello client using hello [Azure Notification Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).</span></span>

1. <span data-ttu-id="db65a-199">Expandera **App** > **src** > **main** > **res** > **layout** i projektvyn för Android Studio.</span><span class="sxs-lookup"><span data-stu-id="db65a-199">In Android Studio Project View, expand **App** > **src** > **main** > **res** > **layout**.</span></span> <span data-ttu-id="db65a-200">Öppna hello `activity_main.xml` layout och klicka på hello **Text** fliken tooupdate hello textinnehållet i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="db65a-200">Open hello `activity_main.xml` layout file and click hello **Text** tab tooupdate hello text contents of hello file.</span></span> <span data-ttu-id="db65a-201">Uppdatera den med hello koden nedan, som lägger till nya `Button` och `EditText` kontroller för att skicka push-meddelande meddelanden toohello meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="db65a-201">Update it with hello code below, which adds new `Button` and `EditText` controls for sending push notification messages toohello notification hub.</span></span> <span data-ttu-id="db65a-202">Lägg till den här koden längst ned hello, precis före `</RelativeLayout>`.</span><span class="sxs-lookup"><span data-stu-id="db65a-202">Add this code at hello bottom, just before `</RelativeLayout>`.</span></span>
   
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
2. <span data-ttu-id="db65a-203">Expandera **App** > **src** > **main** > **res** > **värden** i projektvyn för Android Studio.</span><span class="sxs-lookup"><span data-stu-id="db65a-203">In Android Studio Project View, expand **App** > **src** > **main** > **res** > **values**.</span></span> <span data-ttu-id="db65a-204">Öppna hello `strings.xml` och Lägg till hello strängvärden som refereras av hello nya `Button` och `EditText` kontroller.</span><span class="sxs-lookup"><span data-stu-id="db65a-204">Open hello `strings.xml` file and add hello string values that are referenced by hello new `Button` and `EditText` controls.</span></span> <span data-ttu-id="db65a-205">Lägg till dessa längst ned hello hello-fil, precis före `</resources>`.</span><span class="sxs-lookup"><span data-stu-id="db65a-205">Add these at hello bottom of hello file, just before `</resources>`.</span></span>
   
        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>
3. <span data-ttu-id="db65a-206">I din `NotificationSetting.java` lägger du till följande inställning toohello hello `NotificationSettings` klass.</span><span class="sxs-lookup"><span data-stu-id="db65a-206">In your `NotificationSetting.java` file, add hello following setting toohello `NotificationSettings` class.</span></span>
   
    <span data-ttu-id="db65a-207">Uppdatera `HubFullAccess` med hello **DefaultFullSharedAccessSignature** anslutningssträngen för din hubb.</span><span class="sxs-lookup"><span data-stu-id="db65a-207">Update `HubFullAccess` with hello **DefaultFullSharedAccessSignature** connection string for your hub.</span></span> <span data-ttu-id="db65a-208">Den här anslutningssträngen kan kopieras från hello [Azure Portal] genom att klicka på **åtkomstprinciper** på hello **inställningar** bladet för din meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="db65a-208">This connection string can be copied from hello [Azure Portal] by clicking **Access Policies** on hello **Settings** blade for your notification hub.</span></span>
   
        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccessSignature Connection string>";
4. <span data-ttu-id="db65a-209">I din `MainActivity.java` lägger du till följande hello `import` uttryck ovanför hello `MainActivity` klass.</span><span class="sxs-lookup"><span data-stu-id="db65a-209">In your `MainActivity.java` file, add hello following `import` statements above hello `MainActivity` class.</span></span>
   
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
5. <span data-ttu-id="db65a-210">I din `MainActivity.java` lägger du till följande medlemmar hello överst i hello hello `MainActivity` klass.</span><span class="sxs-lookup"><span data-stu-id="db65a-210">In your `MainActivity.java` file, add hello following members at hello top of hello `MainActivity` class.</span></span>    
   
        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;
6. <span data-ttu-id="db65a-211">Du måste skapa en token tooauthenticate Software Access Signature (SaS) en POST-begäran toosend meddelanden tooyour notification hub.</span><span class="sxs-lookup"><span data-stu-id="db65a-211">You must create a Software Access Signature (SaS) token tooauthenticate a POST request toosend messages tooyour notification hub.</span></span> <span data-ttu-id="db65a-212">Detta görs genom parsning hello nyckeldata från hello anslutningssträngen och sedan skapa hello SaS-token som anges i hello [vanliga koncept](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API-referensen.</span><span class="sxs-lookup"><span data-stu-id="db65a-212">This is done by parsing hello key data from hello connection string and then creating hello SaS token, as mentioned in hello [Common Concepts](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API reference.</span></span> <span data-ttu-id="db65a-213">hello följande kod är ett exempel på implementering.</span><span class="sxs-lookup"><span data-stu-id="db65a-213">hello following code is an example implementation.</span></span>
   
    <span data-ttu-id="db65a-214">I `MainActivity.java`, Lägg till följande metod toohello hello `MainActivity` klassen tooparse anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="db65a-214">In `MainActivity.java`, add hello following method toohello `MainActivity` class tooparse your connection string.</span></span>
   
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
7. <span data-ttu-id="db65a-215">I `MainActivity.java`, Lägg till följande metod toohello hello `MainActivity` klassen toocreate en SaS-token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="db65a-215">In `MainActivity.java`, add hello following method toohello `MainActivity` class toocreate a SaS authentication token.</span></span>
   
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
8. <span data-ttu-id="db65a-216">I `MainActivity.java`, Lägg till följande metod toohello hello `MainActivity` klassen toohandle hello **skicka meddelande** Klicka på och skicka push-meddelande för hello meddelandet toohello hubb med hjälp av hello inbyggda REST-API.</span><span class="sxs-lookup"><span data-stu-id="db65a-216">In `MainActivity.java`, add hello following method toohello `MainActivity` class toohandle hello **Send Notification** button click and send hello push notification message toohello hub by using hello built-in REST API.</span></span>
   
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

## <a name="testing-your-app"></a><span data-ttu-id="db65a-217">Testa din app</span><span class="sxs-lookup"><span data-stu-id="db65a-217">Testing your app</span></span>
#### <a name="push-notifications-in-hello-emulator"></a><span data-ttu-id="db65a-218">Push-meddelanden i emulatorn hello</span><span class="sxs-lookup"><span data-stu-id="db65a-218">Push notifications in hello emulator</span></span>
<span data-ttu-id="db65a-219">Om du vill tootest push-meddelanden inne i en emulator, se till att emulatorbilden stöder hello Google API-nivå som du har valt för din app.</span><span class="sxs-lookup"><span data-stu-id="db65a-219">If you want tootest push notifications inside an emulator, make sure that your emulator image supports hello Google API level that you chose for your app.</span></span> <span data-ttu-id="db65a-220">Om bilden inte stöder interna Google APIs, du kommer att få hello **SERVICE\_inte\_tillgänglig** undantag.</span><span class="sxs-lookup"><span data-stu-id="db65a-220">If your image doesn't support native Google APIs, you will end up with hello **SERVICE\_NOT\_AVAILABLE** exception.</span></span>

<span data-ttu-id="db65a-221">Dessutom toohello ovan, se till att du har lagt till ditt Google-konto tooyour kör emulatorns under **inställningar** > **konton**.</span><span class="sxs-lookup"><span data-stu-id="db65a-221">In addition toohello above, ensure that you have added your Google account tooyour running emulator under **Settings** > **Accounts**.</span></span> <span data-ttu-id="db65a-222">I annat fall din försök tooregister med GCM leda till hello **AUTENTISERING\_misslyckades** undantag.</span><span class="sxs-lookup"><span data-stu-id="db65a-222">Otherwise, your attempts tooregister with GCM may result in hello **AUTHENTICATION\_FAILED** exception.</span></span>

#### <a name="running-hello-application"></a><span data-ttu-id="db65a-223">Kör hello program</span><span class="sxs-lookup"><span data-stu-id="db65a-223">Running hello application</span></span>
1. <span data-ttu-id="db65a-224">Kör hello appen och Lägg märke till att hello registrerings-ID rapporteras vid en lyckad registrering.</span><span class="sxs-lookup"><span data-stu-id="db65a-224">Run hello app and notice that hello registration ID is reported for a successful registration.</span></span>
   
       ![Testing on Android - Channel registration](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)
2. <span data-ttu-id="db65a-225">Ange ett meddelande meddelandet toobe skickas tooall Android-enheter som har registrerats med hello-hubben.</span><span class="sxs-lookup"><span data-stu-id="db65a-225">Enter a notification message toobe sent tooall Android devices that have registered with hello hub.</span></span>
   
       ![Testing on Android - sending a message](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)
3. <span data-ttu-id="db65a-226">Tryck på **Skicka meddelande**.</span><span class="sxs-lookup"><span data-stu-id="db65a-226">Press **Send Notification**.</span></span> <span data-ttu-id="db65a-227">Alla enheter som har hello appen körs visas en `AlertDialog` -instans med hello push-meddelandet.</span><span class="sxs-lookup"><span data-stu-id="db65a-227">Any devices that have hello app running will show an `AlertDialog` instance with hello push notification message.</span></span> <span data-ttu-id="db65a-228">Enheter som inte har hello app körs men som tidigare hade registrerats för push-meddelanden får ett meddelande i hello Android Notification Manager.</span><span class="sxs-lookup"><span data-stu-id="db65a-228">Devices that don't have hello app running but were previously registered for push notifications will receive a notification in hello Android Notification Manager.</span></span> <span data-ttu-id="db65a-229">Dessa kan visas genom att svepa nedåt från hello övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="db65a-229">Those can be viewed by swiping down from hello upper-left corner.</span></span>
   
       ![Testing on Android - notifications](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

## <a name="next-steps"></a><span data-ttu-id="db65a-230">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db65a-230">Next steps</span></span>
<span data-ttu-id="db65a-231">Vi rekommenderar hello [använda Notification Hubs toopush meddelanden toousers] självstudiekurs som hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="db65a-231">We recommend hello [Use Notification Hubs toopush notifications toousers] tutorial as hello next step.</span></span> <span data-ttu-id="db65a-232">Den visar hur toosend meddelanden från en ASP.NET-backend med taggar tootarget specifika användare.</span><span class="sxs-lookup"><span data-stu-id="db65a-232">It will show you how toosend notifications from an ASP.NET backend using tags tootarget specific users.</span></span>

<span data-ttu-id="db65a-233">Om du vill toosegment in användarna efter intressegrupper, kolla hello [använda Notification Hubs toosend senaste nytt] kursen.</span><span class="sxs-lookup"><span data-stu-id="db65a-233">If you want toosegment your users by interest groups, check out hello [Use Notification Hubs toosend breaking news] tutorial.</span></span>

<span data-ttu-id="db65a-234">toolearn mer allmän information om Notification Hubs finns i vår [riktlinjer om Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="db65a-234">toolearn more general information about Notification Hubs, see our [Notification Hubs Guidance].</span></span>

<!-- Images. -->



<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[riktlinjer om Notification Hubs]: notification-hubs-push-notification-overview.md
[använda Notification Hubs toopush meddelanden toousers]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[använda Notification Hubs toosend senaste nytt]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure Portal]: https://portal.azure.com
