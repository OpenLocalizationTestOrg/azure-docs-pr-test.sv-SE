---
title: "Lägg till Push-meddelanden i Apache Cordova-App med Azure Mobilappar | Microsoft Docs"
description: "Lär dig hur du använder Azure Mobile Apps för att skicka push-meddelanden till din Apache Cordova-app."
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
ms.openlocfilehash: dc3cab0a6a8b4a56ab0fba1a02e5bba9d0ed1b1f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-apache-cordova-app"></a><span data-ttu-id="06242-103">Lägg till push-meddelanden i din Apache Cordova-app</span><span class="sxs-lookup"><span data-stu-id="06242-103">Add push notifications to your Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="06242-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="06242-104">Overview</span></span>
<span data-ttu-id="06242-105">I kursen får du lägga till push-meddelanden i [Apache Cordova Snabbstart]-projektet så att ett push-meddelande skickas till enheten varje gång en post infogas.</span><span class="sxs-lookup"><span data-stu-id="06242-105">In this tutorial, you add push notifications to the [Apache Cordova quick start] project so that a push notification is sent to the device every time a record is inserted.</span></span>

<span data-ttu-id="06242-106">Om du inte använder hämtade Snabbstart serverprojekt måste push notification extension-paketet.</span><span class="sxs-lookup"><span data-stu-id="06242-106">If you do not use the downloaded quick start server project, you need the push notification extension package.</span></span> <span data-ttu-id="06242-107">Mer information finns i [arbeta med serverdelen .NET SDK för Azure Mobile Apps][1].</span><span class="sxs-lookup"><span data-stu-id="06242-107">For more information, see [Work with the .NET backend server SDK for Azure Mobile Apps][1].</span></span>

## <span data-ttu-id="06242-108"><a name="prerequisites"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="06242-108"><a name="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="06242-109">Den här kursen ingår en Apache Cordova-program som utvecklats med Visual Studio 2015 som körs på Google Android-emulatorn, en Android-enhet, en Windows-enhet och en iOS-enhet.</span><span class="sxs-lookup"><span data-stu-id="06242-109">This tutorial covers an Apache Cordova application developed with Visual Studio 2015 that runs on the Google Android Emulator, an Android device, a Windows device, and an iOS device.</span></span>

<span data-ttu-id="06242-110">För att slutföra den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="06242-110">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="06242-111">En dator med [Visual Studio Community 2015] [ 2] eller senare versioner.</span><span class="sxs-lookup"><span data-stu-id="06242-111">A PC with [Visual Studio Community 2015][2] or later versions.</span></span>
* <span data-ttu-id="06242-112">[Visual Studio Tools för Apache Cordova][4].</span><span class="sxs-lookup"><span data-stu-id="06242-112">[Visual Studio Tools for Apache Cordova][4].</span></span>
* <span data-ttu-id="06242-113">En [aktivt Azure-konto][3].</span><span class="sxs-lookup"><span data-stu-id="06242-113">An [active Azure account][3].</span></span>
* <span data-ttu-id="06242-114">En slutförd [Snabbstart för Apache Cordova] [ 5] projekt.</span><span class="sxs-lookup"><span data-stu-id="06242-114">A completed [Apache Cordova quick start][5] project.</span></span>
* <span data-ttu-id="06242-115">(Android) En [Google-konto] [ 6] med en verifierad e-postadress.</span><span class="sxs-lookup"><span data-stu-id="06242-115">(Android) A [Google account][6] with a verified email address.</span></span>
* <span data-ttu-id="06242-116">(iOS) En [Apple Developer Program medlemskap] [ 7] och en iOS-enhet (iOS-simulatorn stöder inte push).</span><span class="sxs-lookup"><span data-stu-id="06242-116">(iOS) An [Apple Developer Program membership][7] and an iOS device (iOS Simulator does not support push).</span></span>
* <span data-ttu-id="06242-117">(Windows) En [Windows Store-utvecklarkonto] [ 8] och en Windows 10-enhet.</span><span class="sxs-lookup"><span data-stu-id="06242-117">(Windows) A [Windows Store Developer Account][8] and a Windows 10 device.</span></span>

## <span data-ttu-id="06242-118"><a name="configure-hub"></a>Konfigurera en meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="06242-118"><a name="configure-hub"></a>Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

<span data-ttu-id="06242-119">[Se en video som visar stegen i det här avsnittet][9]</span><span class="sxs-lookup"><span data-stu-id="06242-119">[Watch a video showing steps in this section][9]</span></span>

## <a name="update-the-server-project"></a><span data-ttu-id="06242-120">Uppdatera serverprojektet</span><span class="sxs-lookup"><span data-stu-id="06242-120">Update the server project</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <span data-ttu-id="06242-121"><a name="add-push-to-app"></a>Ändra din Cordova-app</span><span class="sxs-lookup"><span data-stu-id="06242-121"><a name="add-push-to-app"></a>Modify your Cordova app</span></span>
<span data-ttu-id="06242-122">Se till att projektet Apache Cordova-app är redo att hantera push-meddelanden genom att installera pluginprogrammet Cordova push plus alla plattformsspecifika push-tjänster.</span><span class="sxs-lookup"><span data-stu-id="06242-122">Ensure your Apache Cordova app project is ready to handle push notifications by installing the Cordova push plugin plus any platform-specific push services.</span></span>

