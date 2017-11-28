---
title: "aaaGet igång med Azure Mobile Engagement för Cordova/Phonegap"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och Push-meddelanden för Cordova/Phonegap-appar."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 54fe9113-e239-4ed7-9fd1-a502d7ac7f47
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-phonegap
ms.devlang: js
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: e67dabbdf7886802bb058f38964e558d5ae6854c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a><span data-ttu-id="c08db-103">Komma igång med Azure Mobile Engagement för Cordova/Phonegap</span><span class="sxs-lookup"><span data-stu-id="c08db-103">Get Started with Azure Mobile Engagement for Cordova/Phonegap</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="c08db-104">Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand användnings- och skicka push-meddelanden toosegmented användare av ditt program för mobila program som utvecklats med Cordova.</span><span class="sxs-lookup"><span data-stu-id="c08db-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users for a mobile application developed with Cordova.</span></span>

<span data-ttu-id="c08db-105">I den här självstudiekursen skapar vi en tom Cordova-app med Mac och integrerar sedan Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="c08db-105">In this tutorial, we will create a blank Cordova app using Mac and then integrate Mobile Engagement SDK.</span></span> <span data-ttu-id="c08db-106">Den samlar in grundläggande analysdata och tar emot push-meddelanden via Apple Push Notification System (APNS) för iOS och Google Cloud Messaging (GCM) för Android.</span><span class="sxs-lookup"><span data-stu-id="c08db-106">It collects basic analytics data and receives push notifications using Apple Push Notification System (APNS) for iOS and Google Cloud Messaging (GCM) for Android.</span></span> <span data-ttu-id="c08db-107">Vi distribuerar den här tooan iOS eller Android-enhet för testning.</span><span class="sxs-lookup"><span data-stu-id="c08db-107">We will deploy this tooan iOS or Android device for testing.</span></span> 

> [!NOTE]
> <span data-ttu-id="c08db-108">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c08db-108">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="c08db-109">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="c08db-109">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c08db-110">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).</span><span class="sxs-lookup"><span data-stu-id="c08db-110">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).</span></span>
> 
> 

<span data-ttu-id="c08db-111">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="c08db-111">This tutorial requires hello following:</span></span>

