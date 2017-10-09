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
# <a name="how-toouse-apache-cordova-client-library-for-azure-mobile-apps"></a><span data-ttu-id="26115-103">Hur toouse Apache Cordova-klientbiblioteket för Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="26115-103">How toouse Apache Cordova client library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="26115-104">Den här guiden lär du dig tooperform vanliga scenarier med hjälp av hello senaste [Apache Cordova-pluginprogrammet för Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="26115-104">This guide teaches you tooperform common scenarios using hello latest [Apache Cordova Plugin for Azure Mobile Apps].</span></span> <span data-ttu-id="26115-105">Om du är ny tooAzure Mobile Apps först slutföra [Azure Mobile Apps Snabbstart] toocreate en serverdel, skapa en tabell och ladda ned en förskapad Apache Cordova-projekt.</span><span class="sxs-lookup"><span data-stu-id="26115-105">If you are new tooAzure Mobile Apps, first complete [Azure Mobile Apps Quick Start] toocreate a backend, create a table, and download a pre-built Apache Cordova project.</span></span> <span data-ttu-id="26115-106">I den här guiden vi fokusera på klientsidan av hello Apache Cordova-pluginprogrammet.</span><span class="sxs-lookup"><span data-stu-id="26115-106">In this guide, we focus on hello client-side Apache Cordova Plugin.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="26115-107">Plattformar som stöds</span><span class="sxs-lookup"><span data-stu-id="26115-107">Supported platforms</span></span>
<span data-ttu-id="26115-108">Detta SDK stöder Apache Cordova v6.0.0 och senare på iOS, Android och Windows enheter.</span><span class="sxs-lookup"><span data-stu-id="26115-108">This SDK supports Apache Cordova v6.0.0 and later on iOS, Android, and Windows devices.</span></span>  <span data-ttu-id="26115-109">Hej plattformsstödet är följande:</span><span class="sxs-lookup"><span data-stu-id="26115-109">hello platform support is as follows:</span></span>

* <span data-ttu-id="26115-110">Android API 19-24 (KitKat via Nougat).</span><span class="sxs-lookup"><span data-stu-id="26115-110">Android API 19-24 (KitKat through Nougat).</span></span>
* <span data-ttu-id="26115-111">iOS version 8.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="26115-111">iOS versions 8.0 and later.</span></span>
* <span data-ttu-id="26115-112">Windows Phone 8.1.</span><span class="sxs-lookup"><span data-stu-id="26115-112">Windows Phone 8.1.</span></span>
* <span data-ttu-id="26115-113">Universella Windows-plattformen.</span><span class="sxs-lookup"><span data-stu-id="26115-113">Universal Windows Platform.</span></span>

## <span data-ttu-id="26115-114"><a name="Setup"></a>Installationen och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="26115-114"><a name="Setup"></a>Setup and prerequisites</span></span>
<span data-ttu-id="26115-115">Den här handboken förutsätts att du har skapat en serverdel med en tabell.</span><span class="sxs-lookup"><span data-stu-id="26115-115">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="26115-116">Den här handboken utgår från tabellen hello hello samma schema som hello tabeller i dessa självstudier.</span><span class="sxs-lookup"><span data-stu-id="26115-116">This guide assumes that hello table has hello same schema as hello tables in those tutorials.</span></span> <span data-ttu-id="26115-117">Den här handboken förutsätter också att du har lagt till hello Apache Cordova-pluginprogrammet tooyour kod.</span><span class="sxs-lookup"><span data-stu-id="26115-117">This guide also assumes that you have added hello Apache Cordova Plugin tooyour code.</span></span>  <span data-ttu-id="26115-118">Om du inte har gjort det, kan du lägga till hello Apache Cordova-plugin-programmet tooyour projektet på hello kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="26115-118">If you have not done so, you may add hello Apache Cordova plugin tooyour project on hello command line:</span></span>

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

