---
title: aaaSend push-meddelanden tooChrome appar med Azure Notification Hubs | Microsoft Docs
description: "Lär dig hur toouse Azure Notification Hubs toosend push-meddelanden tooa Chrome-appen."
services: notification-hubs
keywords: mobila push-meddelanden, push-meddelanden, push-meddelande, push-meddelanden i chrome
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 75d4ff59-d04a-455f-bd44-0130a68e641f
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-chrome
ms.devlang: JavaScript
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 7dec8ab02622563bc3730a2e96820da8932d22f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-push-notifications-toochrome-apps-with-azure-notification-hubs"></a><span data-ttu-id="5f557-104">Skicka push-meddelanden tooChrome appar med Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="5f557-104">Send push notifications tooChrome apps with Azure Notification Hubs</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

<span data-ttu-id="5f557-105">Det här avsnittet visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa Chrome-App som kommer att visas i hello kontexten för hello webbläsaren Google Chrome.</span><span class="sxs-lookup"><span data-stu-id="5f557-105">This topic shows you how toouse Azure Notification Hubs toosend push notifications tooa Chrome App, which will be displayed within hello context of hello Google Chrome browser.</span></span> <span data-ttu-id="5f557-106">I den här självstudiekursen kommer vi att skapa en Chrome-app som tar emot push-meddelanden genom att använda [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/).</span><span class="sxs-lookup"><span data-stu-id="5f557-106">In this tutorial, we will create a Chrome app that receives push notifications by using [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/).</span></span> 

> [!NOTE]
> <span data-ttu-id="5f557-107">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="5f557-107">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="5f557-108">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="5f557-108">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="5f557-109">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="5f557-109">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).</span></span>
> 
> 

<span data-ttu-id="5f557-110">hello självstudiekursen vägleder dig igenom de här stegen tooenable push-meddelanden:</span><span class="sxs-lookup"><span data-stu-id="5f557-110">hello tutorial walks you through these basic steps tooenable push notifications:</span></span>