#### <a name="update-the-cordova-version-in-your-project"></a><span data-ttu-id="06242-123">Uppdatera Cordova-version i projektet.</span><span class="sxs-lookup"><span data-stu-id="06242-123">Update the Cordova version in your project.</span></span>
<span data-ttu-id="06242-124">Om ditt projekt använder en tidigare version av Apache Cordova än v6.1.1, uppdatera klientprojektet.</span><span class="sxs-lookup"><span data-stu-id="06242-124">If your project uses a version of Apache Cordova earlier than v6.1.1, update the client project.</span></span> <span data-ttu-id="06242-125">Så här uppdaterar projektet:</span><span class="sxs-lookup"><span data-stu-id="06242-125">To update the project:</span></span>

* <span data-ttu-id="06242-126">Högerklicka på `config.xml` att öppna configuration designer.</span><span class="sxs-lookup"><span data-stu-id="06242-126">Right-click `config.xml` to open the configuration designer.</span></span>
* <span data-ttu-id="06242-127">Välj fliken plattformar.</span><span class="sxs-lookup"><span data-stu-id="06242-127">Select the Platforms tab.</span></span>
* <span data-ttu-id="06242-128">Välj 6.1.1 i den **Cordova CLI** textruta.</span><span class="sxs-lookup"><span data-stu-id="06242-128">Choose 6.1.1 in the **Cordova CLI** text box.</span></span>
* <span data-ttu-id="06242-129">Välj **skapa**, sedan **skapa lösning** att uppdatera projektet.</span><span class="sxs-lookup"><span data-stu-id="06242-129">Choose **Build**, then **Build Solution** to update the project.</span></span>

#### <a name="install-the-push-plugin"></a><span data-ttu-id="06242-130">Installera push plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="06242-130">Install the push plugin</span></span>
<span data-ttu-id="06242-131">Apache Cordova-program hanterar inte internt enhet eller nätverket funktioner.</span><span class="sxs-lookup"><span data-stu-id="06242-131">Apache Cordova applications do not natively handle device or network capabilities.</span></span>  <span data-ttu-id="06242-132">Dessa funktioner som tillhandahålls av plugin-program som publicerats antingen på [npm] [ 10] eller på GitHub.</span><span class="sxs-lookup"><span data-stu-id="06242-132">These capabilities are provided by plugins that are published either on [npm][10] or on GitHub.</span></span>  <span data-ttu-id="06242-133">Den `phonegap-plugin-push` plugin-programmet används för att hantera nätverk push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="06242-133">The `phonegap-plugin-push` plugin is used to handle network push notifications.</span></span>

<span data-ttu-id="06242-134">Du kan installera pluginprogrammet push i något av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="06242-134">You can install the push plugin in one of these ways:</span></span>

<span data-ttu-id="06242-135">**Från Kommandotolken:**</span><span class="sxs-lookup"><span data-stu-id="06242-135">**From the command-prompt:**</span></span>

<span data-ttu-id="06242-136">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="06242-136">Execute the following command:</span></span>

    cordova plugin add phonegap-plugin-push

<span data-ttu-id="06242-137">**Från Visual Studio:**</span><span class="sxs-lookup"><span data-stu-id="06242-137">**From within Visual Studio:**</span></span>

1. <span data-ttu-id="06242-138">I Solution Explorer öppnar den `config.xml` klickar du på filen **plugin-program** > **anpassad**väljer **Git** som installationskälla, ange `https://github.com/phonegap/phonegap-plugin-push` som källa.</span><span class="sxs-lookup"><span data-stu-id="06242-138">In Solution Explorer, open the `config.xml` file click **Plugins** > **Custom**, select **Git** as the  installation source, then enter `https://github.com/phonegap/phonegap-plugin-push` as the source.</span></span>

   ![][img1]

2. <span data-ttu-id="06242-139">Klicka på pilen bredvid installationskälla.</span><span class="sxs-lookup"><span data-stu-id="06242-139">Click the arrow next to the installation source.</span></span>
3. <span data-ttu-id="06242-140">I **SENDER_ID**, om du redan har ett numeriskt projekt-ID för Google Developer Console-projekt, du kan lägga till den här.</span><span class="sxs-lookup"><span data-stu-id="06242-140">In **SENDER_ID**, if you already have a numeric project ID for the Google Developer Console project, you can add it here.</span></span> <span data-ttu-id="06242-141">Annars kan du ange en platshållarvärde, till exempel 777777.</span><span class="sxs-lookup"><span data-stu-id="06242-141">Otherwise, enter a placeholder value, like 777777.</span></span>  <span data-ttu-id="06242-142">Om du utvecklar för Android, kan du uppdatera det här värdet i config.xml senare.</span><span class="sxs-lookup"><span data-stu-id="06242-142">If you are targeting Android, you can update this value in config.xml later.</span></span>
4. <span data-ttu-id="06242-143">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="06242-143">Click **Add**.</span></span>

