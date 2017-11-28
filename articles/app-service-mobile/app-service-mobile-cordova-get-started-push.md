---
title: aaaAdd Push-meddelanden tooApache Cordova-App med Azure Mobile Apps | Microsoft Docs
description: "Lär dig hur toouse Azure Mobile Apps toosend push-meddelanden tooyour Apache Cordova-app."
services: app-service\mobile
documentationcenter: javascript
manager: syntaxc4
editor: 
author: ggailey777
ms.assetid: 92c596a9-875c-4840-b0e1-69198817576f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 8e1b23d6145b446b6f01599337b677e2f2b31d7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-apache-cordova-app"></a><span data-ttu-id="779b3-103">Lägg till push-meddelanden tooyour Apache Cordova-app</span><span class="sxs-lookup"><span data-stu-id="779b3-103">Add push notifications tooyour Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="779b3-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="779b3-104">Overview</span></span>
<span data-ttu-id="779b3-105">I kursen får du lägga till push-meddelanden toohello [Apache Cordova Snabbstart]-projekt så att ett push-meddelande skickas toohello enheten varje gång en post infogas.</span><span class="sxs-lookup"><span data-stu-id="779b3-105">In this tutorial, you add push notifications toohello [Apache Cordova quick start] project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="779b3-106">Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="779b3-106">If you do not use hello downloaded quick start server project, you need hello push notification extension package.</span></span> <span data-ttu-id="779b3-107">Mer information finns i [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps][1].</span><span class="sxs-lookup"><span data-stu-id="779b3-107">For more information, see [Work with hello .NET backend server SDK for Azure Mobile Apps][1].</span></span>

## <span data-ttu-id="779b3-108"><a name="prerequisites"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="779b3-108"><a name="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="779b3-109">Den här kursen ingår en Apache Cordova-program som utvecklats med Visual Studio 2015 som körs på hello Google Android-emulatorn, en Android-enhet, en Windows-enhet och en iOS-enhet.</span><span class="sxs-lookup"><span data-stu-id="779b3-109">This tutorial covers an Apache Cordova application developed with Visual Studio 2015 that runs on hello Google Android Emulator, an Android device, a Windows device, and an iOS device.</span></span>

<span data-ttu-id="779b3-110">toocomplete den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="779b3-110">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="779b3-111">En dator med [Visual Studio Community 2015] [ 2] eller senare versioner.</span><span class="sxs-lookup"><span data-stu-id="779b3-111">A PC with [Visual Studio Community 2015][2] or later versions.</span></span>
* <span data-ttu-id="779b3-112">[Visual Studio Tools för Apache Cordova][4].</span><span class="sxs-lookup"><span data-stu-id="779b3-112">[Visual Studio Tools for Apache Cordova][4].</span></span>
* <span data-ttu-id="779b3-113">En [aktivt Azure-konto][3].</span><span class="sxs-lookup"><span data-stu-id="779b3-113">An [active Azure account][3].</span></span>
* <span data-ttu-id="779b3-114">En slutförd [Snabbstart för Apache Cordova] [ 5] projekt.</span><span class="sxs-lookup"><span data-stu-id="779b3-114">A completed [Apache Cordova quick start][5] project.</span></span>
* <span data-ttu-id="779b3-115">(Android) En [Google-konto] [ 6] med en verifierad e-postadress.</span><span class="sxs-lookup"><span data-stu-id="779b3-115">(Android) A [Google account][6] with a verified email address.</span></span>
* <span data-ttu-id="779b3-116">(iOS) En [Apple Developer Program medlemskap] [ 7] och en iOS-enhet (iOS-simulatorn stöder inte push).</span><span class="sxs-lookup"><span data-stu-id="779b3-116">(iOS) An [Apple Developer Program membership][7] and an iOS device (iOS Simulator does not support push).</span></span>
* <span data-ttu-id="779b3-117">(Windows) En [Windows Store-utvecklarkonto] [ 8] och en Windows 10-enhet.</span><span class="sxs-lookup"><span data-stu-id="779b3-117">(Windows) A [Windows Store Developer Account][8] and a Windows 10 device.</span></span>

## <span data-ttu-id="779b3-118"><a name="configure-hub"></a>Konfigurera en meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="779b3-118"><a name="configure-hub"></a>Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

<span data-ttu-id="779b3-119">[Se en video som visar stegen i det här avsnittet][9]</span><span class="sxs-lookup"><span data-stu-id="779b3-119">[Watch a video showing steps in this section][9]</span></span>

## <a name="update-hello-server-project"></a><span data-ttu-id="779b3-120">Uppdatera hello serverprojekt</span><span class="sxs-lookup"><span data-stu-id="779b3-120">Update hello server project</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <span data-ttu-id="779b3-121"><a name="add-push-to-app"></a>Ändra din Cordova-app</span><span class="sxs-lookup"><span data-stu-id="779b3-121"><a name="add-push-to-app"></a>Modify your Cordova app</span></span>
<span data-ttu-id="779b3-122">Kontrollera din Apache Cordova-app-projekt är klar toohandle push-meddelanden genom att installera hello Cordova push-pluginprogrammet plus alla plattformsspecifika push-tjänster.</span><span class="sxs-lookup"><span data-stu-id="779b3-122">Ensure your Apache Cordova app project is ready toohandle push notifications by installing hello Cordova push plugin plus any platform-specific push services.</span></span>

