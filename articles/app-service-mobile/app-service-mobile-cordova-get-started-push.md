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
# <a name="add-push-notifications-tooyour-apache-cordova-app"></a>Lägg till push-meddelanden tooyour Apache Cordova-app
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Översikt
I kursen får du lägga till push-meddelanden toohello [Apache Cordova Snabbstart]-projekt så att ett push-meddelande skickas toohello enheten varje gång en post infogas.

Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande. Mer information finns i [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps][1].

## <a name="prerequisites"></a>Förhandskrav
Den här kursen ingår en Apache Cordova-program som utvecklats med Visual Studio 2015 som körs på hello Google Android-emulatorn, en Android-enhet, en Windows-enhet och en iOS-enhet.

toocomplete den här kursen behöver du:

* En dator med [Visual Studio Community 2015] [ 2] eller senare versioner.
* [Visual Studio Tools för Apache Cordova][4].
* En [aktivt Azure-konto][3].
* En slutförd [Snabbstart för Apache Cordova] [ 5] projekt.
* (Android) En [Google-konto] [ 6] med en verifierad e-postadress.
* (iOS) En [Apple Developer Program medlemskap] [ 7] och en iOS-enhet (iOS-simulatorn stöder inte push).
* (Windows) En [Windows Store-utvecklarkonto] [ 8] och en Windows 10-enhet.

## <a name="configure-hub"></a>Konfigurera en meddelandehubb
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Se en video som visar stegen i det här avsnittet][9]

## <a name="update-hello-server-project"></a>Uppdatera hello serverprojekt
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="add-push-to-app"></a>Ändra din Cordova-app
Kontrollera din Apache Cordova-app-projekt är klar toohandle push-meddelanden genom att installera hello Cordova push-pluginprogrammet plus alla plattformsspecifika push-tjänster.

#### <a name="update-hello-cordova-version-in-your-project"></a>Uppdatera hello Cordova-version i projektet.
Om ditt projekt använder en tidigare version av Apache Cordova än v6.1.1, uppdatera hello klientprojekt. tooupdate hello projekt:

* Högerklicka på `config.xml` tooopen hello configuration designer.
* Välj fliken för hello-plattformar.
* Välj 6.1.1 i hello **Cordova CLI** textruta.
* Välj **skapa**, sedan **skapa lösning** tooupdate hello projektet.

#### <a name="install-hello-push-plugin"></a>Installera hello push plugin-programmet
Apache Cordova-program hanterar inte internt enhet eller nätverket funktioner.  Dessa funktioner som tillhandahålls av plugin-program som publicerats antingen på [npm] [ 10] eller på GitHub.  Hej `phonegap-plugin-push` plugin-programmet har använt toohandle nätverk push-meddelanden.

Du kan installera push-plugin-programmet hello i något av följande sätt:

**Från hello-kommandotolk:**

Kör följande kommando hello:

    cordova plugin add phonegap-plugin-push

**Från Visual Studio:**

1. I Solution Explorer öppnar du hello `config.xml` klickar du på filen **plugin-program** > **anpassad**väljer **Git** som installationskälla, ange `https://github.com/phonegap/phonegap-plugin-push`som hello källa.

   ![][img1]

2. Klicka på hello pilen Nästa toohello installationskälla.
3. I **SENDER_ID**, om du redan har ett numeriskt projekt-ID för hello Google Developer Console-projekt, du kan lägga till den här. Annars kan du ange en platshållarvärde, till exempel 777777.  Om du utvecklar för Android, kan du uppdatera det här värdet i config.xml senare.
4. Klicka på **Lägg till**.

hello push-plugin-programmet har installerats.

#### <a name="install-hello-device-plugin"></a>Installera hello enheten plugin-programmet
Följ hello samma procedur som du använde tooinstall hello push plugin-programmet.  Lägg till enhet-plugin-programmet hello hello Core plugin-program listan (klicka på **plugin-program** > **Core** toofind den). Du behöver det här plugin-programmet tooobtain hello plattformsnamnet.

#### <a name="register-your-device-on-application-start-up"></a>Registrera din enhet på programmet uppstart
Vi ta först minimal kod för Android. Senare ändra hello app toorun på iOS- eller Windows 10.

1. Lägg till ett anrop för**registerForPushNotifications** under hello återanrop för hello inloggningen eller längst hello hello **onDeviceReady** metod:

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

    Det här exemplet visar anropar **registerForPushNotifications** när autentiseringen lyckas.  Du kan anropa `registerForPushNotifications()` så ofta som krävs.

2. Lägga till nya hello **registerForPushNotifications** metoden på följande sätt:

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
3. (Android) Ersätt i hello föregående kod, `Your_Project_ID` med hello numeriska projektet ID för din app från den [Google Developer Console][18].

## <a name="optional-configure-and-run-hello-app-on-android"></a>(Valfritt) Konfigurera och köra hello app på Android
Slutför det här avsnittet tooenable push-meddelanden för Android.