<span data-ttu-id="06242-144">Push-plugin-programmet har installerats.</span><span class="sxs-lookup"><span data-stu-id="06242-144">The push plugin is now installed.</span></span>

#### <a name="install-the-device-plugin"></a><span data-ttu-id="06242-145">Installera plugin-program för enhet</span><span class="sxs-lookup"><span data-stu-id="06242-145">Install the device plugin</span></span>
<span data-ttu-id="06242-146">Följ samma steg som du använde för att installera push plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="06242-146">Follow the same procedure you used to install the push plugin.</span></span>  <span data-ttu-id="06242-147">Lägga till plugin-program för enhet från listan över Core plugin-program (klicka på **plugin-program** > **Core** att hitta den).</span><span class="sxs-lookup"><span data-stu-id="06242-147">Add the Device plugin from the Core plugins list (click **Plugins** > **Core** to find it).</span></span> <span data-ttu-id="06242-148">Du behöver den här plugin-programmet för att hämta plattformsnamnet på.</span><span class="sxs-lookup"><span data-stu-id="06242-148">You need this plugin to obtain the platform name.</span></span>

#### <a name="register-your-device-on-application-start-up"></a><span data-ttu-id="06242-149">Registrera din enhet på programmet uppstart</span><span class="sxs-lookup"><span data-stu-id="06242-149">Register your device on application start-up</span></span>
<span data-ttu-id="06242-150">Vi ta först minimal kod för Android.</span><span class="sxs-lookup"><span data-stu-id="06242-150">Initially, we include some minimal code for Android.</span></span> <span data-ttu-id="06242-151">Senare ändra app att köras på iOS- eller Windows 10.</span><span class="sxs-lookup"><span data-stu-id="06242-151">Later, modify the app to run on iOS or Windows 10.</span></span>

1. <span data-ttu-id="06242-152">Lägg till ett anrop till **registerForPushNotifications** under återanrop för inloggningen eller längst ned i den **onDeviceReady** metod:</span><span class="sxs-lookup"><span data-stu-id="06242-152">Add a call to **registerForPushNotifications** during the callback for the login process, or at the bottom of  the **onDeviceReady** method:</span></span>

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    <span data-ttu-id="06242-153">Det här exemplet visar anropar **registerForPushNotifications** när autentiseringen lyckas.</span><span class="sxs-lookup"><span data-stu-id="06242-153">This example shows calling **registerForPushNotifications** after authentication succeeds.</span></span>  <span data-ttu-id="06242-154">Du kan anropa `registerForPushNotifications()` så ofta som krävs.</span><span class="sxs-lookup"><span data-stu-id="06242-154">You can call `registerForPushNotifications()` as often as is required.</span></span>

2. <span data-ttu-id="06242-155">Lägg till de nya **registerForPushNotifications** metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="06242-155">Add the new **registerForPushNotifications** method as follows:</span></span>

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
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
3. <span data-ttu-id="06242-156">(Android) I föregående kod ersätter `Your_Project_ID` med numeriskt projektet ID för din app från den [Google Developer Console][18].</span><span class="sxs-lookup"><span data-stu-id="06242-156">(Android) In the preceding code, replace `Your_Project_ID` with the numeric project ID for your app from the  [Google Developer Console][18].</span></span>

## <a name="optional-configure-and-run-the-app-on-android"></a><span data-ttu-id="06242-157">(Valfritt) Konfigurera och köra appen på Android</span><span class="sxs-lookup"><span data-stu-id="06242-157">(Optional) Configure and run the app on Android</span></span>
<span data-ttu-id="06242-158">Slutför det här avsnittet om du vill aktivera push-meddelanden för Android.</span><span class="sxs-lookup"><span data-stu-id="06242-158">Complete this section to enable push notifications for Android.</span></span>

#### <span data-ttu-id="06242-159"><a name="enable-gcm"></a>Aktivera Firebase Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="06242-159"><a name="enable-gcm"></a>Enable Firebase Cloud Messaging</span></span>
<span data-ttu-id="06242-160">Eftersom vi utvecklar Google Android-plattformen först, måste du aktivera Firebase Cloud Messaging.</span><span class="sxs-lookup"><span data-stu-id="06242-160">Since we are targeting the Google Android platform initially, you must enable Firebase Cloud Messaging.</span></span>

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <span data-ttu-id="06242-161"><a name="configure-backend"></a>Konfigurera serverdelen för Mobilappen för att skicka push-begäranden som använder FCM</span><span class="sxs-lookup"><span data-stu-id="06242-161"><a name="configure-backend"></a>Configure the Mobile App backend to send push requests using FCM</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a><span data-ttu-id="06242-162">Konfigurera din Cordova-app för Android</span><span class="sxs-lookup"><span data-stu-id="06242-162">Configure your Cordova app for Android</span></span>
<span data-ttu-id="06242-163">Öppna config.xml i Cordova-app och Ersätt `Your_Project_ID` med numeriskt projektet ID för din app från den [Google Developer Console][18].</span><span class="sxs-lookup"><span data-stu-id="06242-163">In your Cordova app, open config.xml and replace `Your_Project_ID` with the numeric project ID for your app from the [Google Developer Console][18].</span></span>

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

