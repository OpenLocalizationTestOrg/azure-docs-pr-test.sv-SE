---
title: "aaaGet igång med Android-appar Azure Mobile Engagement"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och push-meddelanden för Android-appar."
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
ms.openlocfilehash: e8c92607691104750cdf1c4f7639a041d8a7bcd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a><span data-ttu-id="a1c2f-103">Kom igång med Azure Mobile Engagement för Android-appar</span><span class="sxs-lookup"><span data-stu-id="a1c2f-103">Get started with Azure Mobile Engagement for Android apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="a1c2f-104">Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand appanvändningen och hur toosend push-meddelanden toosegmented användare av en Android-App.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and how toosend push notifications toosegmented users of an Android application.</span></span>
<span data-ttu-id="a1c2f-105">Den här kursen visar hello enkelt scenario för sändning med Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-105">This tutorial demonstrates hello simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="a1c2f-106">I självstudiekursen skapar du en tom Android-app som samlar in grundläggande data och tar emot push-meddelanden med Google Cloud Messaging (GCM).</span><span class="sxs-lookup"><span data-stu-id="a1c2f-106">In it, you create a blank Android app that collects basic data and receives push notifications using Google Cloud Messaging (GCM).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a1c2f-107">Krav</span><span class="sxs-lookup"><span data-stu-id="a1c2f-107">Prerequisites</span></span>
<span data-ttu-id="a1c2f-108">Den här kursen krävs hello [Android Developer Tools](https://developer.android.com/sdk/index.html), som innehåller hello Android Studio integrated development environment och hello senaste Android-plattformen.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-108">Completing this tutorial requires hello [Android Developer Tools](https://developer.android.com/sdk/index.html), which includes hello Android Studio integrated development environment, and hello latest Android platform.</span></span>

<span data-ttu-id="a1c2f-109">Det kräver också hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span><span class="sxs-lookup"><span data-stu-id="a1c2f-109">It also requires hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a1c2f-110">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-110">toocomplete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="a1c2f-111">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="a1c2f-112">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="a1c2f-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).</span></span>
>
>

## <a name="set-up-mobile-engagement-for-your-android-app"></a><span data-ttu-id="a1c2f-113">Konfigurera Mobile Engagement för din Android-app</span><span class="sxs-lookup"><span data-stu-id="a1c2f-113">Set up Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-toohello-mobile-engagement-backend"></a><span data-ttu-id="a1c2f-114">Ansluta appen toohello Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="a1c2f-114">Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="a1c2f-115">Den här kursen behandlas en ”grundläggande integration”, vilket är hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-115">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="a1c2f-116">Du kan skapa en grundläggande app för Android Studio toodemonstrate hello-integration.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-116">You create a basic app with Android Studio toodemonstrate hello integration.</span></span>

<span data-ttu-id="a1c2f-117">hello fullständiga integrationsdokumentationen finns i hello [Mobile Engagement Android SDK integration](mobile-engagement-android-sdk-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a1c2f-117">hello complete integration documentation can be found in hello [Mobile Engagement Android SDK integration](mobile-engagement-android-sdk-overview.md).</span></span>

### <a name="create-an-android-project"></a><span data-ttu-id="a1c2f-118">Skapa ett Android-projekt</span><span class="sxs-lookup"><span data-stu-id="a1c2f-118">Create an Android project</span></span>
1. <span data-ttu-id="a1c2f-119">Starta **Android Studio**, och välj i popup-fönstret hello **starta ett nytt Android Studio-projekt**.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-119">Start **Android Studio**, and in hello pop-up, select **Start a new Android Studio project**.</span></span>

    ![][1]
2. <span data-ttu-id="a1c2f-120">Ange ett namn för appen och en företagsdomän.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-120">Provide an app name and company domain.</span></span> <span data-ttu-id="a1c2f-121">Anteckna vad du fyller i eftersom du kommer att behöva samma uppgifter senare.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-121">Make a note of what you are filling, because you need it later.</span></span> <span data-ttu-id="a1c2f-122">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-122">Click **Next**.</span></span>

    ![][2]
3. <span data-ttu-id="a1c2f-123">Välj hello formfaktor för mål samt API-nivå och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-123">Select hello target form factor and API level, and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a1c2f-124">Mobile Engagement kräver en API-nivå på minst 10 (Android 2.3.3).</span><span class="sxs-lookup"><span data-stu-id="a1c2f-124">Mobile Engagement requires API level 10 minimum (Android 2.3.3).</span></span>
   >
   >

    ![][3]
4. <span data-ttu-id="a1c2f-125">Välj **tom aktivitet** här, vilket är endast hello-skärmen för den här appen och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-125">Select **Blank Activity** here, which is hello only screen for this app and click **Next**.</span></span>

    ![][4]
5. <span data-ttu-id="a1c2f-126">Låt hello standardvärdena vara och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-126">Finally, leave hello defaults as is and click **Finish**.</span></span>

    ![][5]

<span data-ttu-id="a1c2f-127">Android Studio skapar nu hello demoappen där Mobile Engagement integreras.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-127">Android Studio now creates hello demo app into which we integrate Mobile Engagement.</span></span>

### <a name="include-hello-sdk-library-in-your-project"></a><span data-ttu-id="a1c2f-128">Inkludera hello SDK-biblioteket i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="a1c2f-128">Include hello SDK library in your project</span></span>
1. <span data-ttu-id="a1c2f-129">Hämta hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span><span class="sxs-lookup"><span data-stu-id="a1c2f-129">Download hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span></span>
2. <span data-ttu-id="a1c2f-130">Extrahera hello filen tooa arkivmapp i datorn.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-130">Extract hello archive file tooa folder in your computer.</span></span>
3. <span data-ttu-id="a1c2f-131">Identifiera hello .jar-biblioteket för hello aktuella versionen av detta SDK och kopiera den toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-131">Identify hello .jar library for hello current version of this SDK and copy it toohello Clipboard.</span></span>

      ![][6]
4. <span data-ttu-id="a1c2f-132">Navigera toohello **projekt** avsnittet (1) och klistra in hello .jar i biblioteksmappen hello (2).</span><span class="sxs-lookup"><span data-stu-id="a1c2f-132">Navigate toohello **Project** section (1) and paste hello .jar in hello libs folder (2).</span></span>

      ![][7]
5. <span data-ttu-id="a1c2f-133">tooload hello bibliotek, synkronisera hello projektet.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-133">tooload hello library, sync hello project .</span></span>

      ![][8]

### <a name="connect-your-app-toomobile-engagement-backend-with-hello-connection-string"></a><span data-ttu-id="a1c2f-134">Ansluta appen tooMobile Engagement-serverdelen med hello anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="a1c2f-134">Connect your app tooMobile Engagement backend with hello Connection String</span></span>
1. <span data-ttu-id="a1c2f-135">Kopiera hello följande rader med kod till hello aktivitet skapades (måste endast göras på ett ställe i appen, vanligtvis hello huvudaktiviteten).</span><span class="sxs-lookup"><span data-stu-id="a1c2f-135">Copy hello following lines of code into hello activity creation (must be done only in one place of your application, usually hello main activity).</span></span> <span data-ttu-id="a1c2f-136">Den här exempelappen öppna hello MainActivity under src -> main -> java-mappen och Lägg till hello följande:</span><span class="sxs-lookup"><span data-stu-id="a1c2f-136">For this sample app, open up hello MainActivity under src -> main -> java folder and add hello following:</span></span>

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);
2. <span data-ttu-id="a1c2f-137">Lösa hello referenser genom att trycka på Alt + Retur eller lägga till följande importuttryck hello:</span><span class="sxs-lookup"><span data-stu-id="a1c2f-137">Resolve hello references by pressing Alt + Enter or adding hello following import statements:</span></span>

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;
3. <span data-ttu-id="a1c2f-138">Gå tillbaka toohello Azure klassiska Portal i din app **anslutningsinformation** och kopiera hello **anslutningssträngen**.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-138">Go back toohello Azure Classic Portal in your app's **Connection Info** page and copy hello **Connection String**.</span></span>

      ![][9]
4. <span data-ttu-id="a1c2f-139">Klistra in den i hello `setConnectionString` parameter, ersätter hello hela strängen som visas i följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="a1c2f-139">Paste it into hello `setConnectionString` parameter, replacing hello entire string shown in hello following code:</span></span>

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a><span data-ttu-id="a1c2f-140">Lägga till behörigheter och en tjänstedeklaration</span><span class="sxs-lookup"><span data-stu-id="a1c2f-140">Add permissions and a service declaration</span></span>
1. <span data-ttu-id="a1c2f-141">Lägg till dessa behörigheter toohello Manifest.xml i ditt projekt omedelbart före eller efter hello `<application>` tagg:</span><span class="sxs-lookup"><span data-stu-id="a1c2f-141">Add these permissions toohello Manifest.xml of your project immediately before or after hello `<application>` tag:</span></span>

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
2. <span data-ttu-id="a1c2f-142">toodeclare Hej agent-tjänsten, Lägg till denna kod mellan hello `<application>` och `</application>` taggar:</span><span class="sxs-lookup"><span data-stu-id="a1c2f-142">toodeclare hello agent service, add this code between hello `<application>` and `</application>` tags:</span></span>

        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
3. <span data-ttu-id="a1c2f-143">Ersätt i hello kod du klistrade in, `"<Your application name>"` i hello etiketten som visas i hello **inställningar** menyn där du kan se tjänster som körs på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-143">In hello code you pasted, replace `"<Your application name>"` in hello label, which is displayed in hello **Settings** menu where you can see services running on hello device.</span></span> <span data-ttu-id="a1c2f-144">Du kan lägga till hello ordet ”tjänst” i etiketten t.ex.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-144">You can add hello word "Service" in that label for example.</span></span>

### <a name="send-a-screen-toomobile-engagement"></a><span data-ttu-id="a1c2f-145">Skicka en skärm tooMobile Engagement</span><span class="sxs-lookup"><span data-stu-id="a1c2f-145">Send a screen tooMobile Engagement</span></span>
<span data-ttu-id="a1c2f-146">toostart skicka data och se till att hello användarna är aktiva, måste du skicka minst en skärm (aktivitet) toohello Mobile Engagement-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-146">toostart sending data and ensure that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

<span data-ttu-id="a1c2f-147">Gå för**MainActivity.java** och Lägg till hello följande tooreplace hello basklassen **MainActivity** för**EngagementActivity**:</span><span class="sxs-lookup"><span data-stu-id="a1c2f-147">Go too**MainActivity.java** and add hello following tooreplace hello base class of **MainActivity** too**EngagementActivity**:</span></span>

    public class MainActivity extends EngagementActivity {

> [!NOTE]
> <span data-ttu-id="a1c2f-148">Om din basklass inte *aktiviteten*, se [avancerad Android-rapportering](mobile-engagement-android-advanced-reporting.md) för tooinherit från olika klasser.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-148">If your base class is not *Activity*, consult [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md) for how tooinherit from different classes.</span></span>
>
>

<span data-ttu-id="a1c2f-149">Kommentera ut hello följande rad i detta enkla Exempelscenario:</span><span class="sxs-lookup"><span data-stu-id="a1c2f-149">Comment out hello following line for this simple sample scenario:</span></span>

    // setSupportActionBar(toolbar);

<span data-ttu-id="a1c2f-150">Om du vill tookeep hello `ActionBar` i din app Se [avancerad Android-rapportering](mobile-engagement-android-advanced-reporting.md).</span><span class="sxs-lookup"><span data-stu-id="a1c2f-150">If you want tookeep hello `ActionBar` in your app, see [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md).</span></span>

## <a name="connect-app-with-real-time-monitoring"></a><span data-ttu-id="a1c2f-151">Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="a1c2f-151">Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a><span data-ttu-id="a1c2f-152">Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="a1c2f-152">Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="a1c2f-153">Under en kampanj kan du med hjälp av Mobile Engagement samverka med och nå ut till användarna med push-meddelanden och meddelanden i appen.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-153">During a campaign, Mobile Engagement lets you interact with and REACH your users with push notifications and in-app messaging.</span></span> <span data-ttu-id="a1c2f-154">Denna modul är heter REACH i hello Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-154">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="a1c2f-155">hello efter avsnittet ställer in din app tooreceive dem.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-155">hello following section sets up your app tooreceive them.</span></span>

### <a name="copy-sdk-resources-in-your-project"></a><span data-ttu-id="a1c2f-156">Kopiera SDK-resurser i ditt projekt</span><span class="sxs-lookup"><span data-stu-id="a1c2f-156">Copy SDK resources in your project</span></span>
1. <span data-ttu-id="a1c2f-157">Gå tillbaka tooyour SDK ladda ned innehåll och kopierar hello **res** mapp.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-157">Navigate back tooyour SDK download content and copy hello **res** folder.</span></span>

    ![][10]
2. <span data-ttu-id="a1c2f-158">Gå tillbaka tooAndroid Studio, Välj hello **huvudsakliga** katalogen för dina projektfiler och klistra in den tooadd hello resurser tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-158">Go back tooAndroid Studio, select hello **main** directory of your project files, and then paste it tooadd hello resources tooyour project.</span></span>

    ![][11]

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="a1c2f-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a1c2f-159">Next steps</span></span>
<span data-ttu-id="a1c2f-160">Gå för[Android SDK](mobile-engagement-android-sdk-overview.md) tooget detaljerade kunskaper om hello SDK-integration.</span><span class="sxs-lookup"><span data-stu-id="a1c2f-160">Go too[Android SDK](mobile-engagement-android-sdk-overview.md) tooget detailed knowledge about hello SDK integration.</span></span>

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
