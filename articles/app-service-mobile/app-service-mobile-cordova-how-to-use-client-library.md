---
title: "aaaHow tooUse Apache Cordova-pluginprogrammet för Azure Mobile Apps"
description: "Hur tooUse Apache Cordova-pluginprogrammet för Azure Mobile Apps"
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: a56a1ce4-de0c-4f3c-8763-66252c52aa59
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: d3e0639e6478c409132af25304a2fb0f28401e98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-apache-cordova-client-library-for-azure-mobile-apps"></a>Hur toouse Apache Cordova-klientbiblioteket för Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Den här guiden lär du dig tooperform vanliga scenarier med hjälp av hello senaste [Apache Cordova-pluginprogrammet för Azure Mobile Apps]. Om du är ny tooAzure Mobile Apps först slutföra [Azure Mobile Apps Snabbstart] toocreate en serverdel, skapa en tabell och ladda ned en förskapad Apache Cordova-projekt. I den här guiden vi fokusera på klientsidan av hello Apache Cordova-pluginprogrammet.

## <a name="supported-platforms"></a>Plattformar som stöds
Detta SDK stöder Apache Cordova v6.0.0 och senare på iOS, Android och Windows enheter.  Hej plattformsstödet är följande:

* Android API 19-24 (KitKat via Nougat).
* iOS version 8.0 och senare.
* Windows Phone 8.1.
* Universella Windows-plattformen.

## <a name="Setup"></a>Installationen och förutsättningar
Den här handboken förutsätts att du har skapat en serverdel med en tabell. Den här handboken utgår från tabellen hello hello samma schema som hello tabeller i dessa självstudier. Den här handboken förutsätter också att du har lagt till hello Apache Cordova-pluginprogrammet tooyour kod.  Om du inte har gjort det, kan du lägga till hello Apache Cordova-plugin-programmet tooyour projektet på hello kommandoraden:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

Mer information om hur du skapar [din första Apache Cordova-app], finns i deras dokumentation.

## <a name="ionic"></a>Ställa in en joniska v2-app

tooproperly konfigurerar joniska v2-projekt först skapa en grundläggande app och Lägg till hello Cordova-pluginprogrammet:

```
ionic start projectName --v2
cd projectName
ionic plugin add cordova-plugin-ms-azure-mobile-apps
```

Lägg till följande rader för hello`app.component.ts` toocreate hello objekt:

```
declare var WindowsAzure: any;
var client = new WindowsAzure.MobileServiceClient("https://yoursite.azurewebsites.net");
```

Du kan nu skapa och köra hello-projekt i hello webbläsare:

```
ionic platform add browser
ionic run browser
```

hello Azure Mobile Apps Cordova-pluginprogrammet stöder både joniska v1 och v2-appar.  Endast hello joniska v2 appar kräver ytterligare deklarationen för hello `WindowsAzure` objekt.

[!INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]

## <a name="auth"></a>Så här: autentisera användare
Azure Apptjänst har stöd för autentisering och auktorisering av appanvändare som använder olika externa identitetsleverantörer: Facebook, Google, Account och Twitter. Du kan ange behörigheter för tabeller toorestrict åtkomst för specifika åtgärder tooonly autentiserade användare. Du kan också använda hello identiteten för autentiserade användare tooimplement auktoriseringsregler i server-skript. Mer information finns i hello [komma igång med autentisering] kursen.

När du använder autentisering i en Apache Cordova-app måste hello följande Cordova-plugin-program vara tillgängliga:

* [cordova-plugin-enhet]
* [cordova-plugin-inappbrowser]

Två autentisering flöden stöds: ett flöde för server och ett klient-flöde.  hello server flödet ger hello enklaste autentiseringsupplevelse som använder hello providern Webbgränssnitt för autentisering. hello flödet tillåter djupare integrering med specifika funktioner som enkel inloggning som den förlitar sig på provider-specifik enhetsspecifika SDK.