<span data-ttu-id="06242-164">Öppna index.js och uppdatera koden med numeriska projekt-ID.</span><span class="sxs-lookup"><span data-stu-id="06242-164">Open index.js and update the code to use your numeric project ID.</span></span>

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

#### <span data-ttu-id="06242-165"><a name="configure-device"></a>Konfigurera din Android-enhet för USB-felsökning</span><span class="sxs-lookup"><span data-stu-id="06242-165"><a name="configure-device"></a>Configure your Android device for USB debugging</span></span>
<span data-ttu-id="06242-166">Innan du kan distribuera programmet till din Android-enhet, måste du aktivera USB-felsökning.</span><span class="sxs-lookup"><span data-stu-id="06242-166">Before you can deploy your application to your Android Device, you need to enable USB Debugging.</span></span>  <span data-ttu-id="06242-167">Utför följande steg på din Android-telefon:</span><span class="sxs-lookup"><span data-stu-id="06242-167">Perform the following steps on your Android phone:</span></span>

1. <span data-ttu-id="06242-168">Gå till **inställningar** > **om telefonen**, tryck på **Build-nummer** förrän utvecklarläge har aktiverats (ungefär sju gånger).</span><span class="sxs-lookup"><span data-stu-id="06242-168">Go to **Settings** > **About phone**, then tap the **Build number** until developer mode is enabled  (about seven times).</span></span>
2. <span data-ttu-id="06242-169">Tillbaka i **inställningar** > **utvecklaralternativ** aktivera **USB-felsökning**, Anslut din Android-telefon till din utvecklings-dator med en USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="06242-169">Back in **Settings** > **Developer Options** enable **USB debugging**, then connect your Android phone  to your development PC with a USB Cable.</span></span>

<span data-ttu-id="06242-170">Vi testade detta med hjälp av en Google Nexus 5 X-enhet som kör Android 6.0 (Marshmallow).</span><span class="sxs-lookup"><span data-stu-id="06242-170">We tested this using a Google Nexus 5X device running Android 6.0 (Marshmallow).</span></span>  <span data-ttu-id="06242-171">Teknikerna som är dock gemensamma för alla moderna Android-versionen.</span><span class="sxs-lookup"><span data-stu-id="06242-171">However, the techniques are common across any modern Android release.</span></span>

#### <a name="install-google-play-services"></a><span data-ttu-id="06242-172">Installera Google Play-tjänster</span><span class="sxs-lookup"><span data-stu-id="06242-172">Install Google Play Services</span></span>
<span data-ttu-id="06242-173">Push-plugin-programmet är beroende av Android Google Play-tjänster för push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="06242-173">The push plugin relies on Android Google Play Services for push notifications.</span></span>

1. <span data-ttu-id="06242-174">I Visual Studio klickar du på **verktyg** > **Android** > **Android SDK Manager**, expandera den **tillägg** mappen och markera kryssrutan för att kontrollera att var och en av följande SDK är installerat.</span><span class="sxs-lookup"><span data-stu-id="06242-174">In Visual Studio, click **Tools** > **Android** > **Android SDK Manager**, expand the **Extras** folder and  check the box to make sure that each of the following SDKs is installed.</span></span>

   * <span data-ttu-id="06242-175">Android 2.3 eller högre</span><span class="sxs-lookup"><span data-stu-id="06242-175">Android 2.3 or higher</span></span>
   * <span data-ttu-id="06242-176">Google databasen revision 27 eller högre</span><span class="sxs-lookup"><span data-stu-id="06242-176">Google Repository revision 27 or higher</span></span>
   * <span data-ttu-id="06242-177">Google Play Services 9.0.2 eller högre</span><span class="sxs-lookup"><span data-stu-id="06242-177">Google Play Services 9.0.2 or higher</span></span>

2. <span data-ttu-id="06242-178">Klicka på **installationspaket** och vänta på att installationen ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="06242-178">Click **Install Packages** and wait for the installation to complete.</span></span>

<span data-ttu-id="06242-179">De aktuella bibliotek som krävs finns i den [phonegap-plugin-push-installation av dokumentationen][19].</span><span class="sxs-lookup"><span data-stu-id="06242-179">The current required libraries are listed in the [phonegap-plugin-push installation documentation][19].</span></span>