#### <a name="enable-gcm"></a>Aktivera Firebase Cloud Messaging
Eftersom vi utvecklar hello Google Android-plattformen först, måste du aktivera Firebase Cloud Messaging.

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <a name="configure-backend"></a>Konfigurera hello Mobilapp backend toosend push-begäranden som använder FCM
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a>Konfigurera din Cordova-app för Android
Öppna config.xml i Cordova-app och Ersätt `Your_Project_ID` med hello numeriska projektet ID för din app från hello [Google Developer Console][18].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Öppna index.js och uppdatera hello kod toouse numeriska projekt-ID.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

#### <a name="configure-device"></a>Konfigurera din Android-enhet för USB-felsökning
Innan du kan distribuera ditt program tooyour Android-enhet behöver du tooenable USB-felsökning.  Utför följande steg på din Android-telefon:

1. Gå för**inställningar** > **om telefonen**, tryck på hello **Build-nummer** förrän utvecklarläge har aktiverats (ungefär sju gånger).
2. Tillbaka i **inställningar** > **utvecklaralternativ** aktivera **USB-felsökning**, Anslut din Android-telefon tooyour development dator med en USB-kabel.

Vi testade detta med hjälp av en Google Nexus 5 X-enhet som kör Android 6.0 (Marshmallow).  Hello tekniker är dock gemensamma för alla moderna Android-versionen.

#### <a name="install-google-play-services"></a>Installera Google Play-tjänster
push-plugin-programmet hello förlitar sig på Android Google Play-tjänster för push-meddelanden.

1. I Visual Studio klickar du på **verktyg** > **Android** > **Android SDK Manager**, expandera hello **tillägg** mappen och kontrollera hello rutan toomake att var och en av följande SDK hello har installerats.

   * Android 2.3 eller högre
   * Google databasen revision 27 eller högre
   * Google Play Services 9.0.2 eller högre

2. Klicka på **installationspaket** och vänta tills hello installation toocomplete.

hello aktuella krävs bibliotek visas i hello [phonegap-plugin-push-installation av dokumentationen][19].

#### <a name="test-push-notifications-in-hello-app-on-android"></a>Testa push-meddelanden i hello app på Android
Nu kan du testa push-meddelanden genom att köra hello appen och infoga objekt i hello TodoItem-tabell. Du kan testa från hello samma enhet eller från en annan enhet som du använder hello samma backend. Testa din Cordova-app på Android hello-plattformen i något av följande sätt hello:

* **På en fysisk enhet:** bifoga Android-enhet tooyour utvecklingsdator med en USB-kabel.  I stället för **Google Android-emulatorn**väljer **enhet**. Visual Studio distribuerar hello programmet toohello enheten och kör sedan programmet hello.  Du kan sedan interagera med hello programmet på hello enhet.

  Förbättra din utvecklingsmetod.  Dela program som skärm [Mobizen] [ 20] kan hjälpa dig att utveckla ett Android-program.  Mobizen projekt webbläsaren Android skärmen tooa på din dator.

* **På en Android-emulatorn:** finns ytterligare konfigurationssteg som krävs vid körning på en emulator.

    Kontrollera att du distribuerar tooa virtuell enhet som har Google APIs som hello mål, enligt hello Android Virtual Device (AVD)-hanteraren.

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Om du vill toouse snabbare x86 emulatorn du [installera drivrutinen för hello HAXM] [ 11] och konfigurera hello emulatorn toouse den.

    Lägg till en Google-konto toohello Android-enhet genom att klicka på **appar** > **inställningar** > **Lägg till konto**, följ sedan anvisningarna för hello.

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Kör hello todolist appen som innan och infoga ett nytt todo-objekt. Den här tiden kan visas en meddelandeikonen i meddelandefältet hello. Du kan öppna hello lådan tooview hello fullständig meddelandetext av hello-meddelande.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a>(Valfritt) Konfigurera och köra på iOS
Det här avsnittet handlar om att köra hello Cordova-projekt på iOS-enheter. Om du inte arbetar med iOS-enheter, kan du hoppa över det här avsnittet.

#### <a name="install-and-run-hello-ios-remote-build-agent-on-a-mac-or-cloud-service"></a>Installera och köra hello iOS remote skapa agenten på en Mac- eller molnet
Innan du kan köra en Cordova-app på iOS med Visual Studio, gå igenom hello steg i hello [iOS inställningsguiden] [ 12] tooinstall och kör hello remote skapa agenten.

Kontrollera att du kan skapa hello appen för iOS. hello är stegen i guiden hello obligatoriska toobuild för iOS från Visual Studio. Om du inte har en Mac, kan du skapa för iOS med hello remote build-agenten på en tjänst som MacInCloud. Mer information finns i [kör iOS-app i hello molnet][21].

> [!NOTE]
> XCode 7 eller senare är obligatoriska toouse hello push-plugin-programmet på iOS.

