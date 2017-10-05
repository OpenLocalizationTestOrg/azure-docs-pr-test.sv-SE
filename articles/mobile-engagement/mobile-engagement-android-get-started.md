---
title: "Komma igång med Azure Mobile Engagement för Andoid-appar"
description: "Lär dig hur du använder Azure Mobile Engagement med analyser och push-meddelanden för Android-appar."
services: mobile-engagement
documentationcenter: android
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3c286c6d-cfef-4e3e-9b2c-715429fe82db
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: dc255a930bf71e6ef6d964bc5e3472a38ce4e467
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a><span data-ttu-id="ced65-103">Komma igång med Azure Mobile Engagement för Android-appar</span><span class="sxs-lookup"><span data-stu-id="ced65-103">Get started with Azure Mobile Engagement for Android apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="ced65-104">I det här avsnittet beskrivs hur du använder Azure Mobile Engagement för att förstå appanvändningen, och hur du skickar push-meddelanden till segmenterade användare i en Android-app.</span><span class="sxs-lookup"><span data-stu-id="ced65-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and how to send push notifications to segmented users of an Android application.</span></span>
<span data-ttu-id="ced65-105">I den här självstudiekursen visas ett enkelt scenario för sändning med Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="ced65-105">This tutorial demonstrates the simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="ced65-106">I självstudiekursen skapar du en tom Android-app som samlar in grundläggande data och tar emot push-meddelanden med Google Cloud Messaging (GCM).</span><span class="sxs-lookup"><span data-stu-id="ced65-106">In it, you create a blank Android app that collects basic data and receives push notifications using Google Cloud Messaging (GCM).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ced65-107">Krav</span><span class="sxs-lookup"><span data-stu-id="ced65-107">Prerequisites</span></span>
<span data-ttu-id="ced65-108">För den här kursen krävs [Android Developer Tools](https://developer.android.com/sdk/index.html) (utvecklingsverktyg för Android) som omfattar Android Studio Integrated Development Environment och den senaste Android-plattformen.</span><span class="sxs-lookup"><span data-stu-id="ced65-108">Completing this tutorial requires the [Android Developer Tools](https://developer.android.com/sdk/index.html), which includes the Android Studio integrated development environment, and the latest Android platform.</span></span>

<span data-ttu-id="ced65-109">Du behöver även [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span><span class="sxs-lookup"><span data-stu-id="ced65-109">It also requires the [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ced65-110">Du behöver ett Azure-konto för att slutföra den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="ced65-110">To complete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="ced65-111">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="ced65-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ced65-112">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="ced65-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).</span></span>
>
>

## <a name="set-up-mobile-engagement-for-your-android-app"></a><span data-ttu-id="ced65-113">Konfigurera Mobile Engagement för din Android-app</span><span class="sxs-lookup"><span data-stu-id="ced65-113">Set up Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a><span data-ttu-id="ced65-114">Ansluta appen till Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="ced65-114">Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="ced65-115">I den här kursen behandlas en ”grundläggande integration”, vilket är den minsta uppsättningen som krävs för att samla in data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="ced65-115">This tutorial presents a "basic integration", which is the minimal set required to collect data and send a push notification.</span></span> <span data-ttu-id="ced65-116">Du skapar en grundläggande app i Android Studio för att demonstrera integrationen.</span><span class="sxs-lookup"><span data-stu-id="ced65-116">You create a basic app with Android Studio to demonstrate the integration.</span></span>

<span data-ttu-id="ced65-117">Den fullständiga integrationsdokumentationen finns i [Mobile Engagement Android SDK-integration](mobile-engagement-android-sdk-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ced65-117">The complete integration documentation can be found in the [Mobile Engagement Android SDK integration](mobile-engagement-android-sdk-overview.md).</span></span>

### <a name="create-an-android-project"></a><span data-ttu-id="ced65-118">Skapa ett Android-projekt</span><span class="sxs-lookup"><span data-stu-id="ced65-118">Create an Android project</span></span>
1. <span data-ttu-id="ced65-119">Starta **Android Studio** och välj **Start a new Android Studio project** (Starta ett nytt Android Studio-projekt) i popup-fönstret.</span><span class="sxs-lookup"><span data-stu-id="ced65-119">Start **Android Studio**, and in the pop-up, select **Start a new Android Studio project**.</span></span>

    ![][1]
2. <span data-ttu-id="ced65-120">Ange ett namn för appen och en företagsdomän.</span><span class="sxs-lookup"><span data-stu-id="ced65-120">Provide an app name and company domain.</span></span> <span data-ttu-id="ced65-121">Anteckna vad du fyller i eftersom du kommer att behöva samma uppgifter senare.</span><span class="sxs-lookup"><span data-stu-id="ced65-121">Make a note of what you are filling, because you need it later.</span></span> <span data-ttu-id="ced65-122">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="ced65-122">Click **Next**.</span></span>

    ![][2]
3. <span data-ttu-id="ced65-123">Välj formfaktor för mål samt API-nivå och klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="ced65-123">Select the target form factor and API level, and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ced65-124">Mobile Engagement kräver en API-nivå på minst 10 (Android 2.3.3).</span><span class="sxs-lookup"><span data-stu-id="ced65-124">Mobile Engagement requires API level 10 minimum (Android 2.3.3).</span></span>
   >
   >

    ![][3]
4. <span data-ttu-id="ced65-125">Välj **Tom aktivitet** och klicka på **Nästa**. Det här är den enda skärm som visas i appen.</span><span class="sxs-lookup"><span data-stu-id="ced65-125">Select **Blank Activity** here, which is the only screen for this app and click **Next**.</span></span>

    ![][4]
5. <span data-ttu-id="ced65-126">Låt standardvärdena vara och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="ced65-126">Finally, leave the defaults as is and click **Finish**.</span></span>

    ![][5]

<span data-ttu-id="ced65-127">Android Studio skapar nu demoappen där Mobile Engagement ska integreras.</span><span class="sxs-lookup"><span data-stu-id="ced65-127">Android Studio now creates the demo app into which we integrate Mobile Engagement.</span></span>

### <a name="include-the-sdk-library-in-your-project"></a><span data-ttu-id="ced65-128">Inkludera SDK-biblioteket i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="ced65-128">Include the SDK library in your project</span></span>
1. <span data-ttu-id="ced65-129">Hämta [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span><span class="sxs-lookup"><span data-stu-id="ced65-129">Download the [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span></span>
2. <span data-ttu-id="ced65-130">Extrahera arkivfilen till en mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="ced65-130">Extract the archive file to a folder in your computer.</span></span>
3. <span data-ttu-id="ced65-131">Hitta .jar-biblioteket för den aktuella SDK-versionen och kopiera det till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="ced65-131">Identify the .jar library for the current version of this SDK and copy it to the Clipboard.</span></span>

      ![][6]
4. <span data-ttu-id="ced65-132">Navigera till avsnittet **Projekt** (1) och klistra in .jar i biblioteksmappen (2).</span><span class="sxs-lookup"><span data-stu-id="ced65-132">Navigate to the **Project** section (1) and paste the .jar in the libs folder (2).</span></span>

      ![][7]
5. <span data-ttu-id="ced65-133">Synkronisera projektet för att läsa in biblioteket.</span><span class="sxs-lookup"><span data-stu-id="ced65-133">To load the library, sync the project .</span></span>

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a><span data-ttu-id="ced65-134">Anslut appen till Mobile Engagement-serverdelen med anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="ced65-134">Connect your app to Mobile Engagement backend with the Connection String</span></span>
1. <span data-ttu-id="ced65-135">Kopiera följande rader med kod till den aktivitet som skapas (får endast göras på ett ställe i appen, vanligtvis i huvudaktiviteten).</span><span class="sxs-lookup"><span data-stu-id="ced65-135">Copy the following lines of code into the activity creation (must be done only in one place of your application, usually the main activity).</span></span> <span data-ttu-id="ced65-136">I den här exempelappen öppnar du MainActivity i mappen src -> main -> java och lägger till följande:</span><span class="sxs-lookup"><span data-stu-id="ced65-136">For this sample app, open up the MainActivity under src -> main -> java folder and add the following:</span></span>

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);
2. <span data-ttu-id="ced65-137">Matcha referenserna genom att trycka på Alt + Retur eller lägga till följande importuttryck:</span><span class="sxs-lookup"><span data-stu-id="ced65-137">Resolve the references by pressing Alt + Enter or adding the following import statements:</span></span>

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;
3. <span data-ttu-id="ced65-138">Gå tillbaka till den klassiska Azure-portalen via appsidan **Anslutningsinformation** och kopiera **anslutningssträngen**.</span><span class="sxs-lookup"><span data-stu-id="ced65-138">Go back to the Azure Classic Portal in your app's **Connection Info** page and copy the **Connection String**.</span></span>

      ![][9]
4. <span data-ttu-id="ced65-139">Klistra in den i `setConnectionString`-parametern och ersätt hela strängen som visas i följande kod:</span><span class="sxs-lookup"><span data-stu-id="ced65-139">Paste it into the `setConnectionString` parameter, replacing the entire string shown in the following code:</span></span>

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a><span data-ttu-id="ced65-140">Lägg till behörigheter och en tjänstedeklaration</span><span class="sxs-lookup"><span data-stu-id="ced65-140">Add permissions and a service declaration</span></span>
1. <span data-ttu-id="ced65-141">Lägg till följande behörigheter i Manifest.xml i ditt projekt omedelbart före eller efter taggen `<application>`:</span><span class="sxs-lookup"><span data-stu-id="ced65-141">Add these permissions to the Manifest.xml of your project immediately before or after the `<application>` tag:</span></span>

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
2. <span data-ttu-id="ced65-142">För att deklarera agenttjänsten lägger du till följande kod mellan taggarna `<application>` och `</application>`:</span><span class="sxs-lookup"><span data-stu-id="ced65-142">To declare the agent service, add this code between the `<application>` and `</application>` tags:</span></span>

        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
3. <span data-ttu-id="ced65-143">I den kod som du klistrade in ersätter du `"<Your application name>"` i etiketten som visas i menyn **Inställningar** där du kan se tjänster som körs på enheten.</span><span class="sxs-lookup"><span data-stu-id="ced65-143">In the code you pasted, replace `"<Your application name>"` in the label, which is displayed in the **Settings** menu where you can see services running on the device.</span></span> <span data-ttu-id="ced65-144">Du kan till exempel lägga till ordet ”Service” eller ”Tjänst” i etiketten.</span><span class="sxs-lookup"><span data-stu-id="ced65-144">You can add the word "Service" in that label for example.</span></span>

### <a name="send-a-screen-to-mobile-engagement"></a><span data-ttu-id="ced65-145">Skicka en skärm till Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="ced65-145">Send a screen to Mobile Engagement</span></span>
<span data-ttu-id="ced65-146">För att kunna börja skicka data och försäkra dig om att användarna är aktiva måste du skicka minst en skärm (aktivitet) till Mobile Engagement-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="ced65-146">To start sending data and ensure that the users are active, you must send at least one screen (Activity) to the Mobile Engagement backend.</span></span>

<span data-ttu-id="ced65-147">Gå till **MainActivity.java** och lägg till följande om du vill byta ut basklassen **MainActivity** mot **EngagementActivity**:</span><span class="sxs-lookup"><span data-stu-id="ced65-147">Go to **MainActivity.java** and add the following to replace the base class of **MainActivity** to **EngagementActivity**:</span></span>

    public class MainActivity extends EngagementActivity {

> [!NOTE]
> <span data-ttu-id="ced65-148">Om din basklass inte är *aktivitet* läser du [Avancerad Android-rapportering](mobile-engagement-android-advanced-reporting.md) för att få veta hur du gör för att ärva från olika klasser.</span><span class="sxs-lookup"><span data-stu-id="ced65-148">If your base class is not *Activity*, consult [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md) for how to inherit from different classes.</span></span>
>
>

<span data-ttu-id="ced65-149">Kommentera ut följande rad i detta enkla exempelscenario:</span><span class="sxs-lookup"><span data-stu-id="ced65-149">Comment out the following line for this simple sample scenario:</span></span>

    // setSupportActionBar(toolbar);

<span data-ttu-id="ced65-150">Om du vill behålla `ActionBar` i din app kan du läsa [Avancerad Android-rapportering](mobile-engagement-android-advanced-reporting.md).</span><span class="sxs-lookup"><span data-stu-id="ced65-150">If you want to keep the `ActionBar` in your app, see [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md).</span></span>

## <a name="connect-app-with-real-time-monitoring"></a><span data-ttu-id="ced65-151">Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="ced65-151">Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a><span data-ttu-id="ced65-152">Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="ced65-152">Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="ced65-153">Under en kampanj kan du med hjälp av Mobile Engagement samverka med och nå ut till användarna med push-meddelanden och meddelanden i appen.</span><span class="sxs-lookup"><span data-stu-id="ced65-153">During a campaign, Mobile Engagement lets you interact with and REACH your users with push notifications and in-app messaging.</span></span> <span data-ttu-id="ced65-154">Modulen som används för det heter REACH och finns i Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="ced65-154">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="ced65-155">I följande avsnitt konfigurerar du appen för att ta emot dem.</span><span class="sxs-lookup"><span data-stu-id="ced65-155">The following section sets up your app to receive them.</span></span>

### <a name="copy-sdk-resources-in-your-project"></a><span data-ttu-id="ced65-156">Kopiera SDK-resurser i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="ced65-156">Copy SDK resources in your project</span></span>
1. <span data-ttu-id="ced65-157">Gå tillbaka till den nedladdade SDK-filen och kopiera mappen **res**.</span><span class="sxs-lookup"><span data-stu-id="ced65-157">Navigate back to your SDK download content and copy the **res** folder.</span></span>

    ![][10]
2. <span data-ttu-id="ced65-158">Gå tillbaka till Android Studio, markera **huvudkatalogen (main)** för dina projektfiler och klistra in den för att lägga till resurser i projektet.</span><span class="sxs-lookup"><span data-stu-id="ced65-158">Go back to Android Studio, select the **main** directory of your project files, and then paste it to add the resources to your project.</span></span>

    ![][11]

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="ced65-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ced65-159">Next steps</span></span>
<span data-ttu-id="ced65-160">Gå till [Android SDK](mobile-engagement-android-sdk-overview.md) för detaljerad information om SDK-integration.</span><span class="sxs-lookup"><span data-stu-id="ced65-160">Go to [Android SDK](mobile-engagement-android-sdk-overview.md) to get detailed knowledge about the SDK integration.</span></span>

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