#### <a name="test-push-notifications-in-the-app-on-android"></a><span data-ttu-id="06242-180">Testa push-meddelanden i appen på Android</span><span class="sxs-lookup"><span data-stu-id="06242-180">Test push notifications in the app on Android</span></span>
<span data-ttu-id="06242-181">Du kan nu testa push-meddelanden genom att köra appen och lägga till objekt i tabellen TodoItem.</span><span class="sxs-lookup"><span data-stu-id="06242-181">You can now test push notifications by running the app and inserting items in the TodoItem table.</span></span> <span data-ttu-id="06242-182">Du kan testa från samma enhet eller från en annan enhet som du använder samma backend.</span><span class="sxs-lookup"><span data-stu-id="06242-182">You can test from the same device or from a second device, as long as you are using the same backend.</span></span> <span data-ttu-id="06242-183">Testa din Cordova-app på Android-plattformen i något av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="06242-183">Test your Cordova app on the Android platform in one of the following ways:</span></span>

* <span data-ttu-id="06242-184">**På en fysisk enhet:** Anslut din Android-enhet till utvecklingsdator med en USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="06242-184">**On a physical device:** Attach your Android device to your development computer with a USB cable.</span></span>  <span data-ttu-id="06242-185">I stället för **Google Android-emulatorn**väljer **enhet**.</span><span class="sxs-lookup"><span data-stu-id="06242-185">Instead of **Google Android Emulator**, select **Device**.</span></span> <span data-ttu-id="06242-186">Visual Studio distribuerar programmet till enheten och därefter körs programmet.</span><span class="sxs-lookup"><span data-stu-id="06242-186">Visual Studio deploys the application to the device and then runs the application.</span></span>  <span data-ttu-id="06242-187">Du kan sedan interagera med program på enheten.</span><span class="sxs-lookup"><span data-stu-id="06242-187">You can then interact with the application on the device.</span></span>

  <span data-ttu-id="06242-188">Förbättra din utvecklingsmetod.</span><span class="sxs-lookup"><span data-stu-id="06242-188">Improve your development experience.</span></span>  <span data-ttu-id="06242-189">Dela program som skärm [Mobizen] [ 20] kan hjälpa dig att utveckla ett Android-program.</span><span class="sxs-lookup"><span data-stu-id="06242-189">Screen sharing applications such as [Mobizen][20] can assist you in developing an Android application.</span></span>  <span data-ttu-id="06242-190">Mobizen projekt skärmen Android till en webbläsare på din dator.</span><span class="sxs-lookup"><span data-stu-id="06242-190">Mobizen projects your Android screen to a web browser on your PC.</span></span>

* <span data-ttu-id="06242-191">**På en Android-emulatorn:** finns ytterligare konfigurationssteg som krävs vid körning på en emulator.</span><span class="sxs-lookup"><span data-stu-id="06242-191">**On an Android emulator:** There are additional configuration steps required when running on an emulator.</span></span>

    <span data-ttu-id="06242-192">Kontrollera att du distribuerar till en virtuell enhet som innehåller Google APIs som mål, som visas i hanteraren för Android Virtual Device (AVD).</span><span class="sxs-lookup"><span data-stu-id="06242-192">Make sure you are deploying to a virtual device that has Google APIs set as the target, as shown in the Android Virtual Device (AVD) manager.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    <span data-ttu-id="06242-193">Om du vill använda en snabbare x86 emulatorn du [installera drivrutinen HAXM] [ 11] och konfigurera emulatorn för att använda den.</span><span class="sxs-lookup"><span data-stu-id="06242-193">If you want to use a faster x86 emulator, you [install the HAXM driver][11] and configure the emulator to use it.</span></span>

    <span data-ttu-id="06242-194">Lägg till ett Google-konto för Android-enhet genom att klicka på **appar** > **inställningar** > **Lägg till konto**, följ sedan anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="06242-194">Add a Google account to the Android device by clicking **Apps** > **Settings** > **Add account**, then follow the prompts.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    <span data-ttu-id="06242-195">Kör appen todolist som innan och infoga ett nytt todo-objekt.</span><span class="sxs-lookup"><span data-stu-id="06242-195">Run the todolist app as before and insert a new todo item.</span></span> <span data-ttu-id="06242-196">Den här tiden kan visas en meddelandeikonen i meddelandefältet.</span><span class="sxs-lookup"><span data-stu-id="06242-196">This time, a notification icon is displayed in the notification area.</span></span> <span data-ttu-id="06242-197">Du kan öppna lådan meddelande om du vill visa den fullständiga texten i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="06242-197">You can open the notification drawer to view the full text of the notification.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a><span data-ttu-id="06242-198">(Valfritt) Konfigurera och köra på iOS</span><span class="sxs-lookup"><span data-stu-id="06242-198">(Optional) Configure and run on iOS</span></span>
<span data-ttu-id="06242-199">Det här avsnittet handlar om att köra Cordova-projektet på iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="06242-199">This section is for running the Cordova project on iOS devices.</span></span> <span data-ttu-id="06242-200">Om du inte arbetar med iOS-enheter, kan du hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="06242-200">If you are not working with iOS devices, you can skip this section.</span></span>