#### <a name="update-hello-cordova-version-in-your-project"></a><span data-ttu-id="779b3-123">Uppdatera hello Cordova-version i projektet.</span><span class="sxs-lookup"><span data-stu-id="779b3-123">Update hello Cordova version in your project.</span></span>
<span data-ttu-id="779b3-124">Om ditt projekt använder en tidigare version av Apache Cordova än v6.1.1, uppdatera hello klientprojekt.</span><span class="sxs-lookup"><span data-stu-id="779b3-124">If your project uses a version of Apache Cordova earlier than v6.1.1, update hello client project.</span></span> <span data-ttu-id="779b3-125">tooupdate hello projekt:</span><span class="sxs-lookup"><span data-stu-id="779b3-125">tooupdate hello project:</span></span>

* <span data-ttu-id="779b3-126">Högerklicka på `config.xml` tooopen hello configuration designer.</span><span class="sxs-lookup"><span data-stu-id="779b3-126">Right-click `config.xml` tooopen hello configuration designer.</span></span>
* <span data-ttu-id="779b3-127">Välj fliken för hello-plattformar.</span><span class="sxs-lookup"><span data-stu-id="779b3-127">Select hello Platforms tab.</span></span>
* <span data-ttu-id="779b3-128">Välj 6.1.1 i hello **Cordova CLI** textruta.</span><span class="sxs-lookup"><span data-stu-id="779b3-128">Choose 6.1.1 in hello **Cordova CLI** text box.</span></span>
* <span data-ttu-id="779b3-129">Välj **skapa**, sedan **skapa lösning** tooupdate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="779b3-129">Choose **Build**, then **Build Solution** tooupdate hello project.</span></span>

#### <a name="install-hello-push-plugin"></a><span data-ttu-id="779b3-130">Installera hello push plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="779b3-130">Install hello push plugin</span></span>
<span data-ttu-id="779b3-131">Apache Cordova-program hanterar inte internt enhet eller nätverket funktioner.</span><span class="sxs-lookup"><span data-stu-id="779b3-131">Apache Cordova applications do not natively handle device or network capabilities.</span></span>  <span data-ttu-id="779b3-132">Dessa funktioner som tillhandahålls av plugin-program som publicerats antingen på [npm] [ 10] eller på GitHub.</span><span class="sxs-lookup"><span data-stu-id="779b3-132">These capabilities are provided by plugins that are published either on [npm][10] or on GitHub.</span></span>  <span data-ttu-id="779b3-133">Hej `phonegap-plugin-push` plugin-programmet har använt toohandle nätverk push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="779b3-133">hello `phonegap-plugin-push` plugin is used toohandle network push notifications.</span></span>

<span data-ttu-id="779b3-134">Du kan installera push-plugin-programmet hello i något av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="779b3-134">You can install hello push plugin in one of these ways:</span></span>

<span data-ttu-id="779b3-135">**Från hello-kommandotolk:**</span><span class="sxs-lookup"><span data-stu-id="779b3-135">**From hello command-prompt:**</span></span>

<span data-ttu-id="779b3-136">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="779b3-136">Execute hello following command:</span></span>

    cordova plugin add phonegap-plugin-push

<span data-ttu-id="779b3-137">**Från Visual Studio:**</span><span class="sxs-lookup"><span data-stu-id="779b3-137">**From within Visual Studio:**</span></span>

1. <span data-ttu-id="779b3-138">I Solution Explorer öppnar du hello `config.xml` klickar du på filen **plugin-program** > **anpassad**väljer **Git** som installationskälla, ange `https://github.com/phonegap/phonegap-plugin-push`som hello källa.</span><span class="sxs-lookup"><span data-stu-id="779b3-138">In Solution Explorer, open hello `config.xml` file click **Plugins** > **Custom**, select **Git** as the  installation source, then enter `https://github.com/phonegap/phonegap-plugin-push` as hello source.</span></span>

   ![][img1]

2. <span data-ttu-id="779b3-139">Klicka på hello pilen Nästa toohello installationskälla.</span><span class="sxs-lookup"><span data-stu-id="779b3-139">Click hello arrow next toohello installation source.</span></span>
3. <span data-ttu-id="779b3-140">I **SENDER_ID**, om du redan har ett numeriskt projekt-ID för hello Google Developer Console-projekt, du kan lägga till den här.</span><span class="sxs-lookup"><span data-stu-id="779b3-140">In **SENDER_ID**, if you already have a numeric project ID for hello Google Developer Console project, you can add it here.</span></span> <span data-ttu-id="779b3-141">Annars kan du ange en platshållarvärde, till exempel 777777.</span><span class="sxs-lookup"><span data-stu-id="779b3-141">Otherwise, enter a placeholder value, like 777777.</span></span>  <span data-ttu-id="779b3-142">Om du utvecklar för Android, kan du uppdatera det här värdet i config.xml senare.</span><span class="sxs-lookup"><span data-stu-id="779b3-142">If you are targeting Android, you can update this value in config.xml later.</span></span>
4. <span data-ttu-id="779b3-143">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="779b3-143">Click **Add**.</span></span>

<span data-ttu-id="779b3-144">hello push-plugin-programmet har installerats.</span><span class="sxs-lookup"><span data-stu-id="779b3-144">hello push plugin is now installed.</span></span>