* <span data-ttu-id="c08db-112">XCode som du kan installera från Mac App Store (för att distribuera tooiOS)</span><span class="sxs-lookup"><span data-stu-id="c08db-112">XCode, which you can install from Mac App Store (for deploying tooiOS)</span></span>
* <span data-ttu-id="c08db-113">[Android SDK & Emulator](http://developer.android.com/sdk/installing/index.html) (för att distribuera tooAndroid)</span><span class="sxs-lookup"><span data-stu-id="c08db-113">[Android SDK & Emulator](http://developer.android.com/sdk/installing/index.html) (for deploying tooAndroid)</span></span>
* <span data-ttu-id="c08db-114">Certifikat för push-meddelanden (.p12) som kan hämtas från Apple Dev Center för APNS</span><span class="sxs-lookup"><span data-stu-id="c08db-114">Push notification certificate (.p12) that you can obtain from Apple Dev Center for APNS</span></span>
* <span data-ttu-id="c08db-115">Ett GCM-projektnummer som kan hämtas från Google Developer Console för GCM</span><span class="sxs-lookup"><span data-stu-id="c08db-115">GCM Project number that you can obtain from your Google Developer Console for GCM</span></span>
* [<span data-ttu-id="c08db-116">Cordova-pluginprogrammet för Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="c08db-116">Mobile Engagement Cordova Plugin</span></span>](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [!NOTE]
> <span data-ttu-id="c08db-117">Du kan hitta hello källkoden och hello viktigt för hello Cordova-pluginprogrammet på [GitHub](https://github.com/Azure/azure-mobile-engagement-cordova)</span><span class="sxs-lookup"><span data-stu-id="c08db-117">You can find hello source code and hello ReadMe for hello Cordova plugin on [GitHub](https://github.com/Azure/azure-mobile-engagement-cordova)</span></span>
> 
> 

## <span data-ttu-id="c08db-118"><a id="setup-azme"></a>Konfigurera Mobile Engagement för din Cordova-app</span><span class="sxs-lookup"><span data-stu-id="c08db-118"><a id="setup-azme"></a>Setup Mobile Engagement for your Cordova app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="c08db-119"><a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="c08db-119"><a id="connecting-app"></a>Connecting your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="c08db-120">Den här kursen behandlas en ”grundläggande integration” som hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c08db-120">This tutorial presents a "basic integration" which is hello minimal set required toocollect data and send a push notification.</span></span> 

<span data-ttu-id="c08db-121">Vi skapar en grundläggande app i Cordova toodemonstrate hello integrering:</span><span class="sxs-lookup"><span data-stu-id="c08db-121">We will create a basic app with Cordova toodemonstrate hello integration:</span></span>

### <a name="create-a-new-cordova-project"></a><span data-ttu-id="c08db-122">Skapa ett nytt Cordova-projekt</span><span class="sxs-lookup"><span data-stu-id="c08db-122">Create a new Cordova project</span></span>
1. <span data-ttu-id="c08db-123">Starta *Terminal* fönster på din Mac-datorn och Skriv-hello efter som skapar ett nytt Cordova-projekt från hello standardmallen.</span><span class="sxs-lookup"><span data-stu-id="c08db-123">Launch *Terminal* window on your Mac machine and type hello following which will create a new Cordova project from hello default template.</span></span> <span data-ttu-id="c08db-124">Kontrollera att hello publicering profil du slutligen Använd toodeploy iOS-appen använder ”com.mycompany.myapp” som hello App-ID.</span><span class="sxs-lookup"><span data-stu-id="c08db-124">Make sure that hello publishing profile you eventually use toodeploy your iOS app is using 'com.mycompany.myapp' as hello App ID.</span></span> 
   
        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova
2. <span data-ttu-id="c08db-125">Köra hello följande tooconfigure ditt projekt för **iOS** och kör det i hello iOS-simulatorn:</span><span class="sxs-lookup"><span data-stu-id="c08db-125">Execute hello following tooconfigure your project for **iOS** and run it in hello iOS Simulator:</span></span>
   
        $ cordova platform add ios 
        $ cordova run ios
3. <span data-ttu-id="c08db-126">Köra hello följande tooconfigure ditt projekt för **Android** och kör det i hello Android-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c08db-126">Execute hello following tooconfigure your project for **Android** and run it in hello Android emulator.</span></span> <span data-ttu-id="c08db-127">Se till att inställningarna för Android SDK Emulator har mål som Google APIs (Google Inc.) med hello CPU / ABI som Google APIs ARM.</span><span class="sxs-lookup"><span data-stu-id="c08db-127">Make sure that your Android SDK Emulator settings have its Target as Google APIs (Google Inc.) with hello CPU / ABI as Google APIs ARM.</span></span>  
   
        $ cordova platform add android
        $ cordova run android
4. <span data-ttu-id="c08db-128">Lägg till hello Cordova Console plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="c08db-128">Add hello Cordova Console plugin.</span></span> 

    ```
    $ cordova plugin add cordova-plugin-console
    ``` 

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="c08db-129">Ansluta appen tooMobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="c08db-129">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="c08db-130">Installera hello Azure Mobile Engagement Cordova-pluginprogrammet samtidigt som man hello variabelvärden tooconfigure hello plugin-program:</span><span class="sxs-lookup"><span data-stu-id="c08db-130">Install hello Azure Mobile Engagement Cordova plugin while providing hello variable values tooconfigure hello plugin:</span></span>
   
        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
             --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers hello app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

<span data-ttu-id="c08db-131">*Android Reach-ikon* : måste vara hello namnet på hello resursen utan filtillägg eller drawable-prefix (exempel: mynotificationicon), och hello ikonfilen måste kopieras till din android-projekt (plattformar/android/res/drawable)</span><span class="sxs-lookup"><span data-stu-id="c08db-131">*Android Reach Icon* : must be hello name of hello resource without any extension, nor drawable prefix (ex: mynotificationicon), and hello icon file must be copied into your android project (platforms/android/res/drawable)</span></span>

<span data-ttu-id="c08db-132">*iOS Reach-ikon* : måste vara hello namnet på hello resursen med filtillägg (exempel: mynotificationicon.png), och hello ikonfilen måste läggas till i ditt iOS-projekt med XCode (Använd hello Lägg till filer menyn)</span><span class="sxs-lookup"><span data-stu-id="c08db-132">*iOS Reach Icon*  : must be hello name of hello resource with its extension (ex:  mynotificationicon.png), and hello icon file must be added into your iOS project with XCode (using hello Add Files Menu)</span></span>

## <span data-ttu-id="c08db-133"><a id="monitor"></a>Aktivera realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="c08db-133"><a id="monitor"></a>Enabling real-time monitoring</span></span>
1. <span data-ttu-id="c08db-134">Hej Cordova-projekt - redigera **www/js/index.js** tooadd hello anropet tooMobile Engagement toodeclare en ny aktivitet när hello *deviceReady* händelsen har tagits emot.</span><span class="sxs-lookup"><span data-stu-id="c08db-134">In hello Cordova project - edit **www/js/index.js** tooadd hello call tooMobile Engagement toodeclare a new activity once hello *deviceReady* event is received.</span></span>
   
         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }
2. <span data-ttu-id="c08db-135">Kör programmet hello:</span><span class="sxs-lookup"><span data-stu-id="c08db-135">Run hello application:</span></span>
   
   * <span data-ttu-id="c08db-136">**För iOS**</span><span class="sxs-lookup"><span data-stu-id="c08db-136">**For iOS**</span></span>
     
       <span data-ttu-id="c08db-137">I `Terminal` fönstret starta din app i en ny instans av simulatorn genom att köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="c08db-137">In `Terminal` window launch your app in a new Simulator instance by executing hello following:</span></span>
     
           cordova run ios
   * <span data-ttu-id="c08db-138">**För Android**</span><span class="sxs-lookup"><span data-stu-id="c08db-138">**For Android**</span></span>
     
       <span data-ttu-id="c08db-139">I `Terminal` fönstret starta din app i en ny instans av emulatorn genom att köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="c08db-139">In `Terminal` window launch your app in a new emulator instance by executing hello following:</span></span>
     
           cordova run android
3. <span data-ttu-id="c08db-140">Du kan se hello följande i hello loggarna för konsolen:</span><span class="sxs-lookup"><span data-stu-id="c08db-140">You can see hello following in hello console logs:</span></span>
   
        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

## <span data-ttu-id="c08db-141"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="c08db-141"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="c08db-142"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="c08db-142"><a id="integrate-push"></a>Enabling Push Notifications and in-app messaging</span></span>
<span data-ttu-id="c08db-143">Mobile Engagement kan toointeract med dina användare med hjälp av Push-meddelanden och appmeddelanden i hello kontext kampanjer.</span><span class="sxs-lookup"><span data-stu-id="c08db-143">Mobile Engagement allows you toointeract with your users using Push Notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="c08db-144">Denna modul är heter REACH i hello Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="c08db-144">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="c08db-145">hello följande avsnitt konfigurerar din app tooreceive dem.</span><span class="sxs-lookup"><span data-stu-id="c08db-145">hello following sections will setup your app tooreceive them.</span></span>

### <a name="configure-push-credentials-for-mobile-engagement"></a><span data-ttu-id="c08db-146">Konfigurera push-autentiseringsuppgifter för Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="c08db-146">Configure Push credentials for Mobile Engagement</span></span>
<span data-ttu-id="c08db-147">tooallow Mobile Engagement toosend Push-meddelanden för din räkning, behöver du toogrant komma åt tooyour Apple iOS-certifikat eller GCM Server API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="c08db-147">tooallow Mobile Engagement toosend Push Notifications on your behalf, you need toogrant it access tooyour Apple iOS certificate or GCM Server API Key.</span></span> 

1. <span data-ttu-id="c08db-148">Navigera tooyour Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="c08db-148">Navigate tooyour Mobile Engagement portal.</span></span> <span data-ttu-id="c08db-149">Kontrollera att du befinner dig i hello app vi använder för det här projektet och klicka sedan på hello **starta** knappen längst ned hello:</span><span class="sxs-lookup"><span data-stu-id="c08db-149">Ensure you're in hello app we're using for this project and then click on hello **Engage** button at hello bottom:</span></span>
   
    ![][1]
2. <span data-ttu-id="c08db-150">Du hamnar på hello inställningssidan i Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="c08db-150">You will land in hello settings page in your Engagement Portal.</span></span> <span data-ttu-id="c08db-151">Därifrån klickar du på hello **Native Push** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="c08db-151">From there click on hello **Native Push** section:</span></span>
   
    ![][2]
3. <span data-ttu-id="c08db-152">Konfigurera iOS-certifikat/API-nyckel för GCM-server</span><span class="sxs-lookup"><span data-stu-id="c08db-152">Configure iOS Certificate/GCM Server API Key</span></span>
   
    <span data-ttu-id="c08db-153">**[iOS]**</span><span class="sxs-lookup"><span data-stu-id="c08db-153">**[iOS]**</span></span>
   
    <span data-ttu-id="c08db-154">a.</span><span class="sxs-lookup"><span data-stu-id="c08db-154">a.</span></span> <span data-ttu-id="c08db-155">Välj ditt .p12-certifikat, ladda upp det och ange ditt lösenord:</span><span class="sxs-lookup"><span data-stu-id="c08db-155">Select your .p12, upload it and type your password:</span></span>
   
    ![][3]
   
    <span data-ttu-id="c08db-156">**[Android]**</span><span class="sxs-lookup"><span data-stu-id="c08db-156">**[Android]**</span></span>
   
    <span data-ttu-id="c08db-157">a.</span><span class="sxs-lookup"><span data-stu-id="c08db-157">a.</span></span> <span data-ttu-id="c08db-158">Klicka på hello redigeringsikonen framför **API-nyckel** hello GCM-inställningar och hello popup-fönster som visas, klistra in hello GCM-servernyckeln och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c08db-158">Click hello edit icon in front of **API Key** in hello GCM Settings section and in hello popup which shows up, paste hello GCM Server Key and click **OK**.</span></span> 
   
    ![][4]

### <a name="enable-push-notifications-in-hello-cordova-app"></a><span data-ttu-id="c08db-159">Aktivera push-meddelanden i hello Cordova-app</span><span class="sxs-lookup"><span data-stu-id="c08db-159">Enable push notifications in hello Cordova app</span></span>
<span data-ttu-id="c08db-160">Redigera **www/js/index.js** tooadd hello anropet tooMobile Engagement toorequest push-meddelanden och deklarerar en hanterare:</span><span class="sxs-lookup"><span data-stu-id="c08db-160">Edit **www/js/index.js** tooadd hello call tooMobile Engagement toorequest push notifications and declare a handler:</span></span>

     onDeviceReady: function() {
           Engagement.initializeReach(  
                 // on OpenUrl  
                 function(_url) {   
                 alert(_url);   
                 });  
            Engagement.startActivity("myPage",{});  
        }

### <a name="run-hello-app"></a><span data-ttu-id="c08db-161">Kör hello-appen</span><span class="sxs-lookup"><span data-stu-id="c08db-161">Run hello app</span></span>
<span data-ttu-id="c08db-162">**[iOS]**</span><span class="sxs-lookup"><span data-stu-id="c08db-162">**[iOS]**</span></span>

1. <span data-ttu-id="c08db-163">Vi använder XCode toobuild och distribuera hello-appen på hello enheten tootest push-meddelanden eftersom iOS endast tillåter push-meddelanden tooan faktisk enhet.</span><span class="sxs-lookup"><span data-stu-id="c08db-163">We will use XCode toobuild and deploy hello app on hello device tootest push notifications since iOS only allows push notifications tooan actual device.</span></span> <span data-ttu-id="c08db-164">Gå toohello platsen där Cordova-projektet har skapats och navigera för**...\platforms\ios** plats.</span><span class="sxs-lookup"><span data-stu-id="c08db-164">Go toohello location where your Cordova project is created and navigate too**...\platforms\ios** location.</span></span> <span data-ttu-id="c08db-165">Öppna filen för hello interna .xcodeproj i XCode.</span><span class="sxs-lookup"><span data-stu-id="c08db-165">Open up hello native .xcodeproj file in XCode.</span></span> 
2. <span data-ttu-id="c08db-166">Skapa och distribuera hello Cordova-app toohello iOS-enhet med hello-konto som har hello etableringsprofil som innehåller hello certifikat du just har överfört toohello Mobile Engagement-portalen och hello App-Id som matchar hello något du angav när du skapar Hej Cordova-app.</span><span class="sxs-lookup"><span data-stu-id="c08db-166">Build and deploy hello Cordova app toohello iOS device using hello account which has hello provisioning profile containing hello certificate you just uploaded toohello Mobile Engagement portal and hello App Id which matches hello one you provided while creating hello Cordova app.</span></span> <span data-ttu-id="c08db-167">Du kan checka ut hello *paketidentifierare* i din **resurser\*-info.plist** filen i XCode toomatch upp.</span><span class="sxs-lookup"><span data-stu-id="c08db-167">You can check out hello *Bundle identifier* in your **Resources\*-info.plist** file in XCode toomatch it up.</span></span> 
3. <span data-ttu-id="c08db-168">Hello standard iOS popup på enheten om appen hello begär behörighet toosend meddelanden visas.</span><span class="sxs-lookup"><span data-stu-id="c08db-168">You will see hello standard iOS popup on your device saying that hello app requests permission toosend notifications.</span></span> <span data-ttu-id="c08db-169">Bevilja behörighet att hello.</span><span class="sxs-lookup"><span data-stu-id="c08db-169">Grant hello permission.</span></span> 

<span data-ttu-id="c08db-170">**[Android]**</span><span class="sxs-lookup"><span data-stu-id="c08db-170">**[Android]**</span></span>

<span data-ttu-id="c08db-171">Du behöver bara använda hello emulatorn toorun hello Android-app som GCM-meddelanden stöds hello Android-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c08db-171">You can simply use hello emulator toorun hello Android app as GCM notifications are supported on hello Android emulator.</span></span> 

    cordova run android

## <span data-ttu-id="c08db-172"><a id="send"></a>Skicka en avisering tooyour app</span><span class="sxs-lookup"><span data-stu-id="c08db-172"><a id="send"></a>Send a notification tooyour app</span></span>
<span data-ttu-id="c08db-173">Nu ska vi skapa en enkel Push-meddelandekampanj som skickar ett push tooyour app som körs på hello enhet:</span><span class="sxs-lookup"><span data-stu-id="c08db-173">We will now create a simple Push Notification campaign that will send a push tooyour app running on hello device:</span></span>

1. <span data-ttu-id="c08db-174">Navigera toohello **nå** fliken i din Mobile Engagement-portal</span><span class="sxs-lookup"><span data-stu-id="c08db-174">Navigate toohello **Reach** tab in your Mobile Engagement portal</span></span>
2. <span data-ttu-id="c08db-175">Klicka på **nytt meddelande** toocreate push-kampanj</span><span class="sxs-lookup"><span data-stu-id="c08db-175">Click **New Announcement** toocreate your push campaign</span></span>
   
    ![][6]
3. <span data-ttu-id="c08db-176">Ange indata toocreate din kampanj **[Android]**</span><span class="sxs-lookup"><span data-stu-id="c08db-176">Provide inputs toocreate your campaign **[Android]**</span></span>
   
   * <span data-ttu-id="c08db-177">Ge kampanjen ett **namn**.</span><span class="sxs-lookup"><span data-stu-id="c08db-177">Provide a **Name** for your campaign.</span></span> 
   * <span data-ttu-id="c08db-178">Välj hello **Leveranstyp** som *Systemmeddelande* *enkel*</span><span class="sxs-lookup"><span data-stu-id="c08db-178">Select hello **Delivery Type** as *System notification* *Simple*</span></span>
   * <span data-ttu-id="c08db-179">Välj hello **leveranstiden** som *”helst”*</span><span class="sxs-lookup"><span data-stu-id="c08db-179">Select hello **Delivery time** as *"Any Time"*</span></span>
   * <span data-ttu-id="c08db-180">Ange en **rubrik** som kommer att vara hello första raden i hello push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c08db-180">Provide a **Title** for your notification which will be hello first line in hello push.</span></span>
   * <span data-ttu-id="c08db-181">Ange en **meddelandet** som fungerar som meddelandetexten hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c08db-181">Provide a **Message** for your notification which will serve as hello message body.</span></span> 
     
     ![][11]
4. <span data-ttu-id="c08db-182">Ange indata toocreate din kampanj **[iOS]**</span><span class="sxs-lookup"><span data-stu-id="c08db-182">Provide inputs toocreate your campaign **[iOS]**</span></span>
   
   * <span data-ttu-id="c08db-183">Ge kampanjen ett **namn**.</span><span class="sxs-lookup"><span data-stu-id="c08db-183">Provide a **Name** for your campaign.</span></span> 
   * <span data-ttu-id="c08db-184">Välj hello **leveranstiden** som *”endast utanför appen”*</span><span class="sxs-lookup"><span data-stu-id="c08db-184">Select hello **Delivery time** as *"Out of app only"*</span></span>
   * <span data-ttu-id="c08db-185">Ange en **rubrik** som kommer att vara hello första raden i hello push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c08db-185">Provide a **Title** for your notification which will be hello first line in hello push.</span></span>
   * <span data-ttu-id="c08db-186">Ange en **meddelandet** som fungerar som meddelandetexten hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c08db-186">Provide a **Message** for your notification which will serve as hello message body.</span></span> 
     
     ![][12]
5. <span data-ttu-id="c08db-187">Bläddra nedåt och i hello innehållsavsnitt väljer **endast meddelande**</span><span class="sxs-lookup"><span data-stu-id="c08db-187">Scroll down, and in hello content section select **Notification only**</span></span>
   
    ![][8]
6. <span data-ttu-id="c08db-188">[Valfritt] Du kan även ange åtgärdens URL-adress.</span><span class="sxs-lookup"><span data-stu-id="c08db-188">[Optional] You can also provide an Action URL.</span></span> <span data-ttu-id="c08db-189">Se till att den använder en URL-schema som hello plugin-programmets **AZME\_OMDIRIGERA\_URL** variabeln t.ex. *myapp://test*.</span><span class="sxs-lookup"><span data-stu-id="c08db-189">Make sure that it uses a URL scheme provided while configuring hello plugin's **AZME\_REDIRECT\_URL** variable e.g. *myapp://test*.</span></span>  
7. <span data-ttu-id="c08db-190">Du är klar inställningen hello mest grundläggande kampanjen möjligt.</span><span class="sxs-lookup"><span data-stu-id="c08db-190">You're done setting hello most basic campaign possible.</span></span> <span data-ttu-id="c08db-191">Bläddra nedåt igen och klicka på hello **skapa** knappen toosave din kampanj.</span><span class="sxs-lookup"><span data-stu-id="c08db-191">Now scroll down again and click hello **Create** button toosave your campaign.</span></span>
8. <span data-ttu-id="c08db-192">Slutligen **aktiverar** du din kampanj</span><span class="sxs-lookup"><span data-stu-id="c08db-192">Finally **Activate** your campaign</span></span>
   
    ![][10]
9. <span data-ttu-id="c08db-193">Du bör nu se ett push-meddelande på enheten eller i emulatorn som en del av den här kampanjen.</span><span class="sxs-lookup"><span data-stu-id="c08db-193">You should now see a push notification on your device or emulator as part of this campaign.</span></span> 

## <span data-ttu-id="c08db-194"><a id="next-steps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c08db-194"><a id="next-steps"></a>Next Steps</span></span>
[<span data-ttu-id="c08db-195">Översikt över alla metoder som är tillgängliga med Cordova Mobile Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="c08db-195">Overview of all methods available with Cordova Mobile Engagement SDK</span></span>](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