#### <a name="install-and-run-the-ios-remote-build-agent-on-a-mac-or-cloud-service"></a><span data-ttu-id="06242-201">Installera och köra iOS remote build-agent på en Mac- eller molnet tjänst</span><span class="sxs-lookup"><span data-stu-id="06242-201">Install and run the iOS remote build agent on a Mac or cloud service</span></span>
<span data-ttu-id="06242-202">Innan du kan köra en Cordova-app på iOS med Visual Studio, gå igenom stegen i den [iOS inställningsguiden] [ 12] att installera och köra remote build-agenten.</span><span class="sxs-lookup"><span data-stu-id="06242-202">Before you can run a Cordova app on iOS using Visual Studio, go through the steps in the [iOS Setup Guide][12] to install and run the remote build agent.</span></span>

<span data-ttu-id="06242-203">Kontrollera att du kan bygga appen för iOS.</span><span class="sxs-lookup"><span data-stu-id="06242-203">Make sure you can build the app for iOS.</span></span> <span data-ttu-id="06242-204">Stegen i guiden för installation krävs för att skapa för iOS från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="06242-204">The steps in the setup guide are required to build for iOS from Visual Studio.</span></span> <span data-ttu-id="06242-205">Om du inte har en Mac, kan du skapa för iOS med fjärråtkomst build-agenten på en tjänst som MacInCloud.</span><span class="sxs-lookup"><span data-stu-id="06242-205">If you do not have a Mac, you can build for iOS using the remote build agent on a service like MacInCloud.</span></span> <span data-ttu-id="06242-206">Mer information finns i [kör iOS-app i molnet][21].</span><span class="sxs-lookup"><span data-stu-id="06242-206">For more info, see [Run your iOS app in the cloud][21].</span></span>

> [!NOTE]
> <span data-ttu-id="06242-207">XCode 7 eller senare krävs för att använda push-plugin-programmet på iOS.</span><span class="sxs-lookup"><span data-stu-id="06242-207">XCode 7 or greater is required to use the push plugin on iOS.</span></span>

#### <a name="find-the-id-to-use-as-your-app-id"></a><span data-ttu-id="06242-208">Hitta ID som ska användas som App-ID</span><span class="sxs-lookup"><span data-stu-id="06242-208">Find the ID to use as your App ID</span></span>
<span data-ttu-id="06242-209">Innan du registrerar appen för push-meddelanden, öppna config.xml i Cordova-app hitta den `id` attributvärdet i elementet widget och kopiera den för senare användning.</span><span class="sxs-lookup"><span data-stu-id="06242-209">Before you register your app for push notifications, open config.xml in your Cordova app, find the `id` attribute value in the widget element, and copy it for later use.</span></span> <span data-ttu-id="06242-210">I följande XML-ID: T är `io.cordova.myapp7777777`.</span><span class="sxs-lookup"><span data-stu-id="06242-210">In the following XML, the ID is `io.cordova.myapp7777777`.</span></span>

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
          version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

<span data-ttu-id="06242-211">Använd den här identifieraren senare, när du skapar ett App-ID på Apple developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="06242-211">Later, use this identifier when you create an App ID on Apple's developer portal.</span></span> <span data-ttu-id="06242-212">Om du skapar ett annat App-ID på developer-portalen måste du utföra några extra steg senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="06242-212">If you create a different App ID on the developer portal, you must take a few extra steps later in this tutorial.</span></span> <span data-ttu-id="06242-213">ID: T i widget-elementet måste matcha det App-ID på developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="06242-213">The ID in the widget element must match the App ID on the developer portal.</span></span>

#### <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a><span data-ttu-id="06242-214">Registrera appen för push-meddelanden på Apple developer-portalen</span><span class="sxs-lookup"><span data-stu-id="06242-214">Register the app for push notifications on Apple's developer portal</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[<span data-ttu-id="06242-215">Titta på en video som visar liknande steg</span><span class="sxs-lookup"><span data-stu-id="06242-215">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-to-send-push-notifications"></a><span data-ttu-id="06242-216">Konfigurera Azure för att skicka push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="06242-216">Configure Azure to send push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a><span data-ttu-id="06242-217">Kontrollera att din App-ID som matchar din Cordova-app</span><span class="sxs-lookup"><span data-stu-id="06242-217">Verify that your App ID matches your Cordova app</span></span>
<span data-ttu-id="06242-218">Om App-ID som du redan skapat i Apple Developer kontot matchar ID för elementet widget i config.xml, kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="06242-218">If the App ID you created in your Apple Developer Account already matches the ID of the widget element in config.xml, you can skip this step.</span></span> <span data-ttu-id="06242-219">Dock ID: N inte matchar gör du följande:</span><span class="sxs-lookup"><span data-stu-id="06242-219">However, if the IDs don't match, take the following steps:</span></span>