#### <a name="install-hello-device-plugin"></a><span data-ttu-id="779b3-145">Installera hello enheten plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="779b3-145">Install hello device plugin</span></span>
<span data-ttu-id="779b3-146">Följ hello samma procedur som du använde tooinstall hello push plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="779b3-146">Follow hello same procedure you used tooinstall hello push plugin.</span></span>  <span data-ttu-id="779b3-147">Lägg till enhet-plugin-programmet hello hello Core plugin-program listan (klicka på **plugin-program** > **Core** toofind den).</span><span class="sxs-lookup"><span data-stu-id="779b3-147">Add hello Device plugin from hello Core plugins list (click **Plugins** > **Core** toofind it).</span></span> <span data-ttu-id="779b3-148">Du behöver det här plugin-programmet tooobtain hello plattformsnamnet.</span><span class="sxs-lookup"><span data-stu-id="779b3-148">You need this plugin tooobtain hello platform name.</span></span>

#### <a name="register-your-device-on-application-start-up"></a><span data-ttu-id="779b3-149">Registrera din enhet på programmet uppstart</span><span class="sxs-lookup"><span data-stu-id="779b3-149">Register your device on application start-up</span></span>
<span data-ttu-id="779b3-150">Vi ta först minimal kod för Android.</span><span class="sxs-lookup"><span data-stu-id="779b3-150">Initially, we include some minimal code for Android.</span></span> <span data-ttu-id="779b3-151">Senare ändra hello app toorun på iOS- eller Windows 10.</span><span class="sxs-lookup"><span data-stu-id="779b3-151">Later, modify hello app toorun on iOS or Windows 10.</span></span>

1. <span data-ttu-id="779b3-152">Lägg till ett anrop för**registerForPushNotifications** under hello återanrop för hello inloggningen eller längst hello hello **onDeviceReady** metod:</span><span class="sxs-lookup"><span data-stu-id="779b3-152">Add a call too**registerForPushNotifications** during hello callback for hello login process, or at hello bottom of  hello **onDeviceReady** method:</span></span>

        // Login toohello service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh hello todoItems
                refreshDisplay();

                // Wire up hello UI Event Handler for hello Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added tooregister for push notifications.
                registerForPushNotifications();

            }, handleError);

    <span data-ttu-id="779b3-153">Det här exemplet visar anropar **registerForPushNotifications** när autentiseringen lyckas.</span><span class="sxs-lookup"><span data-stu-id="779b3-153">This example shows calling **registerForPushNotifications** after authentication succeeds.</span></span>  <span data-ttu-id="779b3-154">Du kan anropa `registerForPushNotifications()` så ofta som krävs.</span><span class="sxs-lookup"><span data-stu-id="779b3-154">You can call `registerForPushNotifications()` as often as is required.</span></span>