[!INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="configure-external-redirect-urls"></a>Så här: konfigurera din mobila App Service för externa omdirigerings-URL.
Flera typer av Apache Cordova program använder en loopback-funktionen toohandle OAuth UI flödar.  OAuth UI-flöden på localhost orsaka problem Eftersom hello Autentiseringstjänsten endast vet hur tooutilize din tjänst som standard.  Exempel på problematiska OAuth UI-flöden är:

* hello små emulatorn.
* Läs in igen med joniska för Direktmigrering.
* Kör hello mobilserverdel lokalt
* Kör hello mobilserverdel i en annan Azure App Service än hello en tillhandahåller autentisering.

Följ dessa instruktioner tooadd lokala inställningar toohello konfigurationen:

1. Logga in toohello [Azure-portalen]
2. Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din Mobilapp.
3. Klicka på **verktyg**
4. Klicka på **resursläsaren** hello Följ sedan klicka på menyn **Gå**.  Öppnar ett nytt fönster eller flik.
5. Expandera hello **config**, **authsettings** noder för webbplatsen i hello vänstra navigeringsfönstret.
6. Klicka på **redigera**
7. Leta efter hello ”allowedExternalRedirectUrls” element.  Det kan ställas in toonull eller en matris med värden.  Ändra hello värdet toohello följande värde:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Ersätt hello URL: er med hello URL: er för din tjänst.  Exempel är ”http://localhost: 3000” (för hello Node.js exempel service) eller ”http://localhost:4400” (för hello små service).  Dessa URL: er är dock exempel - sätt, inklusive för hello-tjänster som anges i hello exempel kan vara olika.
8. Klicka på hello **läsning och skrivning** i hello övre högra hörnet av hello-skärmen.
9. Klicka på hello grön **PLACERA** knappen.

hello inställningarna sparas i det här läget.  Stäng inte webbläsarfönstret hello tills hello inställningar är klar sparar.
Lägg även till dessa loopback-URL: er toohello CORS-inställningarna för din Apptjänst:

1. Logga in toohello [Azure-portalen]
2. Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din Mobilapp.
3. hello inställningsbladet öppnas automatiskt.  Om den inte, klickar du på **alla inställningar**.
4. Klicka på **CORS** hello API-menyn.
5. Ange hello-URL som du vill tooadd i hello rutan och tryck på RETUR.
6. Ange ytterligare URL: er som behövs.
7. Klicka på **spara** toosave hello inställningar.

Det tar cirka 10 – 15 sekunder för hello nya inställningar tootake effekt.

## <a name="register-for-push"></a>Så här: registrera för push-meddelanden
Installera hello [phonegap-plugin-push] toohandle push-meddelanden.  Det här plugin-programmet kan enkelt läggas till med hjälp av den `cordova plugin add` kommandot på hello kommandoraden eller via hello Git-plugin-programmet installer i Visual Studio.  Följande kod i din Apache Cordova-app registrerar din enhet för push-meddelanden:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use hello device plugin toodetermine hello device
    // Best is toouse device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by hello PNS - check hello format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Använd hello Notification Hubs SDK toosend push-meddelanden från hello-servern.  Aldrig skicka push-meddelanden direkt från klienter. Göra så kan använda tootrigger en denial of service-attack mot Meddelandehubbar eller hello PNS.  Hej PNS kunde förbud trafiken till följd av sådana attacker.

## <a name="more-information"></a>Mer information

Du hittar detaljerad information om API i vår [API-dokumentationen](http://azure.github.io/azure-mobile-apps-js-client/).

<!-- URLs. -->
[Azure-portalen]: https://portal.azure.com
[Azure Mobile Apps Snabbstart]: app-service-mobile-cordova-get-started.md
[komma igång med autentisering]: app-service-mobile-cordova-get-started-users.md
[Add authentication tooyour app]: app-service-mobile-cordova-get-started-users.md

[Apache Cordova-pluginprogrammet för Azure Mobile Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[din första Apache Cordova-app]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap-plugin-push]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova-plugin-enhet]: https://www.npmjs.com/package/cordova-plugin-device
[cordova-plugin-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