<span data-ttu-id="26115-119">Mer information om hur du skapar [din första Apache Cordova-app], finns i deras dokumentation.</span><span class="sxs-lookup"><span data-stu-id="26115-119">For more information on creating [your first Apache Cordova app], see their documentation.</span></span>

## <span data-ttu-id="26115-120"><a name="ionic"></a>Ställa in en joniska v2-app</span><span class="sxs-lookup"><span data-stu-id="26115-120"><a name="ionic"></a>Setting up an Ionic v2 app</span></span>

<span data-ttu-id="26115-121">tooproperly konfigurerar joniska v2-projekt först skapa en grundläggande app och Lägg till hello Cordova-pluginprogrammet:</span><span class="sxs-lookup"><span data-stu-id="26115-121">tooproperly configure an Ionic v2 project, first create a basic app and add hello Cordova plugin:</span></span>

```
ionic start projectName --v2
cd projectName
ionic plugin add cordova-plugin-ms-azure-mobile-apps
```

<span data-ttu-id="26115-122">Lägg till följande rader för hello`app.component.ts` toocreate hello objekt:</span><span class="sxs-lookup"><span data-stu-id="26115-122">Add hello following lines too`app.component.ts` toocreate hello client object:</span></span>

```
declare var WindowsAzure: any;
var client = new WindowsAzure.MobileServiceClient("https://yoursite.azurewebsites.net");
```

<span data-ttu-id="26115-123">Du kan nu skapa och köra hello-projekt i hello webbläsare:</span><span class="sxs-lookup"><span data-stu-id="26115-123">You can now build and run hello project in hello browser:</span></span>

```
ionic platform add browser
ionic run browser
```

<span data-ttu-id="26115-124">hello Azure Mobile Apps Cordova-pluginprogrammet stöder både joniska v1 och v2-appar.</span><span class="sxs-lookup"><span data-stu-id="26115-124">hello Azure Mobile Apps Cordova plugin supports both Ionic v1 and v2 apps.</span></span>  <span data-ttu-id="26115-125">Endast hello joniska v2 appar kräver ytterligare deklarationen för hello `WindowsAzure` objekt.</span><span class="sxs-lookup"><span data-stu-id="26115-125">Only hello Ionic v2 apps require the additional declaration for hello `WindowsAzure` object.</span></span>

[!INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]

## <span data-ttu-id="26115-126"><a name="auth"></a>Så här: autentisera användare</span><span class="sxs-lookup"><span data-stu-id="26115-126"><a name="auth"></a>How to: Authenticate users</span></span>
<span data-ttu-id="26115-127">Azure Apptjänst har stöd för autentisering och auktorisering av appanvändare som använder olika externa identitetsleverantörer: Facebook, Google, Account och Twitter.</span><span class="sxs-lookup"><span data-stu-id="26115-127">Azure App Service supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, and Twitter.</span></span> <span data-ttu-id="26115-128">Du kan ange behörigheter för tabeller toorestrict åtkomst för specifika åtgärder tooonly autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="26115-128">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="26115-129">Du kan också använda hello identiteten för autentiserade användare tooimplement auktoriseringsregler i server-skript.</span><span class="sxs-lookup"><span data-stu-id="26115-129">You can also use hello identity of authenticated users tooimplement authorization rules in server scripts.</span></span> <span data-ttu-id="26115-130">Mer information finns i hello [komma igång med autentisering] kursen.</span><span class="sxs-lookup"><span data-stu-id="26115-130">For more information, see hello [Get started with authentication] tutorial.</span></span>

<span data-ttu-id="26115-131">När du använder autentisering i en Apache Cordova-app måste hello följande Cordova-plugin-program vara tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="26115-131">When using authentication in an Apache Cordova app, hello following Cordova plugins must be available:</span></span>

* <span data-ttu-id="26115-132">[cordova-plugin-enhet]</span><span class="sxs-lookup"><span data-stu-id="26115-132">[cordova-plugin-device]</span></span>
* <span data-ttu-id="26115-133">[cordova-plugin-inappbrowser]</span><span class="sxs-lookup"><span data-stu-id="26115-133">[cordova-plugin-inappbrowser]</span></span>

