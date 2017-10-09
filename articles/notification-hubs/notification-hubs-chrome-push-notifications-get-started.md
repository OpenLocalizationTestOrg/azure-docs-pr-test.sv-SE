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
# <a name="send-push-notifications-toochrome-apps-with-azure-notification-hubs"></a>Skicka push-meddelanden tooChrome appar med Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Det här avsnittet visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa Chrome-App som kommer att visas i hello kontexten för hello webbläsaren Google Chrome. I den här självstudiekursen kommer vi att skapa en Chrome-app som tar emot push-meddelanden genom att använda [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/). 

> [!NOTE]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).
> 
> 

hello självstudiekursen vägleder dig igenom de här stegen tooenable push-meddelanden:

* [Aktivera Google Cloud Messaging](#register)
* [Konfigurera meddelandehubben](#configure-hub)
* [Ansluta din Chrome-appen toohello notification hub](#connect-app)
* [Skicka ett push-meddelande tooyour Chrome-appen](#send)
* [Ytterligare funktioner](#next-steps)

> [!NOTE]
> Chrome app push-meddelanden är inte generisk i webbläsaren meddelanden - de specifika toohello webbläsarens utökningsbarhetsmodell (se [översikt över Chrome-appar] information). Dessutom toohello skrivbord webbläsaren Chrome-appar köra på mobila enheter (Android och iOS) via Apache Cordova. Se [Chrome-appar på mobila enheter] toolearn mer.
> 
> 

Konfigurera GCM och Azure Notification Hubs är identiska tooconfiguring för Android, eftersom [Google Cloud Messaging för Chrome] är inaktuell och hello samma GCM stöder nu både Android-enheter och Chrome-instanser.

## <a id="register"></a>Aktivera Google Cloud Messaging
1. Navigera toohello [Google Cloud-konsolen] webbplats, logga in med dina Google-konto och klicka sedan på hello **skapa projekt** knappen. Ange ett lämpligt **projektnamn**, och klicka sedan på hello **skapa** knappen.
   
       ![Google Cloud Console - Create Project][1]
2. Anteckna hello **projektnumret** på hello **projekt** för hello-projekt som du nyss skapade. Du kan använda den som hello **GCM-avsändar-ID** i hello Chrome-appen tooregister med GCM.
   
       ![Google Cloud Console - Project Number][2]
3. Hello vänster klickar du på **API: er och aut**, bläddrar sedan nedåt och klicka på hello växla tooenable **Google Cloud Messaging för Android**. Du har inte tooenable **Google Cloud Messaging för Chrome**.
   
       ![Google Cloud Console - Server Key][3]
4. Hello vänster klickar du på **autentiseringsuppgifter** > **Skapa ny nyckel** > **servernyckel** > **skapa**.
   
       ![Google Cloud Console - Credentials][4]
5. Anteckna hello server **API-nyckeln**. Du konfigurerar det i ditt notification hub nästa tooenable den toosend push-meddelanden tooGCM.
   
       ![Google Cloud Console - API Key][5]

## <a id="configure-hub"></a>Konfigurera meddelandehubben
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6.   I hello **inställningar** bladet väljer **Notification Services** och sedan **Google (GCM)**. Ange hello API-nyckeln och spara.

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

## <a id="connect-app"></a>Ansluta din Chrome-appen toohello notification hub
Din meddelandehubb har nu konfigurerat toowork med GCM och du har hello anslutning strängar tooregister app-tooboth ta emot och skicka push-meddelanden. LK

### <a name="create-a-new-chrome-app"></a>Skapa en ny Chrome-App
hello exemplet nedan baseras på hello [Chrome App GCM Sample] och använder hello rekommenderade sätt toocreate en Chrome-App. Vi betona hello steg specifikt relaterade tooAzure Notification Hubs. 

> [!NOTE]
> Vi rekommenderar att du hämtar hello källa för den här Chrome-appen från [Chrome App Notification Hub Sample].
> 
> 

hello Chrome-appen skapas via JavaScript och du kan använda någon av textredigerare för att skapa den. Nedan visas hur den här Chrome-appen kommer att se ut.

![Google Chrome-app][15]

1. Skapa en mapp och kalla den för `ChromePushApp`. Naturligtvis hello namn är godtycklig - om du döper mappen till något annat, kontrollerar du att du ersätta hello sökväg i hello krävs kodsegment.
2. Hämta hello [biblioteket crypto-js] i hello-mappen som du skapade i hello andra steget. Denna biblioteksmapp innehåller två undermappar: `components` och `rollups`.
3. Skapa en `manifest.json`-fil. Alla Chrome-appar backas upp av en manifestfil som innehåller hello appmetadata. och viktigast av allt alla behörigheter som beviljas toohello app när hello användaren installerar den.
   
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
   
    Meddelande hello `permissions` element, som anger att den här Chrome-appen kommer att kunna tooreceive push-meddelanden från GCM. Det måste även ange hello Azure Notification Hubs URI där hello Chrome-appen blir tooregister en REST-anrop.
    Vår exempelapp använder också en ikonfil `gcm_128.png`, som du hittar vid hello-källa som återanvänds från hello ursprungliga GCM-exemplet. Du kan ersätta den med vilken bild som passar hello [ikonkriterierna](https://developer.chrome.com/apps/manifest/icons).
4. Skapa en fil med namnet `background.js` med hello följande kod:
   
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
   
    Detta är hello-fil som öppnas hello Chrome-appen fönstret HTML (**register.html**) och definierar också hanteraren hello **messageReceived** toohandle hello inkommande push-meddelanden.
5. Skapa en fil med namnet `register.html` -detta definierar hello Användargränssnittet för hello Chrome-appen. 
   
   > [!NOTE]
   > I det här exemplet används **CryptoJS v3.1.2**. Om du har hämtat en annan version av hello bibliotek Se till att du ska ersätta hello versionen i hello `src` sökväg.
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
6. Skapa en fil med namnet `register.js` med hello koden nedan. Den här filen anger hello skriptet bakom `register.html`. Chrome-appar tillåter inte infogad körning, så att du har toocreate ett separat Säkerhetskopieringsskript för ditt användargränssnitt.
   
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
   
    hello ovan skript har följande nyckelparametrar hello:
   
   * **window.onLoad** definierar knapptryckningshändelserna för hello hello två knappar på hello Användargränssnittet. En står för registrering med GCM och hello andra använder hello registrerings-ID som returneras efter registrering med GCM tooregister med Azure Notification Hubs.
   * **updateLog** hello-funktion som gör att vi toohandle enkla loggningsfunktioner.
   * **registerWithGCM** hello första klickar på knappen hanterare, vilket gör hello `chrome.gcm.register` anropet tooGCM tooregister hello aktuella Chrome App-instansen.
   * **registerCallback** är hello Återanropsfunktionen som anropas när hello GCM registrering anropet returnerar.
   * **registerWithNH** hello andra klickar på knappen hanterare, som registrerar med Notification Hubs. Hämtar `hubName` och `connectionString` (vilka hello-användaren har angett) och dekorationer hello Notification Hubs Registration REST API-anrop.
   * **splitConnectionString** och **generateSaSToken** är hjälpprogram som representerar hello JavaScript-implementeringen av en SaS-token processen, som måste användas i alla REST API-anrop. Mer information finns i [Vanliga koncept](http://msdn.microsoft.com/library/dn495627.aspx).
   * **sendNHRegistrationRequest** är hello-funktionen som gör att en HTTP-REST anropa tooAzure Notification Hubs.
   * **registrationPayload** definierar nyttolasten för hello registrering XML. Mer information finns i [Skapa Registration NH REST API]. Vi uppdaterar hello registrerings-ID i den med det som togs emot från GCM.
   * **klienten** är en instans av **XMLHttpRequest** att vi använder toomake hello HTTP POST-begäran. Observera att vi uppdaterar hello `Authorization` huvud med `sasToken`. Om detta anrop slutförs korrekt kommer den här Chrome App-instansen att registreras med Azure Notification Hubs.

hello övergripande mappstrukturen för det här projektet ska se ut ungefär så här: ![Google Chrome App – mappstruktur][21]

### <a name="set-up-and-test-your-chrome-app"></a>Konfigurera och testa din Chrome-app
1. Öppna webbläsaren Chrome. Öppna **Chrome-tilläggen** och aktivera **utvecklarläget**.
   
       ![Google Chrome - Enable Developer Mode][16]
2. Klicka på **ladda uppackat tillägg** och navigera toohello mappen där du skapade hello-filer. Du kan också använda hello **Chrome-appar och tillägg Utvecklarverktyget**. Det här verktyget är en Chrome-App i sig själv (som installerats från hello Chrome Web Store) och ger avancerade felsökningsfunktioner för utveckling av Chrome-appen.
   
       ![Google Chrome - Load Unpacked Extension][17]
3. Om hello Chrome-appen har skapats utan fel, ska du se Chrome-appen visas.
   
       ![Google Chrome - Chrome App Display][18]
4. Ange hello **projektnumret** som du tidigare fått från hello **Google Cloud-konsolen** som hello avsändar-ID och klicka på **registrera med GCM**. Du måste läsa hello-meddelande **registrering med GCM har genomförts.**
   
       ![Google Chrome - Chrome App Customization][19]
5. Ange din **Meddelandehubbsnamn** och hello **DefaultListenSharedAccessSignature** du fick från hello portal tidigare och klicka på **registrera med Azure Notification Hub**. Du måste läsa hello-meddelande **registreringen på Notification Hub lyckades!** och hello information om hello registreringssvar som innehåller hello Azure Notification Hubs registration-ID.
   
       ![Google Chrome - Specify Notification Hub Details][20]  

## <a name="send"></a>Skicka ett meddelande tooyour Chrome-appen
I testsyfte skickar vi Chrome-push-meddelanden med hjälp av en .NET-konsolapp. 

> [!NOTE]
> Du kan skicka push-meddelanden med Notification Hubs från alla serverdelar via vårt offentliga <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-gränssnitt</a>. Se vår [dokumentationsportal](https://azure.microsoft.com/documentation/services/notification-hubs/) för fler plattformsoberoende exempel.
> 
> 

1. I Visual Studio från hello **filen** väljer du **ny** och sedan **projekt**. Under **Visual C#** klickar du på **Windows** och **Konsolapp** och sedan på **OK**.  Detta skapar ett nytt konsolappsprojekt.
2. Från hello **verktyg** -menyn klickar du på **Library Package Manager** och sedan **Pakethanterarkonsolen**. Detta visar hello Package Manager-konsolen.
3. Kör hello följande kommando i konsolfönstret hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
       This adds a reference toohello Azure Service Bus SDK with hello <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>.
4. Öppna `Program.cs` och Lägg till följande hello `using` instruktionen:
   
        using Microsoft.Azure.NotificationHubs;
5. I hello `Program` klassen, Lägg till följande metod hello:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }
   
       Make sure tooreplace hello `<hub name>` placeholder with hello name of hello notification hub that appears in hello [portal](https://portal.azure.com) in your Notification Hub blade. Also, replace hello connection string placeholder with hello connection string called `DefaultFullSharedAccessSignature` that you obtained in hello notification hub configuration section.
   
   > [!NOTE]
   > Kontrollera att du använder hello-anslutningssträngen med **fullständig** åt inte **lyssna** åtkomst. Hej **lyssna** anslutningssträngen för åtkomst ger inte behörighet toosend push-meddelanden.
   > 
   > 
6. Lägg till följande hello anropar i hello `Main` metoden:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Se till att Chrome körs och kör hello konsolprogram.
8. Du bör se hello följande meddelande popup på skrivbordet.
   
       ![Google Chrome - Notification][13]
9. Du kan också se alla meddelanden med hjälp av hello Chrome Notifications i Aktivitetsfältet hello (i Windows) när Chrome körs.
   
       ![Google Chrome - Notifications List][14]

> [!NOTE]
> Du behöver inte toohave hello Chrome-appen körs eller öppna i webbläsare hello (även om hello själva webbläsaren Chrome måste köras). Du kan också få en samlad vy över alla meddelanden i hello Chrome meddelanden fönster.
> 
> 

## <a name="next-steps"> </a>Nästa steg
Mer information om Notification Hubs finns i [Översikt över Notification Hubs].

tootarget specifika användare, se toohello [Meddelandeanvändare för Azure Notification Hubs] kursen. 

Om du vill toosegment in användarna efter intressegrupper, kan du följa hello [Azure Notification Hubs senaste nytt] kursen.

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