* [<span data-ttu-id="5f557-111">Aktivera Google Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="5f557-111">Enable Google Cloud Messaging</span></span>](#register)
* [<span data-ttu-id="5f557-112">Konfigurera meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="5f557-112">Configure your notification hub</span></span>](#configure-hub)
* [<span data-ttu-id="5f557-113">Ansluta din Chrome-appen toohello notification hub</span><span class="sxs-lookup"><span data-stu-id="5f557-113">Connect your Chrome App toohello notification hub</span></span>](#connect-app)
* [<span data-ttu-id="5f557-114">Skicka ett push-meddelande tooyour Chrome-appen</span><span class="sxs-lookup"><span data-stu-id="5f557-114">Send a push notification tooyour Chrome App</span></span>](#send)
* [<span data-ttu-id="5f557-115">Ytterligare funktioner</span><span class="sxs-lookup"><span data-stu-id="5f557-115">Additional functionality & capabilities</span></span>](#next-steps)

> [!NOTE]
> <span data-ttu-id="5f557-116">Chrome app push-meddelanden är inte generisk i webbläsaren meddelanden - de specifika toohello webbläsarens utökningsbarhetsmodell (se [översikt över Chrome-appar] information).</span><span class="sxs-lookup"><span data-stu-id="5f557-116">Chrome app push notifications are not generic in-browser notifications - they are specific toohello browser extensibility model (see [Chrome Apps Overview] for details).</span></span> <span data-ttu-id="5f557-117">Dessutom toohello skrivbord webbläsaren Chrome-appar köra på mobila enheter (Android och iOS) via Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="5f557-117">In addition toohello desktop browser, Chrome apps run on mobile (Android and iOS) through Apache Cordova.</span></span> <span data-ttu-id="5f557-118">Se [Chrome-appar på mobila enheter] toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="5f557-118">See [Chrome Apps on Mobile] toolearn more.</span></span>
> 
> 

<span data-ttu-id="5f557-119">Konfigurera GCM och Azure Notification Hubs är identiska tooconfiguring för Android, eftersom [Google Cloud Messaging för Chrome] är inaktuell och hello samma GCM stöder nu både Android-enheter och Chrome-instanser.</span><span class="sxs-lookup"><span data-stu-id="5f557-119">Configuring GCM and Azure Notification Hubs is identical tooconfiguring for Android, since [Google Cloud Messaging for Chrome] has been deprecated and hello same GCM now supports both Android devices and Chrome instances.</span></span>

## <span data-ttu-id="5f557-120"><a id="register"></a>Aktivera Google Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="5f557-120"><a id="register"></a>Enable Google Cloud Messaging</span></span>
1. <span data-ttu-id="5f557-121">Navigera toohello [Google Cloud-konsolen] webbplats, logga in med dina Google-konto och klicka sedan på hello **skapa projekt** knappen.</span><span class="sxs-lookup"><span data-stu-id="5f557-121">Navigate toohello [Google Cloud Console] website, sign in with your Google account credentials, and then click hello **Create Project** button.</span></span> <span data-ttu-id="5f557-122">Ange ett lämpligt **projektnamn**, och klicka sedan på hello **skapa** knappen.</span><span class="sxs-lookup"><span data-stu-id="5f557-122">Provide an appropriate **Project Name**, and then click hello **Create** button.</span></span>
   
       ![Google Cloud Console - Create Project][1]
2. <span data-ttu-id="5f557-123">Anteckna hello **projektnumret** på hello **projekt** för hello-projekt som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="5f557-123">Make a note of hello **Project Number** on hello **Projects** page for hello project that you just created.</span></span> <span data-ttu-id="5f557-124">Du kan använda den som hello **GCM-avsändar-ID** i hello Chrome-appen tooregister med GCM.</span><span class="sxs-lookup"><span data-stu-id="5f557-124">You will use this as hello **GCM Sender ID** in hello Chrome App tooregister with GCM.</span></span>
   
       ![Google Cloud Console - Project Number][2]
3. <span data-ttu-id="5f557-125">Hello vänster klickar du på **API: er och aut**, bläddrar sedan nedåt och klicka på hello växla tooenable **Google Cloud Messaging för Android**.</span><span class="sxs-lookup"><span data-stu-id="5f557-125">In hello left pane, click **APIs & auth**, and then scroll down and click hello toggle tooenable **Google Cloud Messaging for Android**.</span></span> <span data-ttu-id="5f557-126">Du har inte tooenable **Google Cloud Messaging för Chrome**.</span><span class="sxs-lookup"><span data-stu-id="5f557-126">You don't have tooenable **Google Cloud Messaging for Chrome**.</span></span>
   
       ![Google Cloud Console - Server Key][3]
4. <span data-ttu-id="5f557-127">Hello vänster klickar du på **autentiseringsuppgifter** > **Skapa ny nyckel** > **servernyckel** > **skapa**.</span><span class="sxs-lookup"><span data-stu-id="5f557-127">In hello left pane, click **Credentials** > **Create New Key** > **Server Key** > **Create**.</span></span>
   
       ![Google Cloud Console - Credentials][4]
5. <span data-ttu-id="5f557-128">Anteckna hello server **API-nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="5f557-128">Make a note of hello server **API Key**.</span></span> <span data-ttu-id="5f557-129">Du konfigurerar det i ditt notification hub nästa tooenable den toosend push-meddelanden tooGCM.</span><span class="sxs-lookup"><span data-stu-id="5f557-129">You will configure this in your notification hub next, tooenable it toosend push notifications tooGCM.</span></span>
   
       ![Google Cloud Console - API Key][5]

## <span data-ttu-id="5f557-130"><a id="configure-hub"></a>Konfigurera meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="5f557-130"><a id="configure-hub"></a>Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<span data-ttu-id="5f557-131">&emsp;&emsp;6.</span><span class="sxs-lookup"><span data-stu-id="5f557-131">&emsp;&emsp;6.</span></span>   <span data-ttu-id="5f557-132">I hello **inställningar** bladet väljer **Notification Services** och sedan **Google (GCM)**.</span><span class="sxs-lookup"><span data-stu-id="5f557-132">In hello **Settings** blade, select **Notification Services** and then **Google (GCM)**.</span></span> <span data-ttu-id="5f557-133">Ange hello API-nyckeln och spara.</span><span class="sxs-lookup"><span data-stu-id="5f557-133">Enter hello API key and save.</span></span>

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

## <span data-ttu-id="5f557-135"><a id="connect-app"></a>Ansluta din Chrome-appen toohello notification hub</span><span class="sxs-lookup"><span data-stu-id="5f557-135"><a id="connect-app"></a>Connect your Chrome App toohello notification hub</span></span>
<span data-ttu-id="5f557-136">Din meddelandehubb har nu konfigurerat toowork med GCM och du har hello anslutning strängar tooregister app-tooboth ta emot och skicka push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5f557-136">Your notification hub is now configured toowork with GCM, and you have hello connection strings tooregister your app tooboth receive and send push notifications.</span></span> <span data-ttu-id="5f557-137">LK</span><span class="sxs-lookup"><span data-stu-id="5f557-137">LK</span></span>

### <a name="create-a-new-chrome-app"></a><span data-ttu-id="5f557-138">Skapa en ny Chrome-App</span><span class="sxs-lookup"><span data-stu-id="5f557-138">Create a new Chrome App</span></span>
<span data-ttu-id="5f557-139">hello exemplet nedan baseras på hello [Chrome App GCM Sample] och använder hello rekommenderade sätt toocreate en Chrome-App.</span><span class="sxs-lookup"><span data-stu-id="5f557-139">hello sample below is based on hello [Chrome App GCM Sample] and uses hello recommended way toocreate a Chrome App.</span></span> <span data-ttu-id="5f557-140">Vi betona hello steg specifikt relaterade tooAzure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="5f557-140">We will highlight hello steps specifically related tooAzure Notification Hubs.</span></span> 

> [!NOTE]
> <span data-ttu-id="5f557-141">Vi rekommenderar att du hämtar hello källa för den här Chrome-appen från [Chrome App Notification Hub Sample].</span><span class="sxs-lookup"><span data-stu-id="5f557-141">We recommend that you download hello source for this Chrome App from [Chrome App Notification Hub Sample].</span></span>
> 
> 

<span data-ttu-id="5f557-142">hello Chrome-appen skapas via JavaScript och du kan använda någon av textredigerare för att skapa den.</span><span class="sxs-lookup"><span data-stu-id="5f557-142">hello Chrome App is created via JavaScript, and you can use any of your preferred word editors for creating it.</span></span> <span data-ttu-id="5f557-143">Nedan visas hur den här Chrome-appen kommer att se ut.</span><span class="sxs-lookup"><span data-stu-id="5f557-143">Below is what this Chrome App will look like.</span></span>

![Google Chrome-app][15]

1. <span data-ttu-id="5f557-145">Skapa en mapp och kalla den för `ChromePushApp`.</span><span class="sxs-lookup"><span data-stu-id="5f557-145">Create a folder and name it `ChromePushApp`.</span></span> <span data-ttu-id="5f557-146">Naturligtvis hello namn är godtycklig - om du döper mappen till något annat, kontrollerar du att du ersätta hello sökväg i hello krävs kodsegment.</span><span class="sxs-lookup"><span data-stu-id="5f557-146">Of course, hello name is arbitrary - if you name it something different, make sure you substitute hello path in hello required code segments.</span></span>
2. <span data-ttu-id="5f557-147">Hämta hello [biblioteket crypto-js] i hello-mappen som du skapade i hello andra steget.</span><span class="sxs-lookup"><span data-stu-id="5f557-147">Download hello [crypto-js library] in hello folder you created in hello second step.</span></span> <span data-ttu-id="5f557-148">Denna biblioteksmapp innehåller två undermappar: `components` och `rollups`.</span><span class="sxs-lookup"><span data-stu-id="5f557-148">This library folder will contain two subfolders: `components` and `rollups`.</span></span>
3. <span data-ttu-id="5f557-149">Skapa en `manifest.json`-fil.</span><span class="sxs-lookup"><span data-stu-id="5f557-149">Create a `manifest.json` file.</span></span> <span data-ttu-id="5f557-150">Alla Chrome-appar backas upp av en manifestfil som innehåller hello appmetadata. och viktigast av allt alla behörigheter som beviljas toohello app när hello användaren installerar den.</span><span class="sxs-lookup"><span data-stu-id="5f557-150">All Chrome Apps are backed by a manifest file that contains hello app metadata and, most importantly, all permissions that are granted toohello app when hello user installs it.</span></span>
   
        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }
   
    <span data-ttu-id="5f557-151">Meddelande hello `permissions` element, som anger att den här Chrome-appen kommer att kunna tooreceive push-meddelanden från GCM.</span><span class="sxs-lookup"><span data-stu-id="5f557-151">Notice hello `permissions` element, which specifies that this Chrome App will be able tooreceive push notifications from GCM.</span></span> <span data-ttu-id="5f557-152">Det måste även ange hello Azure Notification Hubs URI där hello Chrome-appen blir tooregister en REST-anrop.</span><span class="sxs-lookup"><span data-stu-id="5f557-152">It must also specify hello Azure Notification Hubs URI where hello Chrome App will make a REST call tooregister.</span></span>
    <span data-ttu-id="5f557-153">Vår exempelapp använder också en ikonfil `gcm_128.png`, som du hittar vid hello-källa som återanvänds från hello ursprungliga GCM-exemplet.</span><span class="sxs-lookup"><span data-stu-id="5f557-153">Our sample app also uses an icon file, `gcm_128.png`, that you will find at hello source that's reused from hello original GCM sample.</span></span> <span data-ttu-id="5f557-154">Du kan ersätta den med vilken bild som passar hello [ikonkriterierna](https://developer.chrome.com/apps/manifest/icons).</span><span class="sxs-lookup"><span data-stu-id="5f557-154">You can substitute it for any image that fits hello [icon criteria](https://developer.chrome.com/apps/manifest/icons).</span></span>
4. <span data-ttu-id="5f557-155">Skapa en fil med namnet `background.js` med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="5f557-155">Create a file called `background.js` with hello following code:</span></span>
   
        // Returns a new notification ID used in hello notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }
   
        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.
   
          // Concatenate all key-value pairs tooform a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);
   
          // Pop up a notification tooshow hello GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }
   
        var registerWindowCreated = false;
   
        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {
   
            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }
   
        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);
   
        // Set up listeners tootrigger hello first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);
   
    <span data-ttu-id="5f557-156">Detta är hello-fil som öppnas hello Chrome-appen fönstret HTML (**register.html**) och definierar också hanteraren hello **messageReceived** toohandle hello inkommande push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5f557-156">This is hello file that pops up hello Chrome App window HTML (**register.html**) and also defines hello handler **messageReceived** toohandle hello incoming push notification.</span></span>
5. <span data-ttu-id="5f557-157">Skapa en fil med namnet `register.html` -detta definierar hello Användargränssnittet för hello Chrome-appen.</span><span class="sxs-lookup"><span data-stu-id="5f557-157">Create a file called `register.html` - this defines hello UI of hello Chrome App.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="5f557-158">I det här exemplet används **CryptoJS v3.1.2**.</span><span class="sxs-lookup"><span data-stu-id="5f557-158">This sample uses **CryptoJS v3.1.2**.</span></span> <span data-ttu-id="5f557-159">Om du har hämtat en annan version av hello bibliotek Se till att du ska ersätta hello versionen i hello `src` sökväg.</span><span class="sxs-lookup"><span data-stu-id="5f557-159">If you downloaded another version of hello library, make sure you properly substitute hello version in hello `src` path.</span></span>
   > 
   > 
   
        <html>
   
        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>
   
        <body>
   
        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>
   
        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>
   
        <br/>
   
        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>
   
        <br/>
        <br/>
   
        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>
   
        </body>
   
        </html>
6. <span data-ttu-id="5f557-160">Skapa en fil med namnet `register.js` med hello koden nedan.</span><span class="sxs-lookup"><span data-stu-id="5f557-160">Create a file called `register.js` with hello code below.</span></span> <span data-ttu-id="5f557-161">Den här filen anger hello skriptet bakom `register.html`.</span><span class="sxs-lookup"><span data-stu-id="5f557-161">This file specifies hello script behind `register.html`.</span></span> <span data-ttu-id="5f557-162">Chrome-appar tillåter inte infogad körning, så att du har toocreate ett separat Säkerhetskopieringsskript för ditt användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="5f557-162">Chrome Apps do not allow inline execution, so you have toocreate a separate backing script for your UI.</span></span>
   
        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";
   
        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }
   
        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }
   
          document.getElementById("console").innerHTML = currentStatus  + status;
        }
   
        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);
   
          // Prevent register button from being clicked again before hello registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }
   
        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;
   
          if (chrome.runtime.lastError) {
            // When hello registration fails, handle hello error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }
   
          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;
   
          // Mark that hello first-time registration is done.
          chrome.storage.local.set({registered: true});
        }
   
        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }
   
        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";
   
          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });
   
          originalUri = endpoint + hubName;
        }
   
        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration
   
          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;
   
          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);
   
          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }
   
        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";
   
          // Update hello payload with hello registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);
   
          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();
   
          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };
   
          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }
   
          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");
   
          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }
   
    <span data-ttu-id="5f557-163">hello ovan skript har följande nyckelparametrar hello:</span><span class="sxs-lookup"><span data-stu-id="5f557-163">hello above script has hello following key parameters:</span></span>
   
   * <span data-ttu-id="5f557-164">**window.onLoad** definierar knapptryckningshändelserna för hello hello två knappar på hello Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="5f557-164">**window.onload** defines hello button-click events of hello two buttons on hello UI.</span></span> <span data-ttu-id="5f557-165">En står för registrering med GCM och hello andra använder hello registrerings-ID som returneras efter registrering med GCM tooregister med Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="5f557-165">One registers with GCM, and hello other uses hello registration ID that's returned after registration with GCM tooregister with Azure Notification Hubs.</span></span>
   * <span data-ttu-id="5f557-166">**updateLog** hello-funktion som gör att vi toohandle enkla loggningsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="5f557-166">**updateLog** is hello function that allows us toohandle simple logging capabilities.</span></span>
   * <span data-ttu-id="5f557-167">**registerWithGCM** hello första klickar på knappen hanterare, vilket gör hello `chrome.gcm.register` anropet tooGCM tooregister hello aktuella Chrome App-instansen.</span><span class="sxs-lookup"><span data-stu-id="5f557-167">**registerWithGCM** is hello first button-click handler, which makes hello `chrome.gcm.register` call tooGCM tooregister hello current Chrome App instance.</span></span>
   * <span data-ttu-id="5f557-168">**registerCallback** är hello Återanropsfunktionen som anropas när hello GCM registrering anropet returnerar.</span><span class="sxs-lookup"><span data-stu-id="5f557-168">**registerCallback** is hello callback function that gets called when hello GCM registration call returns.</span></span>
   * <span data-ttu-id="5f557-169">**registerWithNH** hello andra klickar på knappen hanterare, som registrerar med Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="5f557-169">**registerWithNH** is hello second button-click handler, which registers with Notification Hubs.</span></span> <span data-ttu-id="5f557-170">Hämtar `hubName` och `connectionString` (vilka hello-användaren har angett) och dekorationer hello Notification Hubs Registration REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="5f557-170">It gets `hubName` and `connectionString` (which hello user has specified) and crafts hello Notification Hubs Registration REST API call.</span></span>
   * <span data-ttu-id="5f557-171">**splitConnectionString** och **generateSaSToken** är hjälpprogram som representerar hello JavaScript-implementeringen av en SaS-token processen, som måste användas i alla REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="5f557-171">**splitConnectionString** and **generateSaSToken** are helpers that represent hello JavaScript implementation of a SaS token creation process, that must be used in all REST API calls.</span></span> <span data-ttu-id="5f557-172">Mer information finns i [Vanliga koncept](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f557-172">For more information, see [Common Concepts](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
   * <span data-ttu-id="5f557-173">**sendNHRegistrationRequest** är hello-funktionen som gör att en HTTP-REST anropa tooAzure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="5f557-173">**sendNHRegistrationRequest** is hello function that makes a HTTP REST call tooAzure Notification Hubs.</span></span>
   * <span data-ttu-id="5f557-174">**registrationPayload** definierar nyttolasten för hello registrering XML.</span><span class="sxs-lookup"><span data-stu-id="5f557-174">**registrationPayload** defines hello registration XML payload.</span></span> <span data-ttu-id="5f557-175">Mer information finns i [Skapa Registration NH REST API].</span><span class="sxs-lookup"><span data-stu-id="5f557-175">For more information, see [Create Registration NH REST API].</span></span> <span data-ttu-id="5f557-176">Vi uppdaterar hello registrerings-ID i den med det som togs emot från GCM.</span><span class="sxs-lookup"><span data-stu-id="5f557-176">We update hello registration ID in it with what we received from GCM.</span></span>
   * <span data-ttu-id="5f557-177">**klienten** är en instans av **XMLHttpRequest** att vi använder toomake hello HTTP POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="5f557-177">**client** is an instance of **XMLHttpRequest** that we use toomake hello HTTP POST request.</span></span> <span data-ttu-id="5f557-178">Observera att vi uppdaterar hello `Authorization` huvud med `sasToken`.</span><span class="sxs-lookup"><span data-stu-id="5f557-178">Note that we update hello `Authorization` header with `sasToken`.</span></span> <span data-ttu-id="5f557-179">Om detta anrop slutförs korrekt kommer den här Chrome App-instansen att registreras med Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="5f557-179">Successful completion of this call will register this Chrome App instance with Azure Notification Hubs.</span></span>

<span data-ttu-id="5f557-180">hello övergripande mappstrukturen för det här projektet ska se ut ungefär så här: ![Google Chrome App – mappstruktur][21]</span><span class="sxs-lookup"><span data-stu-id="5f557-180">hello overall folder structure for this project should resemble this: ![Google Chrome App - Folder Structure][21]</span></span>

### <a name="set-up-and-test-your-chrome-app"></a><span data-ttu-id="5f557-181">Konfigurera och testa din Chrome-app</span><span class="sxs-lookup"><span data-stu-id="5f557-181">Set up and test your Chrome App</span></span>
1. <span data-ttu-id="5f557-182">Öppna webbläsaren Chrome.</span><span class="sxs-lookup"><span data-stu-id="5f557-182">Open your Chrome browser.</span></span> <span data-ttu-id="5f557-183">Öppna **Chrome-tilläggen** och aktivera **utvecklarläget**.</span><span class="sxs-lookup"><span data-stu-id="5f557-183">Open **Chrome extensions** and enable **Developer mode**.</span></span>
   
       ![Google Chrome - Enable Developer Mode][16]
2. <span data-ttu-id="5f557-184">Klicka på **ladda uppackat tillägg** och navigera toohello mappen där du skapade hello-filer.</span><span class="sxs-lookup"><span data-stu-id="5f557-184">Click **Load unpacked extension** and navigate toohello folder where you created hello files.</span></span> <span data-ttu-id="5f557-185">Du kan också använda hello **Chrome-appar och tillägg Utvecklarverktyget**.</span><span class="sxs-lookup"><span data-stu-id="5f557-185">You can also optionally use hello **Chrome Apps & Extensions Developer Tool**.</span></span> <span data-ttu-id="5f557-186">Det här verktyget är en Chrome-App i sig själv (som installerats från hello Chrome Web Store) och ger avancerade felsökningsfunktioner för utveckling av Chrome-appen.</span><span class="sxs-lookup"><span data-stu-id="5f557-186">This tool is a Chrome App in itself (installed from hello Chrome Web Store) and provides advanced debugging capabilities for your Chrome App development.</span></span>
   
       ![Google Chrome - Load Unpacked Extension][17]
3. <span data-ttu-id="5f557-187">Om hello Chrome-appen har skapats utan fel, ska du se Chrome-appen visas.</span><span class="sxs-lookup"><span data-stu-id="5f557-187">If hello Chrome App is created without any errors, then you will see your Chrome App show up.</span></span>
   
       ![Google Chrome - Chrome App Display][18]
4. <span data-ttu-id="5f557-188">Ange hello **projektnumret** som du tidigare fått från hello **Google Cloud-konsolen** som hello avsändar-ID och klicka på **registrera med GCM**.</span><span class="sxs-lookup"><span data-stu-id="5f557-188">Enter hello **Project Number** that you got earlier from hello **Google Cloud Console** as hello sender ID, and click **Register with GCM**.</span></span> <span data-ttu-id="5f557-189">Du måste läsa hello-meddelande **registrering med GCM har genomförts.**</span><span class="sxs-lookup"><span data-stu-id="5f557-189">You must see hello message **Registration with GCM succeeded.**</span></span>
   
       ![Google Chrome - Chrome App Customization][19]
5. <span data-ttu-id="5f557-190">Ange din **Meddelandehubbsnamn** och hello **DefaultListenSharedAccessSignature** du fick från hello portal tidigare och klicka på **registrera med Azure Notification Hub**.</span><span class="sxs-lookup"><span data-stu-id="5f557-190">Enter your **Notification Hub Name** and hello **DefaultListenSharedAccessSignature** that you obtained from hello portal earlier, and click **Register with Azure Notification Hub**.</span></span> <span data-ttu-id="5f557-191">Du måste läsa hello-meddelande **registreringen på Notification Hub lyckades!**</span><span class="sxs-lookup"><span data-stu-id="5f557-191">You must see hello message **Notification Hub Registration successful!**</span></span> <span data-ttu-id="5f557-192">och hello information om hello registreringssvar som innehåller hello Azure Notification Hubs registration-ID.</span><span class="sxs-lookup"><span data-stu-id="5f557-192">and hello details of hello registration response, which contains hello Azure Notification Hubs registration ID.</span></span>
   
       ![Google Chrome - Specify Notification Hub Details][20]  

## <span data-ttu-id="5f557-193"><a name="send"></a>Skicka ett meddelande tooyour Chrome-appen</span><span class="sxs-lookup"><span data-stu-id="5f557-193"><a name="send"></a>Send a notification tooyour Chrome App</span></span>
<span data-ttu-id="5f557-194">I testsyfte skickar vi Chrome-push-meddelanden med hjälp av en .NET-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="5f557-194">For testing purposes, we will send Chrome push notifications by using a .NET console application.</span></span> 

> [!NOTE]
> <span data-ttu-id="5f557-195">Du kan skicka push-meddelanden med Notification Hubs från alla serverdelar via vårt offentliga <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-gränssnitt</a>.</span><span class="sxs-lookup"><span data-stu-id="5f557-195">You can send push notifications with Notification Hubs from any backend via our public <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST interface</a>.</span></span> <span data-ttu-id="5f557-196">Se vår [dokumentationsportal](https://azure.microsoft.com/documentation/services/notification-hubs/) för fler plattformsoberoende exempel.</span><span class="sxs-lookup"><span data-stu-id="5f557-196">Check out our [documentation portal](https://azure.microsoft.com/documentation/services/notification-hubs/) for more cross-platform examples.</span></span>
> 
> 

1. <span data-ttu-id="5f557-197">I Visual Studio från hello **filen** väljer du **ny** och sedan **projekt**.</span><span class="sxs-lookup"><span data-stu-id="5f557-197">In Visual Studio, from hello **File** menu, select **New** and then **Project**.</span></span> <span data-ttu-id="5f557-198">Under **Visual C#** klickar du på **Windows** och **Konsolapp** och sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f557-198">Under **Visual C#**, click **Windows** and **Console Application**, and then click **OK**.</span></span>  <span data-ttu-id="5f557-199">Detta skapar ett nytt konsolappsprojekt.</span><span class="sxs-lookup"><span data-stu-id="5f557-199">This creates a new console application project.</span></span>
2. <span data-ttu-id="5f557-200">Från hello **verktyg** -menyn klickar du på **Library Package Manager** och sedan **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="5f557-200">From hello **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span> <span data-ttu-id="5f557-201">Detta visar hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="5f557-201">This displays hello Package Manager Console.</span></span>
3. <span data-ttu-id="5f557-202">Kör hello följande kommando i konsolfönstret hello:</span><span class="sxs-lookup"><span data-stu-id="5f557-202">In hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
       This adds a reference toohello Azure Service Bus SDK with hello <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>.
4. <span data-ttu-id="5f557-203">Öppna `Program.cs` och Lägg till följande hello `using` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="5f557-203">Open `Program.cs` and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="5f557-204">I hello `Program` klassen, Lägg till följande metod hello:</span><span class="sxs-lookup"><span data-stu-id="5f557-204">In hello `Program` class, add hello following method:</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }
   
       Make sure tooreplace hello `<hub name>` placeholder with hello name of hello notification hub that appears in hello [portal](https://portal.azure.com) in your Notification Hub blade. Also, replace hello connection string placeholder with hello connection string called `DefaultFullSharedAccessSignature` that you obtained in hello notification hub configuration section.
   
   > [!NOTE]
   > <span data-ttu-id="5f557-205">Kontrollera att du använder hello-anslutningssträngen med **fullständig** åt inte **lyssna** åtkomst.</span><span class="sxs-lookup"><span data-stu-id="5f557-205">Make sure that you use hello connection string with **Full** access, not **Listen** access.</span></span> <span data-ttu-id="5f557-206">Hej **lyssna** anslutningssträngen för åtkomst ger inte behörighet toosend push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5f557-206">hello **Listen** access connection string does not grant permissions toosend push notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="5f557-207">Lägg till följande hello anropar i hello `Main` metoden:</span><span class="sxs-lookup"><span data-stu-id="5f557-207">Add hello following calls in hello `Main` method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="5f557-208">Se till att Chrome körs och kör hello konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="5f557-208">Make sure that Chrome is running, and run hello console application.</span></span>
8. <span data-ttu-id="5f557-209">Du bör se hello följande meddelande popup på skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="5f557-209">You should see hello following notification pop up on your desktop.</span></span>
   
       ![Google Chrome - Notification][13]
9. <span data-ttu-id="5f557-210">Du kan också se alla meddelanden med hjälp av hello Chrome Notifications i Aktivitetsfältet hello (i Windows) när Chrome körs.</span><span class="sxs-lookup"><span data-stu-id="5f557-210">You can also see all your notifications by using hello Chrome Notifications window in hello taskbar (in Windows) when Chrome is running.</span></span>
   
       ![Google Chrome - Notifications List][14]

> [!NOTE]
> <span data-ttu-id="5f557-211">Du behöver inte toohave hello Chrome-appen körs eller öppna i webbläsare hello (även om hello själva webbläsaren Chrome måste köras).</span><span class="sxs-lookup"><span data-stu-id="5f557-211">You don't need toohave hello Chrome App running or open in hello browser (though hello Chrome browser itself must be running).</span></span> <span data-ttu-id="5f557-212">Du kan också få en samlad vy över alla meddelanden i hello Chrome meddelanden fönster.</span><span class="sxs-lookup"><span data-stu-id="5f557-212">You also get a consolidated view of all your notifications in hello Chrome Notifications window.</span></span>
> 
> 

## <span data-ttu-id="5f557-213"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5f557-213"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="5f557-214">Mer information om Notification Hubs finns i [Översikt över Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="5f557-214">Learn more about Notification Hubs in [Notification Hubs Overview].</span></span>

<span data-ttu-id="5f557-215">tootarget specifika användare, se toohello [Meddelandeanvändare för Azure Notification Hubs] kursen.</span><span class="sxs-lookup"><span data-stu-id="5f557-215">tootarget specific users, refer toohello [Azure Notification Hubs Notify Users] tutorial.</span></span> 

<span data-ttu-id="5f557-216">Om du vill toosegment in användarna efter intressegrupper, kan du följa hello [Azure Notification Hubs senaste nytt] kursen.</span><span class="sxs-lookup"><span data-stu-id="5f557-216">If you want toosegment your users by interest groups, you can follow hello [Azure Notification Hubs breaking news] tutorial.</span></span>

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Chrome App Notification Hub Sample]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Google Cloud-konsolen]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Översikt över Notification Hubs]: notification-hubs-push-notification-overview.md
[översikt över Chrome-appar]: https://developer.chrome.com/apps/about_apps
[Chrome App GCM Sample]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome-appar på mobila enheter]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Skapa Registration NH REST API]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[biblioteket crypto-js]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging för Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Meddelandeanvändare för Azure Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure Notification Hubs senaste nytt]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