<span data-ttu-id="26115-134">Två autentisering flöden stöds: ett flöde för server och ett klient-flöde.</span><span class="sxs-lookup"><span data-stu-id="26115-134">Two authentication flows are supported: a server flow and a client flow.</span></span>  <span data-ttu-id="26115-135">hello server flödet ger hello enklaste autentiseringsupplevelse som använder hello providern Webbgränssnitt för autentisering.</span><span class="sxs-lookup"><span data-stu-id="26115-135">hello server flow provides hello simplest authentication experience, as it relies on hello provider's web authentication interface.</span></span> <span data-ttu-id="26115-136">hello flödet tillåter djupare integrering med specifika funktioner som enkel inloggning som den förlitar sig på provider-specifik enhetsspecifika SDK.</span><span class="sxs-lookup"><span data-stu-id="26115-136">hello client flow allows for deeper integration with device-specific capabilities such as single-sign-on as it relies on provider-specific device-specific SDKs.</span></span>

[!INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

### <span data-ttu-id="26115-137"><a name="configure-external-redirect-urls"></a>Så här: konfigurera din mobila App Service för externa omdirigerings-URL.</span><span class="sxs-lookup"><span data-stu-id="26115-137"><a name="configure-external-redirect-urls"></a>How to: Configure your Mobile App Service for external redirect URLs.</span></span>
<span data-ttu-id="26115-138">Flera typer av Apache Cordova program använder en loopback-funktionen toohandle OAuth UI flödar.</span><span class="sxs-lookup"><span data-stu-id="26115-138">Several types of Apache Cordova applications use a loopback capability toohandle OAuth UI flows.</span></span>  <span data-ttu-id="26115-139">OAuth UI-flöden på localhost orsaka problem Eftersom hello Autentiseringstjänsten endast vet hur tooutilize din tjänst som standard.</span><span class="sxs-lookup"><span data-stu-id="26115-139">OAuth UI flows on localhost cause problems since hello authentication service only knows how tooutilize your service by default.</span></span>  <span data-ttu-id="26115-140">Exempel på problematiska OAuth UI-flöden är:</span><span class="sxs-lookup"><span data-stu-id="26115-140">Examples of problematic OAuth UI flows include:</span></span>

* <span data-ttu-id="26115-141">hello små emulatorn.</span><span class="sxs-lookup"><span data-stu-id="26115-141">hello Ripple emulator.</span></span>
* <span data-ttu-id="26115-142">Läs in igen med joniska för Direktmigrering.</span><span class="sxs-lookup"><span data-stu-id="26115-142">Live Reload with Ionic.</span></span>
* <span data-ttu-id="26115-143">Kör hello mobilserverdel lokalt</span><span class="sxs-lookup"><span data-stu-id="26115-143">Running hello mobile backend locally</span></span>
* <span data-ttu-id="26115-144">Kör hello mobilserverdel i en annan Azure App Service än hello en tillhandahåller autentisering.</span><span class="sxs-lookup"><span data-stu-id="26115-144">Running hello mobile backend in a different Azure App Service than hello one providing authentication.</span></span>

<span data-ttu-id="26115-145">Följ dessa instruktioner tooadd lokala inställningar toohello konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="26115-145">Follow these instructions tooadd your local settings toohello configuration:</span></span>

1. <span data-ttu-id="26115-146">Logga in toohello [Azure-portalen]</span><span class="sxs-lookup"><span data-stu-id="26115-146">Log in toohello [Azure portal]</span></span>
2. <span data-ttu-id="26115-147">Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din Mobilapp.</span><span class="sxs-lookup"><span data-stu-id="26115-147">Select **All resources** or **App Services** then click hello name of your Mobile App.</span></span>
3. <span data-ttu-id="26115-148">Klicka på **verktyg**</span><span class="sxs-lookup"><span data-stu-id="26115-148">Click **Tools**</span></span>
4. <span data-ttu-id="26115-149">Klicka på **resursläsaren** hello Följ sedan klicka på menyn **Gå**.</span><span class="sxs-lookup"><span data-stu-id="26115-149">Click **Resource explorer** in hello OBSERVE menu, then click **Go**.</span></span>  <span data-ttu-id="26115-150">Öppnar ett nytt fönster eller flik.</span><span class="sxs-lookup"><span data-stu-id="26115-150">A new window or tab opens.</span></span>
5. <span data-ttu-id="26115-151">Expandera hello **config**, **authsettings** noder för webbplatsen i hello vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="26115-151">Expand hello **config**, **authsettings** nodes for your site in hello left-hand navigation.</span></span>
6. <span data-ttu-id="26115-152">Klicka på **redigera**</span><span class="sxs-lookup"><span data-stu-id="26115-152">Click **Edit**</span></span>
7. <span data-ttu-id="26115-153">Leta efter hello ”allowedExternalRedirectUrls” element.</span><span class="sxs-lookup"><span data-stu-id="26115-153">Look for hello "allowedExternalRedirectUrls" element.</span></span>  <span data-ttu-id="26115-154">Det kan ställas in toonull eller en matris med värden.</span><span class="sxs-lookup"><span data-stu-id="26115-154">It may be set toonull or an array of values.</span></span>  <span data-ttu-id="26115-155">Ändra hello värdet toohello följande värde:</span><span class="sxs-lookup"><span data-stu-id="26115-155">Change hello value toohello following value:</span></span>

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    <span data-ttu-id="26115-156">Ersätt hello URL: er med hello URL: er för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="26115-156">Replace hello URLs with hello URLs of your service.</span></span>  <span data-ttu-id="26115-157">Exempel är ”http://localhost: 3000” (för hello Node.js exempel service) eller ”http://localhost:4400” (för hello små service).</span><span class="sxs-lookup"><span data-stu-id="26115-157">Examples include "http://localhost:3000" (for hello Node.js sample service), or "http://localhost:4400" (for hello Ripple service).</span></span>  <span data-ttu-id="26115-158">Dessa URL: er är dock exempel - sätt, inklusive för hello-tjänster som anges i hello exempel kan vara olika.</span><span class="sxs-lookup"><span data-stu-id="26115-158">However, these URLs are examples - your situation, including for hello services mentioned in hello examples, may be different.</span></span>
8. <span data-ttu-id="26115-159">Klicka på hello **läsning och skrivning** i hello övre högra hörnet av hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="26115-159">Click hello **Read/Write** button in hello top-right corner of hello screen.</span></span>
9. <span data-ttu-id="26115-160">Klicka på hello grön **PLACERA** knappen.</span><span class="sxs-lookup"><span data-stu-id="26115-160">Click hello green **PUT** button.</span></span>

<span data-ttu-id="26115-161">hello inställningarna sparas i det här läget.</span><span class="sxs-lookup"><span data-stu-id="26115-161">hello settings are saved at this point.</span></span>  <span data-ttu-id="26115-162">Stäng inte webbläsarfönstret hello tills hello inställningar är klar sparar.</span><span class="sxs-lookup"><span data-stu-id="26115-162">Do not close hello browser window until hello settings have finished saving.</span></span>
<span data-ttu-id="26115-163">Lägg även till dessa loopback-URL: er toohello CORS-inställningarna för din Apptjänst:</span><span class="sxs-lookup"><span data-stu-id="26115-163">Also add these loopback URLs toohello CORS settings for your App Service:</span></span>

1. <span data-ttu-id="26115-164">Logga in toohello [Azure-portalen]</span><span class="sxs-lookup"><span data-stu-id="26115-164">Log in toohello [Azure portal]</span></span>
2. <span data-ttu-id="26115-165">Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din Mobilapp.</span><span class="sxs-lookup"><span data-stu-id="26115-165">Select **All resources** or **App Services** then click hello name of your Mobile App.</span></span>
3. <span data-ttu-id="26115-166">hello inställningsbladet öppnas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="26115-166">hello Settings blade opens automatically.</span></span>  <span data-ttu-id="26115-167">Om den inte, klickar du på **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="26115-167">If it doesn't, click **All Settings**.</span></span>
4. <span data-ttu-id="26115-168">Klicka på **CORS** hello API-menyn.</span><span class="sxs-lookup"><span data-stu-id="26115-168">Click **CORS** under hello API menu.</span></span>
5. <span data-ttu-id="26115-169">Ange hello-URL som du vill tooadd i hello rutan och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="26115-169">Enter hello URL that you wish tooadd in hello box provided and press Enter.</span></span>
6. <span data-ttu-id="26115-170">Ange ytterligare URL: er som behövs.</span><span class="sxs-lookup"><span data-stu-id="26115-170">Enter additional URLs as needed.</span></span>
7. <span data-ttu-id="26115-171">Klicka på **spara** toosave hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="26115-171">Click **Save** toosave hello settings.</span></span>

<span data-ttu-id="26115-172">Det tar cirka 10 – 15 sekunder för hello nya inställningar tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="26115-172">It takes approximately 10-15 seconds for hello new settings tootake effect.</span></span>

## <span data-ttu-id="26115-173"><a name="register-for-push"></a>Så här: registrera för push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="26115-173"><a name="register-for-push"></a>How to: Register for push notifications</span></span>
<span data-ttu-id="26115-174">Installera hello [phonegap-plugin-push] toohandle push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="26115-174">Install hello [phonegap-plugin-push] toohandle push notifications.</span></span>  <span data-ttu-id="26115-175">Det här plugin-programmet kan enkelt läggas till med hjälp av den `cordova plugin add` kommandot på hello kommandoraden eller via hello Git-plugin-programmet installer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26115-175">This plugin can be easily added using the `cordova plugin add` command on hello command line, or via hello Git plugin installer within Visual Studio.</span></span>  <span data-ttu-id="26115-176">Följande kod i din Apache Cordova-app registrerar din enhet för push-meddelanden:</span><span class="sxs-lookup"><span data-stu-id="26115-176">The following code in your Apache Cordova app registers your device for push notifications:</span></span>

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

<span data-ttu-id="26115-177">Använd hello Notification Hubs SDK toosend push-meddelanden från hello-servern.</span><span class="sxs-lookup"><span data-stu-id="26115-177">Use hello Notification Hubs SDK toosend push notifications from hello server.</span></span>  <span data-ttu-id="26115-178">Aldrig skicka push-meddelanden direkt från klienter.</span><span class="sxs-lookup"><span data-stu-id="26115-178">Never send push notifications directly from clients.</span></span> <span data-ttu-id="26115-179">Göra så kan använda tootrigger en denial of service-attack mot Meddelandehubbar eller hello PNS.</span><span class="sxs-lookup"><span data-stu-id="26115-179">Doing so could be used tootrigger a denial of service attack against Notification Hubs or hello PNS.</span></span>  <span data-ttu-id="26115-180">Hej PNS kunde förbud trafiken till följd av sådana attacker.</span><span class="sxs-lookup"><span data-stu-id="26115-180">hello PNS could ban your traffic as a result of such attacks.</span></span>

## <a name="more-information"></a><span data-ttu-id="26115-181">Mer information</span><span class="sxs-lookup"><span data-stu-id="26115-181">More information</span></span>

<span data-ttu-id="26115-182">Du hittar detaljerad information om API i vår [API-dokumentationen](http://azure.github.io/azure-mobile-apps-js-client/).</span><span class="sxs-lookup"><span data-stu-id="26115-182">You can find detailed API details in our [API documentation](http://azure.github.io/azure-mobile-apps-js-client/).</span></span>

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
