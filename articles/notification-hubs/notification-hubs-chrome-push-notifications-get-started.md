---
title: Skicka push-meddelanden till Chrome-appar med Azure Notification Hubs | Microsoft Docs
description: "Lär dig hur du använder Azure Notification Hubs för att skicka push-meddelanden till en Chrome-app."
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
ms.openlocfilehash: 600b1b7e5f3987c9a0acc33b7049f7118442b931
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Skicka push-meddelanden till Chrome-appar med Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Det här avsnittet visar hur du använder Azure Notification Hubs för att skicka push-meddelanden till en Chrome-app. Meddelandena och appen används och visas i webbläsaren Google Chrome. I den här självstudiekursen kommer vi att skapa en Chrome-app som tar emot push-meddelanden genom att använda [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/). 

> [!NOTE]
> Du måste ha ett aktivt Azure-konto för att slutföra den här kursen. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den kostnadsfria utvärderingsversionen av Azure finns [Kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).
> 
> 

I den här självstudiekursen vägleds du igenom de grundläggande stegen för att aktivera push-meddelanden:

* [Aktivera Google Cloud Messaging](#register)
* [Konfigurera meddelandehubben](#configure-hub)
* [Anslut Chrome-appen till meddelandehubben](#connect-app)
* [Skicka ett push-meddelande till Chrome-appen](#send)
* [Ytterligare funktioner](#next-steps)

> [!NOTE]
> Push-meddelandena för Chrome-appar är inte generiska för avisering i webbläsare. De är specifika för webbläsarens utökningsbarhetsmodell (se [Översikt över Chrome-appar] för mer information). Chrome-appar kan köras i vanliga webbläsare på stationära och bärbara datorer men även på mobila enheter (Android och iOS) via Apache Cordova. Mer information finns i [Chrome-appar på mobila enheter].
> 
> 

Att konfigurera GCM och Azure Notification Hubs fungerar på exakt samma sätt som konfigurationen av Android. Detta eftersom [Google Cloud Messaging för Chrome] är inaktuell och samma GCM stöder nu både Android-enheter och Chrome-instanser.

## <a id="register"></a>Aktivera Google Cloud Messaging
1. Navigera till webbplatsen för [Google Cloud-konsolen] och logga in med dina användaruppgifter för Google-kontot. Klicka sedan på knappen **Skapa projekt**. Ange ett lämpligt **projektnamn** och klicka sedan på knappen **Skapa**.
   
       ![Google Cloud Console - Create Project][1]
2. Skriv ned **projektnumret** på **projekt**-sidan för projektet som du nyss skapade. Du kommer att använda detta som **GCM-avsändar-ID** i Chrome-appen för registreringen på GCM.
   
       ![Google Cloud Console - Project Number][2]
3. I den vänstra fönsterrutan klickar du på **API:er och aut** och sedan rullar du ned och klickar på växlingsknappen för att aktivera **Google Cloud Messaging för Android**. Du behöver inte aktivera **Google Cloud Messaging för Chrome**.
   
       ![Google Cloud Console - Server Key][3]
4. I den vänstra fönsterrutan klickar du på **Autentiseringsuppgifter** > **Skapa ny nyckel** > **Servernyckel** > **Skapa**.
   
       ![Google Cloud Console - Credentials][4]
5. Skriv ned informationen om serverns **API-nyckel**. Du kommer att konfigurera denna nyckel i din meddelandehubb i de kommande stegen. Den gör så att hubben kan skicka push-meddelanden till GCM.
   
       ![Google Cloud Console - API Key][5]

## <a id="configure-hub"></a>Konfigurera meddelandehubben
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6.   I bladet **Inställningar** väljer du **Notification Services** och sedan **Google (GCM)**. Ange API-nyckeln och spara.

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

## <a id="connect-app"></a>Anslut Chrome-appen till meddelandehubben
Din meddelandehubb har nu konfigurerats för att fungera med GCM och du har anslutningssträngar för att registrera din app för att både ta emot och skicka push-meddelanden. LK

### <a name="create-a-new-chrome-app"></a>Skapa en ny Chrome-App
Exemplet nedan baseras på [Chrome App GCM Sample] och använder det rekommenderade sättet att skapa en Chrome-app. Vi kommer särskilt att fokusera på de steg som är relaterade till Azure Notification Hubs. 

> [!NOTE]
> Vi rekommenderar att du hämtar källan för den här Chrome-appen från [Chrome App Notification Hub Sample].
> 
> 

Chrome-appen skapas med hjälp av JavaScript och du kan använda den textredigerare som du tycker bäst om för att skapa appen. Nedan visas hur den här Chrome-appen kommer att se ut.

![Google Chrome-app][15]

1. Skapa en mapp och kalla den för `ChromePushApp`. Naturligtvis är namnet godtyckligt. Om du döper mappen till något annat måste du ersätta sökvägen i alla nödvändiga kodsegment.
2. Hämta [biblioteket crypto-js] från den mapp som du skapade i det andra steget. Denna biblioteksmapp innehåller två undermappar: `components` och `rollups`.
3. Skapa en `manifest.json`-fil. Alla Chrome-appar backas upp av en manifestfil som innehåller app-metadata. Och viktigast av allt: den innehåller även alla behörigheter som beviljas för appen när användaren installerar den.
   
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
   
    Lägg märke till elementet `permissions`. Det anger att den här Chrome-appen kommer att kunna ta emot push-meddelanden från GCM. Det måste även ange URI:n för Azure Notification Hubs, där Chrome-appen kommer att göra ett REST-anrop för registrering.
    Vår exempelapp använder också en ikonfil, `gcm_128.png`, som du hittar vid den källa som återanvänds från det ursprungliga GCM-exemplet. Du kan ersätta den med vilken bild som helst som uppfyller [ikonkriterierna](https://developer.chrome.com/apps/manifest/icons).
4. Skapa en fil med namnet `background.js` med hjälp av följande kod:
   
        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }
   
        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.
   
          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);
   
          // Pop up a notification to show the GCM message.
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
   
        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);
   
    Detta är den fil som gör att HTML-fönstret för Chrome-appen öppnas (**register.html**). Den definierar också hanteraren **messageReceived** för hanteringen av inkommande push-meddelanden.
5. Skapa en fil med namnet `register.html`. Detta definierar användargränssnittet för Chrome-appen. 
   
   > [!NOTE]
   > I det här exemplet används **CryptoJS v3.1.2**. Om du har hämtat en annan version av biblioteket måste du ersätta versionen på ett lämpligt sätt i sökvägen `src`.
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
6. Skapa en fil med namnet `register.js` med hjälp av koden nedan. Den här filen anger skriptet bakom `register.html`. Chrome-apparna tillåter inte infogad körning och därför måste du skapa ett separat säkerhetskopieringsskript för ditt användargränssnitt.
   
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
   
          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }
   
        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;
   
          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }
   
          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;
   
          // Mark that the first-time registration is done.
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
   
          // Update the payload with the registration ID obtained earlier.
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
   
    Detta är de viktigaste parametrarna i skriptet ovan:
   
   * **window.onLoad** definierar knapptryckningshändelserna för de två knapparna i användargränssnittet. En står för registrering med GCM och den andra använder registrerings-ID:et som returneras efter GCM-registreringen för registrering med Azure Notification Hubs.
   * **updateLog** är den funktion som gör att vi kan hantera enkla loggningsfunktioner.
   * **registerWithGCM** är den första knapptryckningshanteraren. Den gör att anropet `chrome.gcm.register` till GCM registrerar den aktuella Chrome App-instansen.
   * **registerCallback** är den återanropsfunktion som anropas när registreringsanropet för GCM svarar.
   * **registerWithNH** är den andra knapptryckningshanteraren. Den utför registreringar med Notification Hubs. Den får `hubName` och `connectionString` (som användaren har angett) och utformar REST-API-anropet för Notification Hubs Registration.
   * **splitConnectionString** och **generateSaSToken** är hjälpprogram som representerar JavaScript-implementeringen för skapandeprocessen av SaS-token. Dessa måste användas i alla REST-API-anrop. Mer information finns i [Vanliga koncept](http://msdn.microsoft.com/library/dn495627.aspx).
   * **sendNHRegistrationRequest** är funktionen som gör ett HTTP-REST-anrop till Azure Notification Hubs.
   * **registrationPayload** definierar nyttolasten för registrering av XML. Mer information finns i [Skapa Registration NH REST API]. Vi uppdaterar registrerings-ID:t i den med det som togs emot från GCM.
   * **client** är en instans av **XMLHttpRequest** som vi använder för att utföra HTTP POST-begäran. Observera att vi uppdaterar rubriken `Authorization` med `sasToken`. Om detta anrop slutförs korrekt kommer den här Chrome App-instansen att registreras med Azure Notification Hubs.

Den övergripande mappstrukturen för det här projektet ska se ut ungefär så här: ![Google Chrome App – Mappstruktur][21]

### <a name="set-up-and-test-your-chrome-app"></a>Konfigurera och testa din Chrome-app
1. Öppna webbläsaren Chrome. Öppna **Chrome-tilläggen** och aktivera **utvecklarläget**.
   
       ![Google Chrome - Enable Developer Mode][16]
2. Klicka på **Ladda uppackat tillägg** och gå till mappen där du skapade filerna. Du kan också använda **Utvecklarverktyget för Chrome-appar och tillägg** om du vill. Det här verktyget är en Chrome-app i sig själv (som installerats från Chrome Web Store) och ger dig avancerade felsökningsfunktioner för utveckling av Chrome-appar.
   
       ![Google Chrome - Load Unpacked Extension][17]
3. Om Chrome-appen har skapats utan fel, kommer den att dyka upp.
   
       ![Google Chrome - Chrome App Display][18]
4. Ange det **projektnummer** som du tidigare fått från **Google Cloud-konsolen** som avsändar-ID. Klicka sedan på **Registrera med GCM**. Meddelandet **Registrering med GCM har genomförts** måste visas.
   
       ![Google Chrome - Chrome App Customization][19]
5. Ange **meddelandehubbens namn** och den **DefaultListenSharedAccessSignature** som du tidigare fått från portalen. Klicka sedan på **Registrera med Azure Notification Hub**. Meddelandet **Registreringen på Notification Hub har genomförts!** måste visas, liksom den detaljerade informationen om registreringssvaret, som innehåller registrerings-ID:et för Azure Notification Hubs.
   
       ![Google Chrome - Specify Notification Hub Details][20]  

## <a name="send"></a>Skicka ett meddelande till din Chrome-app
I testsyfte skickar vi Chrome-push-meddelanden med hjälp av en .NET-konsolapp. 

> [!NOTE]
> Du kan skicka push-meddelanden med Notification Hubs från alla serverdelar via vårt offentliga <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-gränssnitt</a>. Se vår [dokumentationsportal](https://azure.microsoft.com/documentation/services/notification-hubs/) för fler plattformsoberoende exempel.
> 
> 

1. I Visual Studio väljer du **Ny** från **Arkiv**-menyn, och sedan **Projekt**. Under **Visual C#** klickar du på **Windows** och **Konsolapp** och sedan på **OK**.  Detta skapar ett nytt konsolappsprojekt.
2. Klicka på **Library Package Manager** och sedan på **Package Manager-konsolen** från menyn **Verktyg**. Detta öppnar Package Manager-konsolen.
3. Kör följande kommando i konsolfönstret:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
       This adds a reference to the Azure Service Bus SDK with the <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>.
4. Öppna `Program.cs` och lägg till följande `using`-uttryck:
   
        using Microsoft.Azure.NotificationHubs;
5. Lägg till följande metod i `Program`-klassen:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }
   
       Make sure to replace the `<hub name>` placeholder with the name of the notification hub that appears in the [portal](https://portal.azure.com) in your Notification Hub blade. Also, replace the connection string placeholder with the connection string called `DefaultFullSharedAccessSignature` that you obtained in the notification hub configuration section.
   
   > [!NOTE]
   > Kontrollera att du använder anslutningssträngen med **fullständig** åtkomst, inte enbart åtkomst för att **lyssna**. Anslutningssträngen med **lyssna**-åtkomst ger inte behörighet att skicka push-meddelanden.
   > 
   > 
6. Lägg till följande anrop i metoden `Main`:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Kontrollera att Chrome körs och starta konsolappen.
8. Du bör se följande popup-meddelande på ditt skrivbord.
   
       ![Google Chrome - Notification][13]
9. Du kan även visa alla meddelanden med hjälp av fönstret Chrome Notifications i aktivitetsfältet (i Windows) när Chrome körs.
   
       ![Google Chrome - Notifications List][14]

> [!NOTE]
> Du behöver inte köra Chrome-appen eller ha den öppen i webbläsaren (även om själva webbläsaren Chrome måste köras). Du kan också få en samlad vy över alla meddelanden i fönstret Chrome Notifications.
> 
> 

## <a name="next-steps"> </a>Nästa steg
Mer information om Notification Hubs finns i [Översikt över Notification Hubs].

Om du vill rikta in dig mot specifika användare, kan du gå igenom självstudiekursen [Meddela användare via Azure Notification Hubs]. 

Om du vill dela in användarna efter intressegrupper, kan du gå till självstudiekursen [De senaste nyheterna via Azure Notification Hubs].

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
[Översikt över Chrome-appar]: https://developer.chrome.com/apps/about_apps
[Chrome App GCM Sample]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome-appar på mobila enheter]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Skapa Registration NH REST API]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[biblioteket crypto-js]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging för Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Meddela användare via Azure Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[De senaste nyheterna via Azure Notification Hubs]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