1. <span data-ttu-id="06242-220">Ta bort mappen plattformar från projektet.</span><span class="sxs-lookup"><span data-stu-id="06242-220">Delete the platforms folder from your project.</span></span>
2. <span data-ttu-id="06242-221">Ta bort mappen plugin-program från ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="06242-221">Delete the plugins folder from your project.</span></span>
3. <span data-ttu-id="06242-222">Ta bort mappen node_modules från projektet.</span><span class="sxs-lookup"><span data-stu-id="06242-222">Delete the node_modules folder from your project.</span></span>
4. <span data-ttu-id="06242-223">Uppdatera attributet id för elementet widget i config.xml använda App-ID som du skapade i Apple Developer-konto.</span><span class="sxs-lookup"><span data-stu-id="06242-223">Update the id attribute of the widget element in config.xml to use the App ID that you created in your  Apple Developer Account.</span></span>
5. <span data-ttu-id="06242-224">Återskapa projektet.</span><span class="sxs-lookup"><span data-stu-id="06242-224">Rebuild your project.</span></span>

##### <a name="test-push-notifications-in-your-ios-app"></a><span data-ttu-id="06242-225">Testa push-meddelanden i iOS-app</span><span class="sxs-lookup"><span data-stu-id="06242-225">Test push notifications in your iOS app</span></span>
1. <span data-ttu-id="06242-226">I Visual Studio, se till att **iOS** väljs som mål för distribution och välj sedan **enhet** ska köras på den anslutna iOS-enheten.</span><span class="sxs-lookup"><span data-stu-id="06242-226">In Visual Studio, make sure that **iOS** is selected as the deployment target, and then choose **Device** to run on your connected iOS device.</span></span>

    <span data-ttu-id="06242-227">Du kan köra på en iOS-enhet som är ansluten till datorn med iTunes.</span><span class="sxs-lookup"><span data-stu-id="06242-227">You can run on an iOS device connected to your PC using iTunes.</span></span> <span data-ttu-id="06242-228">IOS-simulatorn stöder inte push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="06242-228">The iOS Simulator does not support push notifications.</span></span>

2. <span data-ttu-id="06242-229">Tryck på den **kör** knappen eller **F5** i Visual Studio för att skapa projektet och starta appen i en iOS-enhet, och klicka på **OK** att ta emot push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="06242-229">Press the **Run** button or **F5** in Visual Studio to build the project and start the app in an iOS  device, then click **OK** to accept push notifications.</span></span>

   > [!NOTE]
   > <span data-ttu-id="06242-230">Appen begär bekräftelse för push-meddelanden under den första körningen.</span><span class="sxs-lookup"><span data-stu-id="06242-230">The app requests confirmation for push notifications during the first run.</span></span>

3. <span data-ttu-id="06242-231">I appen, skriver du en uppgift och klicka sedan på plustecknet (+) ikon.</span><span class="sxs-lookup"><span data-stu-id="06242-231">In the app, type a task, and then click the plus (+) icon.</span></span>
4. <span data-ttu-id="06242-232">Kontrollera att ett meddelande tas emot och sedan klicka på OK för att stänga meddelandet.</span><span class="sxs-lookup"><span data-stu-id="06242-232">Verify that a notification is received, then click OK to dismiss the notification.</span></span>

## <a name="optional-configure-and-run-on-windows"></a><span data-ttu-id="06242-233">(Valfritt) Konfigurera och köra i Windows</span><span class="sxs-lookup"><span data-stu-id="06242-233">(Optional) Configure and run on Windows</span></span>
<span data-ttu-id="06242-234">Det här avsnittet handlar om att köra Apache Cordova-app-projekt på Windows 10-enheter (PhoneGap push plugin-programmet stöds på Windows 10).</span><span class="sxs-lookup"><span data-stu-id="06242-234">This section is for running the Apache Cordova app project on Windows 10 devices (the PhoneGap push plugin is supported on Windows 10).</span></span> <span data-ttu-id="06242-235">Om du inte arbetar med Windows-enheter, kan du hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="06242-235">If you are not working with Windows devices, you can skip this section.</span></span>

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a><span data-ttu-id="06242-236">Registrera din Windows-app för push-meddelanden med WNS</span><span class="sxs-lookup"><span data-stu-id="06242-236">Register your Windows app for push notifications with WNS</span></span>
<span data-ttu-id="06242-237">Om du vill använda Store-alternativ i Visual Studio, Välj ett mål för Windows från listan över plattformar som lösning som **Windows x64** eller **Windows x86** (undvika **Windows Platform** för push-meddelanden).</span><span class="sxs-lookup"><span data-stu-id="06242-237">To use the Store options in Visual Studio, select a Windows target from the Solution Platforms list, like **Windows-x64** or **Windows-x86** (avoid **Windows-AnyCPU** for push notifications).</span></span>

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

<span data-ttu-id="06242-238">[Se en video som visar liknande steg][13]</span><span class="sxs-lookup"><span data-stu-id="06242-238">[Watch a video showing similar steps][13]</span></span>