2. <span data-ttu-id="779b3-155">Lägga till nya hello **registerForPushNotifications** metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="779b3-155">Add hello new **registerForPushNotifications** method as follows:</span></span>

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle hello registration event.
        pushRegistration.on('registration', function (data) {
          // Get hello native platform of hello device.
          var platform = device.platform;
          // Get hello handle returned during registration.
          var handle = data.registrationId;
          // Set hello device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }
3. <span data-ttu-id="779b3-156">(Android) Ersätt i hello föregående kod, `Your_Project_ID` med hello numeriska projektet ID för din app från den [Google Developer Console][18].</span><span class="sxs-lookup"><span data-stu-id="779b3-156">(Android) In hello preceding code, replace `Your_Project_ID` with hello numeric project ID for your app from the  [Google Developer Console][18].</span></span>

## <a name="optional-configure-and-run-hello-app-on-android"></a><span data-ttu-id="779b3-157">(Valfritt) Konfigurera och köra hello app på Android</span><span class="sxs-lookup"><span data-stu-id="779b3-157">(Optional) Configure and run hello app on Android</span></span>
<span data-ttu-id="779b3-158">Slutför det här avsnittet tooenable push-meddelanden för Android.</span><span class="sxs-lookup"><span data-stu-id="779b3-158">Complete this section tooenable push notifications for Android.</span></span>

#### <span data-ttu-id="779b3-159"><a name="enable-gcm"></a>Aktivera Firebase Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="779b3-159"><a name="enable-gcm"></a>Enable Firebase Cloud Messaging</span></span>
<span data-ttu-id="779b3-160">Eftersom vi utvecklar hello Google Android-plattformen först, måste du aktivera Firebase Cloud Messaging.</span><span class="sxs-lookup"><span data-stu-id="779b3-160">Since we are targeting hello Google Android platform initially, you must enable Firebase Cloud Messaging.</span></span>

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <span data-ttu-id="779b3-161"><a name="configure-backend"></a>Konfigurera hello Mobilapp backend toosend push-begäranden som använder FCM</span><span class="sxs-lookup"><span data-stu-id="779b3-161"><a name="configure-backend"></a>Configure hello Mobile App backend toosend push requests using FCM</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a><span data-ttu-id="779b3-162">Konfigurera din Cordova-app för Android</span><span class="sxs-lookup"><span data-stu-id="779b3-162">Configure your Cordova app for Android</span></span>
<span data-ttu-id="779b3-163">Öppna config.xml i Cordova-app och Ersätt `Your_Project_ID` med hello numeriska projektet ID för din app från hello [Google Developer Console][18].</span><span class="sxs-lookup"><span data-stu-id="779b3-163">In your Cordova app, open config.xml and replace `Your_Project_ID` with hello numeric project ID for your app from hello [Google Developer Console][18].</span></span>

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

<span data-ttu-id="779b3-164">Öppna index.js och uppdatera hello kod toouse numeriska projekt-ID.</span><span class="sxs-lookup"><span data-stu-id="779b3-164">Open index.js and update hello code toouse your numeric project ID.</span></span>

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

#### <span data-ttu-id="779b3-165"><a name="configure-device"></a>Konfigurera din Android-enhet för USB-felsökning</span><span class="sxs-lookup"><span data-stu-id="779b3-165"><a name="configure-device"></a>Configure your Android device for USB debugging</span></span>
<span data-ttu-id="779b3-166">Innan du kan distribuera ditt program tooyour Android-enhet behöver du tooenable USB-felsökning.</span><span class="sxs-lookup"><span data-stu-id="779b3-166">Before you can deploy your application tooyour Android Device, you need tooenable USB Debugging.</span></span>  <span data-ttu-id="779b3-167">Utför följande steg på din Android-telefon:</span><span class="sxs-lookup"><span data-stu-id="779b3-167">Perform the following steps on your Android phone:</span></span>

1. <span data-ttu-id="779b3-168">Gå för**inställningar** > **om telefonen**, tryck på hello **Build-nummer** förrän utvecklarläge har aktiverats (ungefär sju gånger).</span><span class="sxs-lookup"><span data-stu-id="779b3-168">Go too**Settings** > **About phone**, then tap hello **Build number** until developer mode is enabled  (about seven times).</span></span>
2. <span data-ttu-id="779b3-169">Tillbaka i **inställningar** > **utvecklaralternativ** aktivera **USB-felsökning**, Anslut din Android-telefon tooyour development dator med en USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="779b3-169">Back in **Settings** > **Developer Options** enable **USB debugging**, then connect your Android phone  tooyour development PC with a USB Cable.</span></span>

<span data-ttu-id="779b3-170">Vi testade detta med hjälp av en Google Nexus 5 X-enhet som kör Android 6.0 (Marshmallow).</span><span class="sxs-lookup"><span data-stu-id="779b3-170">We tested this using a Google Nexus 5X device running Android 6.0 (Marshmallow).</span></span>  <span data-ttu-id="779b3-171">Hello tekniker är dock gemensamma för alla moderna Android-versionen.</span><span class="sxs-lookup"><span data-stu-id="779b3-171">However, hello techniques are common across any modern Android release.</span></span>

#### <a name="install-google-play-services"></a><span data-ttu-id="779b3-172">Installera Google Play-tjänster</span><span class="sxs-lookup"><span data-stu-id="779b3-172">Install Google Play Services</span></span>
<span data-ttu-id="779b3-173">push-plugin-programmet hello förlitar sig på Android Google Play-tjänster för push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="779b3-173">hello push plugin relies on Android Google Play Services for push notifications.</span></span>

1. <span data-ttu-id="779b3-174">I Visual Studio klickar du på **verktyg** > **Android** > **Android SDK Manager**, expandera hello **tillägg** mappen och kontrollera hello rutan toomake att var och en av följande SDK hello har installerats.</span><span class="sxs-lookup"><span data-stu-id="779b3-174">In Visual Studio, click **Tools** > **Android** > **Android SDK Manager**, expand hello **Extras** folder and  check hello box toomake sure that each of hello following SDKs is installed.</span></span>

   * <span data-ttu-id="779b3-175">Android 2.3 eller högre</span><span class="sxs-lookup"><span data-stu-id="779b3-175">Android 2.3 or higher</span></span>
   * <span data-ttu-id="779b3-176">Google databasen revision 27 eller högre</span><span class="sxs-lookup"><span data-stu-id="779b3-176">Google Repository revision 27 or higher</span></span>
   * <span data-ttu-id="779b3-177">Google Play Services 9.0.2 eller högre</span><span class="sxs-lookup"><span data-stu-id="779b3-177">Google Play Services 9.0.2 or higher</span></span>

2. <span data-ttu-id="779b3-178">Klicka på **installationspaket** och vänta tills hello installation toocomplete.</span><span class="sxs-lookup"><span data-stu-id="779b3-178">Click **Install Packages** and wait for hello installation toocomplete.</span></span>

<span data-ttu-id="779b3-179">hello aktuella krävs bibliotek visas i hello [phonegap-plugin-push-installation av dokumentationen][19].</span><span class="sxs-lookup"><span data-stu-id="779b3-179">hello current required libraries are listed in hello [phonegap-plugin-push installation documentation][19].</span></span>

#### <a name="test-push-notifications-in-hello-app-on-android"></a><span data-ttu-id="779b3-180">Testa push-meddelanden i hello app på Android</span><span class="sxs-lookup"><span data-stu-id="779b3-180">Test push notifications in hello app on Android</span></span>
<span data-ttu-id="779b3-181">Nu kan du testa push-meddelanden genom att köra hello appen och infoga objekt i hello TodoItem-tabell.</span><span class="sxs-lookup"><span data-stu-id="779b3-181">You can now test push notifications by running hello app and inserting items in hello TodoItem table.</span></span> <span data-ttu-id="779b3-182">Du kan testa från hello samma enhet eller från en annan enhet som du använder hello samma backend.</span><span class="sxs-lookup"><span data-stu-id="779b3-182">You can test from hello same device or from a second device, as long as you are using hello same backend.</span></span> <span data-ttu-id="779b3-183">Testa din Cordova-app på Android hello-plattformen i något av följande sätt hello:</span><span class="sxs-lookup"><span data-stu-id="779b3-183">Test your Cordova app on hello Android platform in one of hello following ways:</span></span>

* <span data-ttu-id="779b3-184">**På en fysisk enhet:** bifoga Android-enhet tooyour utvecklingsdator med en USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="779b3-184">**On a physical device:** Attach your Android device tooyour development computer with a USB cable.</span></span>  <span data-ttu-id="779b3-185">I stället för **Google Android-emulatorn**väljer **enhet**.</span><span class="sxs-lookup"><span data-stu-id="779b3-185">Instead of **Google Android Emulator**, select **Device**.</span></span> <span data-ttu-id="779b3-186">Visual Studio distribuerar hello programmet toohello enheten och kör sedan programmet hello.</span><span class="sxs-lookup"><span data-stu-id="779b3-186">Visual Studio deploys hello application toohello device and then runs hello application.</span></span>  <span data-ttu-id="779b3-187">Du kan sedan interagera med hello programmet på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="779b3-187">You can then interact with hello application on hello device.</span></span>

  <span data-ttu-id="779b3-188">Förbättra din utvecklingsmetod.</span><span class="sxs-lookup"><span data-stu-id="779b3-188">Improve your development experience.</span></span>  <span data-ttu-id="779b3-189">Dela program som skärm [Mobizen] [ 20] kan hjälpa dig att utveckla ett Android-program.</span><span class="sxs-lookup"><span data-stu-id="779b3-189">Screen sharing applications such as [Mobizen][20] can assist you in developing an Android application.</span></span>  <span data-ttu-id="779b3-190">Mobizen projekt webbläsaren Android skärmen tooa på din dator.</span><span class="sxs-lookup"><span data-stu-id="779b3-190">Mobizen projects your Android screen tooa web browser on your PC.</span></span>

* <span data-ttu-id="779b3-191">**På en Android-emulatorn:** finns ytterligare konfigurationssteg som krävs vid körning på en emulator.</span><span class="sxs-lookup"><span data-stu-id="779b3-191">**On an Android emulator:** There are additional configuration steps required when running on an emulator.</span></span>

    <span data-ttu-id="779b3-192">Kontrollera att du distribuerar tooa virtuell enhet som har Google APIs som hello mål, enligt hello Android Virtual Device (AVD)-hanteraren.</span><span class="sxs-lookup"><span data-stu-id="779b3-192">Make sure you are deploying tooa virtual device that has Google APIs set as hello target, as shown in hello Android Virtual Device (AVD) manager.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    <span data-ttu-id="779b3-193">Om du vill toouse snabbare x86 emulatorn du [installera drivrutinen för hello HAXM] [ 11] och konfigurera hello emulatorn toouse den.</span><span class="sxs-lookup"><span data-stu-id="779b3-193">If you want toouse a faster x86 emulator, you [install hello HAXM driver][11] and configure hello emulator toouse it.</span></span>

    <span data-ttu-id="779b3-194">Lägg till en Google-konto toohello Android-enhet genom att klicka på **appar** > **inställningar** > **Lägg till konto**, följ sedan anvisningarna för hello.</span><span class="sxs-lookup"><span data-stu-id="779b3-194">Add a Google account toohello Android device by clicking **Apps** > **Settings** > **Add account**, then follow hello prompts.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    <span data-ttu-id="779b3-195">Kör hello todolist appen som innan och infoga ett nytt todo-objekt.</span><span class="sxs-lookup"><span data-stu-id="779b3-195">Run hello todolist app as before and insert a new todo item.</span></span> <span data-ttu-id="779b3-196">Den här tiden kan visas en meddelandeikonen i meddelandefältet hello.</span><span class="sxs-lookup"><span data-stu-id="779b3-196">This time, a notification icon is displayed in hello notification area.</span></span> <span data-ttu-id="779b3-197">Du kan öppna hello lådan tooview hello fullständig meddelandetext av hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="779b3-197">You can open hello notification drawer tooview hello full text of hello notification.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a><span data-ttu-id="779b3-198">(Valfritt) Konfigurera och köra på iOS</span><span class="sxs-lookup"><span data-stu-id="779b3-198">(Optional) Configure and run on iOS</span></span>
<span data-ttu-id="779b3-199">Det här avsnittet handlar om att köra hello Cordova-projekt på iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="779b3-199">This section is for running hello Cordova project on iOS devices.</span></span> <span data-ttu-id="779b3-200">Om du inte arbetar med iOS-enheter, kan du hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="779b3-200">If you are not working with iOS devices, you can skip this section.</span></span>

#### <a name="install-and-run-hello-ios-remote-build-agent-on-a-mac-or-cloud-service"></a><span data-ttu-id="779b3-201">Installera och köra hello iOS remote skapa agenten på en Mac- eller molnet</span><span class="sxs-lookup"><span data-stu-id="779b3-201">Install and run hello iOS remote build agent on a Mac or cloud service</span></span>
<span data-ttu-id="779b3-202">Innan du kan köra en Cordova-app på iOS med Visual Studio, gå igenom hello steg i hello [iOS inställningsguiden] [ 12] tooinstall och kör hello remote skapa agenten.</span><span class="sxs-lookup"><span data-stu-id="779b3-202">Before you can run a Cordova app on iOS using Visual Studio, go through hello steps in hello [iOS Setup Guide][12] tooinstall and run hello remote build agent.</span></span>

<span data-ttu-id="779b3-203">Kontrollera att du kan skapa hello appen för iOS.</span><span class="sxs-lookup"><span data-stu-id="779b3-203">Make sure you can build hello app for iOS.</span></span> <span data-ttu-id="779b3-204">hello är stegen i guiden hello obligatoriska toobuild för iOS från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="779b3-204">hello steps in hello setup guide are required toobuild for iOS from Visual Studio.</span></span> <span data-ttu-id="779b3-205">Om du inte har en Mac, kan du skapa för iOS med hello remote build-agenten på en tjänst som MacInCloud.</span><span class="sxs-lookup"><span data-stu-id="779b3-205">If you do not have a Mac, you can build for iOS using hello remote build agent on a service like MacInCloud.</span></span> <span data-ttu-id="779b3-206">Mer information finns i [kör iOS-app i hello molnet][21].</span><span class="sxs-lookup"><span data-stu-id="779b3-206">For more info, see [Run your iOS app in hello cloud][21].</span></span>

> [!NOTE]
> <span data-ttu-id="779b3-207">XCode 7 eller senare är obligatoriska toouse hello push-plugin-programmet på iOS.</span><span class="sxs-lookup"><span data-stu-id="779b3-207">XCode 7 or greater is required toouse hello push plugin on iOS.</span></span>

#### <a name="find-hello-id-toouse-as-your-app-id"></a><span data-ttu-id="779b3-208">Hitta hello ID toouse som App-ID</span><span class="sxs-lookup"><span data-stu-id="779b3-208">Find hello ID toouse as your App ID</span></span>
<span data-ttu-id="779b3-209">Innan du registrerar appen för push-meddelanden, öppnar config.xml i Cordova-app, hitta hello `id` attributvärdet i hello widget element och kopiera den för senare användning.</span><span class="sxs-lookup"><span data-stu-id="779b3-209">Before you register your app for push notifications, open config.xml in your Cordova app, find hello `id` attribute value in hello widget element, and copy it for later use.</span></span> <span data-ttu-id="779b3-210">Hello-ID i hello följande XML, är `io.cordova.myapp7777777`.</span><span class="sxs-lookup"><span data-stu-id="779b3-210">In hello following XML, hello ID is `io.cordova.myapp7777777`.</span></span>

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
          version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

<span data-ttu-id="779b3-211">Använd den här identifieraren senare, när du skapar ett App-ID på Apple developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="779b3-211">Later, use this identifier when you create an App ID on Apple's developer portal.</span></span> <span data-ttu-id="779b3-212">Om du skapar ett annat App-ID på hello developer-portalen måste du utföra några extra steg senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="779b3-212">If you create a different App ID on hello developer portal, you must take a few extra steps later in this tutorial.</span></span> <span data-ttu-id="779b3-213">hello-ID i widget-elementet måste matcha hello App-ID på hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="779b3-213">hello ID in the widget element must match hello App ID on hello developer portal.</span></span>

#### <a name="register-hello-app-for-push-notifications-on-apples-developer-portal"></a><span data-ttu-id="779b3-214">Registrera hello app för push-meddelanden på Apple developer-portalen</span><span class="sxs-lookup"><span data-stu-id="779b3-214">Register hello app for push notifications on Apple's developer portal</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[<span data-ttu-id="779b3-215">Titta på en video som visar liknande steg</span><span class="sxs-lookup"><span data-stu-id="779b3-215">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-toosend-push-notifications"></a><span data-ttu-id="779b3-216">Konfigurera Azure toosend push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="779b3-216">Configure Azure toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a><span data-ttu-id="779b3-217">Kontrollera att din App-ID som matchar din Cordova-app</span><span class="sxs-lookup"><span data-stu-id="779b3-217">Verify that your App ID matches your Cordova app</span></span>
<span data-ttu-id="779b3-218">Om hello App-ID som du redan skapat i Apple Developer kontot matchar hello-ID för hello widget element i config.xml, kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="779b3-218">If hello App ID you created in your Apple Developer Account already matches hello ID of hello widget element in config.xml, you can skip this step.</span></span> <span data-ttu-id="779b3-219">Men om hello-ID: N inte matchar ta hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="779b3-219">However, if hello IDs don't match, take hello following steps:</span></span>

1. <span data-ttu-id="779b3-220">Ta bort hello plattformar mappen i projektet.</span><span class="sxs-lookup"><span data-stu-id="779b3-220">Delete hello platforms folder from your project.</span></span>
2. <span data-ttu-id="779b3-221">Ta bort hello plugin-program-mappen i projektet.</span><span class="sxs-lookup"><span data-stu-id="779b3-221">Delete hello plugins folder from your project.</span></span>
3. <span data-ttu-id="779b3-222">Ta bort hello node_modules mappen i projektet.</span><span class="sxs-lookup"><span data-stu-id="779b3-222">Delete hello node_modules folder from your project.</span></span>
4. <span data-ttu-id="779b3-223">Uppdatera hello ID-attribut för hello widget element i config.xml toouse hello App-ID som du skapade i Apple Developer-konto.</span><span class="sxs-lookup"><span data-stu-id="779b3-223">Update hello id attribute of hello widget element in config.xml toouse hello App ID that you created in your  Apple Developer Account.</span></span>
5. <span data-ttu-id="779b3-224">Återskapa projektet.</span><span class="sxs-lookup"><span data-stu-id="779b3-224">Rebuild your project.</span></span>

##### <a name="test-push-notifications-in-your-ios-app"></a><span data-ttu-id="779b3-225">Testa push-meddelanden i iOS-app</span><span class="sxs-lookup"><span data-stu-id="779b3-225">Test push notifications in your iOS app</span></span>
1. <span data-ttu-id="779b3-226">I Visual Studio, se till att **iOS** väljs som mål för distribution av hello och välj sedan **enhet** toorun på den anslutna iOS-enheten.</span><span class="sxs-lookup"><span data-stu-id="779b3-226">In Visual Studio, make sure that **iOS** is selected as hello deployment target, and then choose **Device** toorun on your connected iOS device.</span></span>

    <span data-ttu-id="779b3-227">Du kan köra på en iOS-enheten ansluten tooyour PC med iTunes.</span><span class="sxs-lookup"><span data-stu-id="779b3-227">You can run on an iOS device connected tooyour PC using iTunes.</span></span> <span data-ttu-id="779b3-228">hello iOS-simulatorn stöder inte push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="779b3-228">hello iOS Simulator does not support push notifications.</span></span>

2. <span data-ttu-id="779b3-229">Tryck på hello **kör** knappen eller **F5** Visual Studio toobuild hello projektet och starta hello app i en iOS-enhet och klicka sedan på **OK** tooaccept push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="779b3-229">Press hello **Run** button or **F5** in Visual Studio toobuild hello project and start hello app in an iOS  device, then click **OK** tooaccept push notifications.</span></span>

   > [!NOTE]
   > <span data-ttu-id="779b3-230">hello app begär bekräftelse för push-meddelanden under hello kör först.</span><span class="sxs-lookup"><span data-stu-id="779b3-230">hello app requests confirmation for push notifications during hello first run.</span></span>

3. <span data-ttu-id="779b3-231">Skriv en aktivitet i hello appen och klicka sedan på hello plus -ikonen.</span><span class="sxs-lookup"><span data-stu-id="779b3-231">In hello app, type a task, and then click hello plus (+) icon.</span></span>
4. <span data-ttu-id="779b3-232">Kontrollera att ett meddelande tas emot och sedan klicka på OK toodismiss hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="779b3-232">Verify that a notification is received, then click OK toodismiss hello notification.</span></span>

## <a name="optional-configure-and-run-on-windows"></a><span data-ttu-id="779b3-233">(Valfritt) Konfigurera och köra i Windows</span><span class="sxs-lookup"><span data-stu-id="779b3-233">(Optional) Configure and run on Windows</span></span>
<span data-ttu-id="779b3-234">Det här avsnittet handlar om att köra hello Apache Cordova-app-projekt på Windows 10-enheter (hello PhoneGap push plugin stöds på Windows 10).</span><span class="sxs-lookup"><span data-stu-id="779b3-234">This section is for running hello Apache Cordova app project on Windows 10 devices (hello PhoneGap push plugin is supported on Windows 10).</span></span> <span data-ttu-id="779b3-235">Om du inte arbetar med Windows-enheter, kan du hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="779b3-235">If you are not working with Windows devices, you can skip this section.</span></span>

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a><span data-ttu-id="779b3-236">Registrera din Windows-app för push-meddelanden med WNS</span><span class="sxs-lookup"><span data-stu-id="779b3-236">Register your Windows app for push notifications with WNS</span></span>
<span data-ttu-id="779b3-237">toouse hello Store alternativ i Visual Studio, Välj ett mål för Windows hello lösning plattformar listan som **Windows x64** eller **Windows x86** (undvika **Windows Platform** för push-meddelanden).</span><span class="sxs-lookup"><span data-stu-id="779b3-237">toouse hello Store options in Visual Studio, select a Windows target from hello Solution Platforms list, like **Windows-x64** or **Windows-x86** (avoid **Windows-AnyCPU** for push notifications).</span></span>

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

<span data-ttu-id="779b3-238">[Se en video som visar liknande steg][13]</span><span class="sxs-lookup"><span data-stu-id="779b3-238">[Watch a video showing similar steps][13]</span></span>

#### <a name="configure-hello-notification-hub-for-wns"></a><span data-ttu-id="779b3-239">Konfigurera hello meddelandehubb för WNS</span><span class="sxs-lookup"><span data-stu-id="779b3-239">Configure hello notification hub for WNS</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-toosupport-windows-push-notifications"></a><span data-ttu-id="779b3-240">Konfigurera din Cordova-app toosupport Windows push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="779b3-240">Configure your Cordova app toosupport Windows push notifications</span></span>
<span data-ttu-id="779b3-241">Öppna hello configuration designer (Högerklicka på config.xml och välj **Vydesigner**), Välj hello **Windows** , och välj **Windows 10** under **Windows använder Version**.</span><span class="sxs-lookup"><span data-stu-id="779b3-241">Open hello configuration designer (right-click on config.xml and select **View Designer**), select hello **Windows** tab, and choose **Windows 10** under **Windows Target Version**.</span></span>

<span data-ttu-id="779b3-242">toosupport push-meddelanden i din standard (debug) versioner, öppna build.json fil.</span><span class="sxs-lookup"><span data-stu-id="779b3-242">toosupport push notifications in your default (debug) builds, open build.json file.</span></span> <span data-ttu-id="779b3-243">Kopiera ”version” configuration tooyour debug-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="779b3-243">Copy the "release" configuration tooyour debug configuration.</span></span>

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

<span data-ttu-id="779b3-244">Efter uppdateringen hello bör hello build.json innehålla hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="779b3-244">After hello update, hello build.json should contain hello following code:</span></span>

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

<span data-ttu-id="779b3-245">Skapa hello program och kontrollera att du har några fel.</span><span class="sxs-lookup"><span data-stu-id="779b3-245">Build hello app and verify that you have no errors.</span></span> <span data-ttu-id="779b3-246">Klientappen bör nu registrera för hello meddelanden från hello mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="779b3-246">Your client app should now register for hello notifications from hello Mobile App backend.</span></span> <span data-ttu-id="779b3-247">Upprepa det här avsnittet för varje Windows-projekt i din lösning.</span><span class="sxs-lookup"><span data-stu-id="779b3-247">Repeat this section for every Windows project in your solution.</span></span>

#### <a name="test-push-notifications-in-your-windows-app"></a><span data-ttu-id="779b3-248">Testa push-meddelanden i Windows-appen</span><span class="sxs-lookup"><span data-stu-id="779b3-248">Test push notifications in your Windows app</span></span>
<span data-ttu-id="779b3-249">I Visual Studio, se till att en Windows-plattform som är markerad som mål för distribution av hello, **Windows x64** eller **Windows x86**.</span><span class="sxs-lookup"><span data-stu-id="779b3-249">In Visual Studio, make sure that a Windows platform is selected as hello deployment target, such as **Windows-x64** or **Windows-x86**.</span></span> <span data-ttu-id="779b3-250">toorun hello-appen på en Windows 10-dator som värd för Visual Studio väljer **lokal dator**.</span><span class="sxs-lookup"><span data-stu-id="779b3-250">toorun hello app on a Windows 10 PC hosting Visual Studio, choose **Local Machine**.</span></span>

<span data-ttu-id="779b3-251">Tryck på hello kör knappen toobuild hello projektet och starta hello app.</span><span class="sxs-lookup"><span data-stu-id="779b3-251">Press hello Run button toobuild hello project and start hello app.</span></span>

<span data-ttu-id="779b3-252">Skriv ett namn för en ny todoitem i hello app och klicka sedan på hello plus -ikonen tooadd den.</span><span class="sxs-lookup"><span data-stu-id="779b3-252">In hello app, type a name for a new todoitem, and then click hello plus (+) icon tooadd it.</span></span>

<span data-ttu-id="779b3-253">Kontrollera att ett meddelande tas emot när hello-objektet har lagts till.</span><span class="sxs-lookup"><span data-stu-id="779b3-253">Verify that a notification is received when hello item is added.</span></span>

## <span data-ttu-id="779b3-254"><a name="next-steps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="779b3-254"><a name="next-steps"></a>Next Steps</span></span>
* <span data-ttu-id="779b3-255">Läs mer om [Meddelandehubbar] [ 17] toolearn om push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="779b3-255">Read about [Notification Hubs][17] toolearn about push notifications.</span></span>
* <span data-ttu-id="779b3-256">Om du inte redan har gjort det, fortsätter hello självstudiekursen [att lägga till autentisering] [ 14] tooyour Apache Cordova-app.</span><span class="sxs-lookup"><span data-stu-id="779b3-256">If you have not already done so, continue hello tutorial by [Adding Authentication][14] tooyour Apache Cordova app.</span></span>

<span data-ttu-id="779b3-257">Lär dig hur toouse hello SDK: er.</span><span class="sxs-lookup"><span data-stu-id="779b3-257">Learn how toouse hello SDKs.</span></span>

* <span data-ttu-id="779b3-258">[Apache Cordova-SDK][15]</span><span class="sxs-lookup"><span data-stu-id="779b3-258">[Apache Cordova SDK][15]</span></span>
* <span data-ttu-id="779b3-259">[ASP.NET Server-SDK][1]</span><span class="sxs-lookup"><span data-stu-id="779b3-259">[ASP.NET Server SDK][1]</span></span>
* <span data-ttu-id="779b3-260">[Node.js Server-SDK][16]</span><span class="sxs-lookup"><span data-stu-id="779b3-260">[Node.js Server SDK][16]</span></span>

<!-- Images -->
[img1]: ./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png

<!-- URLs -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: http://www.visualstudio.com/
[3]: https://azure.microsoft.com/pricing/free-trial/
[4]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[5]: app-service-mobile-cordova-get-started.md
[6]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[7]: https://developer.apple.com/programs/
[8]: https://developer.microsoft.com/en-us/store/register
[9]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub
[10]: https://www.npmjs.com/
[11]: https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM
[12]: http://taco.visualstudio.com/en-us/docs/ios-guide/
[13]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push
[14]: app-service-mobile-cordova-get-started-users.md
[15]: app-service-mobile-cordova-how-to-use-client-library.md
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[17]: ../notification-hubs/notification-hubs-push-notification-overview.md
[18]: https://console.developers.google.com/home/dashboard
[19]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[20]: https://www.mobizen.com/
[21]: http://taco.visualstudio.com/en-us/docs/build_ios_cloud/