#### <a name="find-hello-id-toouse-as-your-app-id"></a>Hitta hello ID toouse som App-ID
Innan du registrerar appen för push-meddelanden, öppnar config.xml i Cordova-app, hitta hello `id` attributvärdet i hello widget element och kopiera den för senare användning. Hello-ID i hello följande XML, är `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
          version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Använd den här identifieraren senare, när du skapar ett App-ID på Apple developer-portalen. Om du skapar ett annat App-ID på hello developer-portalen måste du utföra några extra steg senare i den här kursen. hello-ID i widget-elementet måste matcha hello App-ID på hello developer-portalen.

#### <a name="register-hello-app-for-push-notifications-on-apples-developer-portal"></a>Registrera hello app för push-meddelanden på Apple developer-portalen
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Titta på en video som visar liknande steg](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-toosend-push-notifications"></a>Konfigurera Azure toosend push-meddelanden
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a>Kontrollera att din App-ID som matchar din Cordova-app
Om hello App-ID som du redan skapat i Apple Developer kontot matchar hello-ID för hello widget element i config.xml, kan du hoppa över det här steget. Men om hello-ID: N inte matchar ta hello följande steg:

1. Ta bort hello plattformar mappen i projektet.
2. Ta bort hello plugin-program-mappen i projektet.
3. Ta bort hello node_modules mappen i projektet.
4. Uppdatera hello ID-attribut för hello widget element i config.xml toouse hello App-ID som du skapade i Apple Developer-konto.
5. Återskapa projektet.

##### <a name="test-push-notifications-in-your-ios-app"></a>Testa push-meddelanden i iOS-app
1. I Visual Studio, se till att **iOS** väljs som mål för distribution av hello och välj sedan **enhet** toorun på den anslutna iOS-enheten.

    Du kan köra på en iOS-enheten ansluten tooyour PC med iTunes. hello iOS-simulatorn stöder inte push-meddelanden.

2. Tryck på hello **kör** knappen eller **F5** Visual Studio toobuild hello projektet och starta hello app i en iOS-enhet och klicka sedan på **OK** tooaccept push-meddelanden.

   > [!NOTE]
   > hello app begär bekräftelse för push-meddelanden under hello kör först.

3. Skriv en aktivitet i hello appen och klicka sedan på hello plus -ikonen.
4. Kontrollera att ett meddelande tas emot och sedan klicka på OK toodismiss hello-meddelande.

## <a name="optional-configure-and-run-on-windows"></a>(Valfritt) Konfigurera och köra i Windows
Det här avsnittet handlar om att köra hello Apache Cordova-app-projekt på Windows 10-enheter (hello PhoneGap push plugin stöds på Windows 10). Om du inte arbetar med Windows-enheter, kan du hoppa över det här avsnittet.

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a>Registrera din Windows-app för push-meddelanden med WNS
toouse hello Store alternativ i Visual Studio, Välj ett mål för Windows hello lösning plattformar listan som **Windows x64** eller **Windows x86** (undvika **Windows Platform** för push-meddelanden).

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Se en video som visar liknande steg][13]

#### <a name="configure-hello-notification-hub-for-wns"></a>Konfigurera hello meddelandehubb för WNS
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-toosupport-windows-push-notifications"></a>Konfigurera din Cordova-app toosupport Windows push-meddelanden
Öppna hello configuration designer (Högerklicka på config.xml och välj **Vydesigner**), Välj hello **Windows** , och välj **Windows 10** under **Windows använder Version**.

toosupport push-meddelanden i din standard (debug) versioner, öppna build.json fil. Kopiera ”version” configuration tooyour debug-konfiguration.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Efter uppdateringen hello bör hello build.json innehålla hello följande kod:

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

Skapa hello program och kontrollera att du har några fel. Klientappen bör nu registrera för hello meddelanden från hello mobilappsserverdel. Upprepa det här avsnittet för varje Windows-projekt i din lösning.

#### <a name="test-push-notifications-in-your-windows-app"></a>Testa push-meddelanden i Windows-appen
I Visual Studio, se till att en Windows-plattform som är markerad som mål för distribution av hello, **Windows x64** eller **Windows x86**. toorun hello-appen på en Windows 10-dator som värd för Visual Studio väljer **lokal dator**.

Tryck på hello kör knappen toobuild hello projektet och starta hello app.

Skriv ett namn för en ny todoitem i hello app och klicka sedan på hello plus -ikonen tooadd den.

Kontrollera att ett meddelande tas emot när hello-objektet har lagts till.

## <a name="next-steps"></a>Nästa steg
* Läs mer om [Meddelandehubbar] [ 17] toolearn om push-meddelanden.
* Om du inte redan har gjort det, fortsätter hello självstudiekursen [att lägga till autentisering] [ 14] tooyour Apache Cordova-app.

Lär dig hur toouse hello SDK: er.

* [Apache Cordova-SDK][15]
* [ASP.NET Server-SDK][1]
* [Node.js Server-SDK][16]

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