#### <a name="configure-the-notification-hub-for-wns"></a><span data-ttu-id="06242-239">Konfigurera meddelandehubben för WNS</span><span class="sxs-lookup"><span data-stu-id="06242-239">Configure the notification hub for WNS</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-to-support-windows-push-notifications"></a><span data-ttu-id="06242-240">Konfigurera din Cordova-app för att stödja Windows push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="06242-240">Configure your Cordova app to support Windows push notifications</span></span>
<span data-ttu-id="06242-241">Öppna configuration designer (Högerklicka på config.xml och välj **Vydesigner**), Välj den **Windows** , och välj **Windows 10** under **Windows målversionen**.</span><span class="sxs-lookup"><span data-stu-id="06242-241">Open the configuration designer (right-click on config.xml and select **View Designer**), select the **Windows** tab, and choose **Windows 10** under **Windows Target Version**.</span></span>

<span data-ttu-id="06242-242">Stöd för push-meddelanden i din standard (debug) versioner, öppna build.json fil</span><span class="sxs-lookup"><span data-stu-id="06242-242">To support push notifications in your default (debug) builds, open build.json file.</span></span> <span data-ttu-id="06242-243">Kopiera ”version”-konfigurationen till debug-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="06242-243">Copy the "release" configuration to your debug configuration.</span></span>

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

<span data-ttu-id="06242-244">Efter uppdateringen måste bör build.json innehålla följande kod:</span><span class="sxs-lookup"><span data-stu-id="06242-244">After the update, the build.json should contain the following code:</span></span>

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

<span data-ttu-id="06242-245">Bygga appen och kontrollera att du har några fel.</span><span class="sxs-lookup"><span data-stu-id="06242-245">Build the app and verify that you have no errors.</span></span> <span data-ttu-id="06242-246">Klientappen bör nu registrera för meddelanden från serverdelen för Mobilappen.</span><span class="sxs-lookup"><span data-stu-id="06242-246">Your client app should now register for the notifications from the Mobile App backend.</span></span> <span data-ttu-id="06242-247">Upprepa det här avsnittet för varje Windows-projekt i din lösning.</span><span class="sxs-lookup"><span data-stu-id="06242-247">Repeat this section for every Windows project in your solution.</span></span>

#### <a name="test-push-notifications-in-your-windows-app"></a><span data-ttu-id="06242-248">Testa push-meddelanden i Windows-appen</span><span class="sxs-lookup"><span data-stu-id="06242-248">Test push notifications in your Windows app</span></span>
<span data-ttu-id="06242-249">Se till att en Windows-plattform som är markerad som mål distributionen i Visual Studio **Windows x64** eller **Windows x86**.</span><span class="sxs-lookup"><span data-stu-id="06242-249">In Visual Studio, make sure that a Windows platform is selected as the deployment target, such as **Windows-x64** or **Windows-x86**.</span></span> <span data-ttu-id="06242-250">Om du vill köra appen på en Windows 10-dator som värd för Visual Studio väljer **lokal dator**.</span><span class="sxs-lookup"><span data-stu-id="06242-250">To run the app on a Windows 10 PC hosting Visual Studio, choose **Local Machine**.</span></span>

<span data-ttu-id="06242-251">Klicka på Kör för att bygga projektet och starta appen.</span><span class="sxs-lookup"><span data-stu-id="06242-251">Press the Run button to build the project and start the app.</span></span>

<span data-ttu-id="06242-252">Skriv ett namn för en ny todoitem i appen och klicka sedan på plustecknet (+) ikon för att lägga till den.</span><span class="sxs-lookup"><span data-stu-id="06242-252">In the app, type a name for a new todoitem, and then click the plus (+) icon to add it.</span></span>

<span data-ttu-id="06242-253">Kontrollera att ett meddelande tas emot när objektet har lagts till.</span><span class="sxs-lookup"><span data-stu-id="06242-253">Verify that a notification is received when the item is added.</span></span>

## <span data-ttu-id="06242-254"><a name="next-steps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="06242-254"><a name="next-steps"></a>Next Steps</span></span>
* <span data-ttu-id="06242-255">Läs mer om [Meddelandehubbar] [ 17] mer information om push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="06242-255">Read about [Notification Hubs][17] to learn about push notifications.</span></span>
* <span data-ttu-id="06242-256">Om du inte redan har gjort det fortsätta kursen av [att lägga till autentisering] [ 14] i din Apache Cordova-app.</span><span class="sxs-lookup"><span data-stu-id="06242-256">If you have not already done so, continue the tutorial by [Adding Authentication][14] to your Apache Cordova app.</span></span>

<span data-ttu-id="06242-257">Lär dig hur du använder SDK: er.</span><span class="sxs-lookup"><span data-stu-id="06242-257">Learn how to use the SDKs.</span></span>

* <span data-ttu-id="06242-258">[Apache Cordova-SDK][15]</span><span class="sxs-lookup"><span data-stu-id="06242-258">[Apache Cordova SDK][15]</span></span>
* <span data-ttu-id="06242-259">[ASP.NET Server-SDK][1]</span><span class="sxs-lookup"><span data-stu-id="06242-259">[ASP.NET Server SDK][1]</span></span>
* <span data-ttu-id="06242-260">[Node.js Server-SDK][16]</span><span class="sxs-lookup"><span data-stu-id="06242-260">[Node.js Server SDK][16]</span></span>

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
