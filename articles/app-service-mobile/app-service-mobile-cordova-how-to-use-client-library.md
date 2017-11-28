---
title: "Hur du använder Apache Cordova-pluginprogrammet för Azure Mobile Apps"
description: "Hur du använder Apache Cordova-pluginprogrammet för Azure Mobile Apps"
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
ms.openlocfilehash: ebf0e911eeada0e529f908dd3e3430c94edae763
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a><span data-ttu-id="16cd8-103">Hur du använder Apache Cordova-klientbiblioteket för Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="16cd8-103">How to use Apache Cordova client library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="16cd8-104">Den här guiden lär du dig att utföra vanliga scenarier med senast [Apache Cordova-pluginprogrammet för Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="16cd8-104">This guide teaches you to perform common scenarios using the latest [Apache Cordova Plugin for Azure Mobile Apps].</span></span> <span data-ttu-id="16cd8-105">Om du har använt Azure Mobile Apps först slutföra [Azure Mobile Apps Snabbstart] skapa en tabell för att skapa en serverdel och ladda ned en förskapad Apache Cordova-projekt.</span><span class="sxs-lookup"><span data-stu-id="16cd8-105">If you are new to Azure Mobile Apps, first complete [Azure Mobile Apps Quick Start] to create a backend, create a table, and download a pre-built Apache Cordova project.</span></span> <span data-ttu-id="16cd8-106">I den här guiden fokuserar vi på klientsidan Apache Cordova-pluginprogrammet.</span><span class="sxs-lookup"><span data-stu-id="16cd8-106">In this guide, we focus on the client-side Apache Cordova Plugin.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="16cd8-107">Plattformar som stöds</span><span class="sxs-lookup"><span data-stu-id="16cd8-107">Supported platforms</span></span>
<span data-ttu-id="16cd8-108">Detta SDK stöder Apache Cordova v6.0.0 och senare på iOS, Android och Windows enheter.</span><span class="sxs-lookup"><span data-stu-id="16cd8-108">This SDK supports Apache Cordova v6.0.0 and later on iOS, Android, and Windows devices.</span></span>  <span data-ttu-id="16cd8-109">Plattformsstödet är följande:</span><span class="sxs-lookup"><span data-stu-id="16cd8-109">The platform support is as follows:</span></span>

* <span data-ttu-id="16cd8-110">Android API 19-24 (KitKat via Nougat).</span><span class="sxs-lookup"><span data-stu-id="16cd8-110">Android API 19-24 (KitKat through Nougat).</span></span>
* <span data-ttu-id="16cd8-111">iOS version 8.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="16cd8-111">iOS versions 8.0 and later.</span></span>
* <span data-ttu-id="16cd8-112">Windows Phone 8.1.</span><span class="sxs-lookup"><span data-stu-id="16cd8-112">Windows Phone 8.1.</span></span>
* <span data-ttu-id="16cd8-113">Universella Windows-plattformen.</span><span class="sxs-lookup"><span data-stu-id="16cd8-113">Universal Windows Platform.</span></span>

## <span data-ttu-id="16cd8-114"><a name="Setup"></a>Installationen och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="16cd8-114"><a name="Setup"></a>Setup and prerequisites</span></span>
<span data-ttu-id="16cd8-115">Den här handboken förutsätts att du har skapat en serverdel med en tabell.</span><span class="sxs-lookup"><span data-stu-id="16cd8-115">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="16cd8-116">Den här guiden förutsätter att tabellen har samma schema som tabellerna i dessa självstudier.</span><span class="sxs-lookup"><span data-stu-id="16cd8-116">This guide assumes that the table has the same schema as the tables in those tutorials.</span></span> <span data-ttu-id="16cd8-117">Den här handboken förutsätter också att du har lagt till Apache Cordova-pluginprogrammet din kod.</span><span class="sxs-lookup"><span data-stu-id="16cd8-117">This guide also assumes that you have added the Apache Cordova Plugin to your code.</span></span>  <span data-ttu-id="16cd8-118">Om du inte har gjort det, kan du lägga till Apache Cordova-pluginprogrammet projektet på kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="16cd8-118">If you have not done so, you may add the Apache Cordova plugin to your project on the command line:</span></span>

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

<span data-ttu-id="16cd8-119">Mer information om hur du skapar [din första Apache Cordova-app], finns i deras dokumentation.</span><span class="sxs-lookup"><span data-stu-id="16cd8-119">For more information on creating [your first Apache Cordova app], see their documentation.</span></span>

## <span data-ttu-id="16cd8-120"><a name="ionic"></a>Ställa in en joniska v2-app</span><span class="sxs-lookup"><span data-stu-id="16cd8-120"><a name="ionic"></a>Setting up an Ionic v2 app</span></span>

<span data-ttu-id="16cd8-121">Om du vill konfigurera ett joniska v2-projekt, skapa en grundläggande app och lägga till Cordova-pluginprogrammet:</span><span class="sxs-lookup"><span data-stu-id="16cd8-121">To properly configure an Ionic v2 project, first create a basic app and add the Cordova plugin:</span></span>

```
ionic start projectName --v2
cd projectName
ionic plugin add cordova-plugin-ms-azure-mobile-apps
```

<span data-ttu-id="16cd8-122">Lägg till följande rader till `app.component.ts` att skapa klienten objektet:</span><span class="sxs-lookup"><span data-stu-id="16cd8-122">Add the following lines to `app.component.ts` to create the client object:</span></span>

```
declare var WindowsAzure: any;
var client = new WindowsAzure.MobileServiceClient("https://yoursite.azurewebsites.net");
```

<span data-ttu-id="16cd8-123">Du kan nu skapa och köra projektet i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="16cd8-123">You can now build and run the project in the browser:</span></span>

```
ionic platform add browser
ionic run browser
```

<span data-ttu-id="16cd8-124">Azure Mobile Apps Cordova-pluginprogrammet stöder både joniska v1 och v2-appar.</span><span class="sxs-lookup"><span data-stu-id="16cd8-124">The Azure Mobile Apps Cordova plugin supports both Ionic v1 and v2 apps.</span></span>  <span data-ttu-id="16cd8-125">Endast joniska v2-appar kräver ytterligare deklarationen för den `WindowsAzure` objekt.</span><span class="sxs-lookup"><span data-stu-id="16cd8-125">Only the Ionic v2 apps require the additional declaration for the `WindowsAzure` object.</span></span>

[!INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]

## <span data-ttu-id="16cd8-126"><a name="auth"></a>Så här: autentisera användare</span><span class="sxs-lookup"><span data-stu-id="16cd8-126"><a name="auth"></a>How to: Authenticate users</span></span>
<span data-ttu-id="16cd8-127">Azure Apptjänst har stöd för autentisering och auktorisering av appanvändare som använder olika externa identitetsleverantörer: Facebook, Google, Account och Twitter.</span><span class="sxs-lookup"><span data-stu-id="16cd8-127">Azure App Service supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, and Twitter.</span></span> <span data-ttu-id="16cd8-128">Du kan ange behörigheter på tabeller för att begränsa åtkomsten för specifika åtgärder endast autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="16cd8-128">You can set permissions on tables to restrict access for specific operations to only authenticated users.</span></span> <span data-ttu-id="16cd8-129">Du kan också använda identiteten för autentiserade användare för att implementera auktoriseringsregler i server-skript.</span><span class="sxs-lookup"><span data-stu-id="16cd8-129">You can also use the identity of authenticated users to implement authorization rules in server scripts.</span></span> <span data-ttu-id="16cd8-130">Mer information finns i [komma igång med autentisering] kursen.</span><span class="sxs-lookup"><span data-stu-id="16cd8-130">For more information, see the [Get started with authentication] tutorial.</span></span>

<span data-ttu-id="16cd8-131">När du använder autentisering i en Apache Cordova-app måste följande Cordova-plugin-program vara tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="16cd8-131">When using authentication in an Apache Cordova app, the following Cordova plugins must be available:</span></span>

* <span data-ttu-id="16cd8-132">[cordova-plugin-enhet]</span><span class="sxs-lookup"><span data-stu-id="16cd8-132">[cordova-plugin-device]</span></span>
* <span data-ttu-id="16cd8-133">[cordova-plugin-inappbrowser]</span><span class="sxs-lookup"><span data-stu-id="16cd8-133">[cordova-plugin-inappbrowser]</span></span>

<span data-ttu-id="16cd8-134">Två autentisering flöden stöds: ett flöde för server och ett klient-flöde.</span><span class="sxs-lookup"><span data-stu-id="16cd8-134">Two authentication flows are supported: a server flow and a client flow.</span></span>  <span data-ttu-id="16cd8-135">Server-flöde ger enklaste autentiseringsupplevelse, eftersom det är beroende av leverantörens Webbgränssnitt för autentisering.</span><span class="sxs-lookup"><span data-stu-id="16cd8-135">The server flow provides the simplest authentication experience, as it relies on the provider's web authentication interface.</span></span> <span data-ttu-id="16cd8-136">Flödet tillåter djupare integrering med specifika funktioner som enkel inloggning som den förlitar sig på provider-specifik enhetsspecifika SDK.</span><span class="sxs-lookup"><span data-stu-id="16cd8-136">The client flow allows for deeper integration with device-specific capabilities such as single-sign-on as it relies on provider-specific device-specific SDKs.</span></span>

[!INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

### <span data-ttu-id="16cd8-137"><a name="configure-external-redirect-urls"></a>Så här: konfigurera din mobila App Service för externa omdirigerings-URL.</span><span class="sxs-lookup"><span data-stu-id="16cd8-137"><a name="configure-external-redirect-urls"></a>How to: Configure your Mobile App Service for external redirect URLs.</span></span>
<span data-ttu-id="16cd8-138">Flera typer av Apache Cordova-program använda en loopback-funktion för att hantera OAuth UI-flöden.</span><span class="sxs-lookup"><span data-stu-id="16cd8-138">Several types of Apache Cordova applications use a loopback capability to handle OAuth UI flows.</span></span>  <span data-ttu-id="16cd8-139">OAuth UI-flöden på localhost problem Eftersom Autentiseringstjänsten endast användas att använda tjänsten som standard.</span><span class="sxs-lookup"><span data-stu-id="16cd8-139">OAuth UI flows on localhost cause problems since the authentication service only knows how to utilize your service by default.</span></span>  <span data-ttu-id="16cd8-140">Exempel på problematiska OAuth UI-flöden är:</span><span class="sxs-lookup"><span data-stu-id="16cd8-140">Examples of problematic OAuth UI flows include:</span></span>

* <span data-ttu-id="16cd8-141">Flytta efterföljande emulatorn.</span><span class="sxs-lookup"><span data-stu-id="16cd8-141">The Ripple emulator.</span></span>
* <span data-ttu-id="16cd8-142">Läs in igen med joniska för Direktmigrering.</span><span class="sxs-lookup"><span data-stu-id="16cd8-142">Live Reload with Ionic.</span></span>
* <span data-ttu-id="16cd8-143">Kör mobila serverdel lokalt</span><span class="sxs-lookup"><span data-stu-id="16cd8-143">Running the mobile backend locally</span></span>
* <span data-ttu-id="16cd8-144">Mobila serverdel som körs i en annan Azure App Service än en tillhandahåller autentisering.</span><span class="sxs-lookup"><span data-stu-id="16cd8-144">Running the mobile backend in a different Azure App Service than the one providing authentication.</span></span>

<span data-ttu-id="16cd8-145">Följ instruktionerna för att lägga till dina lokala inställningar i konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="16cd8-145">Follow these instructions to add your local settings to the configuration:</span></span>

1. <span data-ttu-id="16cd8-146">Logga in på [Azure-portalen]</span><span class="sxs-lookup"><span data-stu-id="16cd8-146">Log in to the [Azure portal]</span></span>
2. <span data-ttu-id="16cd8-147">Välj **alla resurser** eller **Apptjänster** klicka sedan på namnet på din Mobilapp.</span><span class="sxs-lookup"><span data-stu-id="16cd8-147">Select **All resources** or **App Services** then click the name of your Mobile App.</span></span>
3. <span data-ttu-id="16cd8-148">Klicka på **verktyg**</span><span class="sxs-lookup"><span data-stu-id="16cd8-148">Click **Tools**</span></span>
4. <span data-ttu-id="16cd8-149">Klicka på **resursläsaren** i Följ-menyn och klicka på **Gå**.</span><span class="sxs-lookup"><span data-stu-id="16cd8-149">Click **Resource explorer** in the OBSERVE menu, then click **Go**.</span></span>  <span data-ttu-id="16cd8-150">Öppnar ett nytt fönster eller flik.</span><span class="sxs-lookup"><span data-stu-id="16cd8-150">A new window or tab opens.</span></span>
5. <span data-ttu-id="16cd8-151">Expandera den **config**, **authsettings** noder för din plats i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="16cd8-151">Expand the **config**, **authsettings** nodes for your site in the left-hand navigation.</span></span>
6. <span data-ttu-id="16cd8-152">Klicka på **redigera**</span><span class="sxs-lookup"><span data-stu-id="16cd8-152">Click **Edit**</span></span>
7. <span data-ttu-id="16cd8-153">Sök efter ”allowedExternalRedirectUrls”-elementet.</span><span class="sxs-lookup"><span data-stu-id="16cd8-153">Look for the "allowedExternalRedirectUrls" element.</span></span>  <span data-ttu-id="16cd8-154">Det kan ställas in på null eller en matris med värden.</span><span class="sxs-lookup"><span data-stu-id="16cd8-154">It may be set to null or an array of values.</span></span>  <span data-ttu-id="16cd8-155">Ändra värdet till följande värde:</span><span class="sxs-lookup"><span data-stu-id="16cd8-155">Change the value to the following value:</span></span>

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    <span data-ttu-id="16cd8-156">Ersätt URL: er med URL: er för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="16cd8-156">Replace the URLs with the URLs of your service.</span></span>  <span data-ttu-id="16cd8-157">Exempel är ”http://localhost: 3000” (för Node.js-exempel service) eller ”http://localhost:4400” (för små service).</span><span class="sxs-lookup"><span data-stu-id="16cd8-157">Examples include "http://localhost:3000" (for the Node.js sample service), or "http://localhost:4400" (for the Ripple service).</span></span>  <span data-ttu-id="16cd8-158">Dessa URL: er är dock exempel - sätt, inklusive för de tjänster som anges i exempel kan vara olika.</span><span class="sxs-lookup"><span data-stu-id="16cd8-158">However, these URLs are examples - your situation, including for the services mentioned in the examples, may be different.</span></span>
8. <span data-ttu-id="16cd8-159">Klicka på den **läsning och skrivning** knappen i det övre högra hörnet på skärmen.</span><span class="sxs-lookup"><span data-stu-id="16cd8-159">Click the **Read/Write** button in the top-right corner of the screen.</span></span>
9. <span data-ttu-id="16cd8-160">Klicka på gröna **PLACERA** knappen.</span><span class="sxs-lookup"><span data-stu-id="16cd8-160">Click the green **PUT** button.</span></span>

<span data-ttu-id="16cd8-161">Inställningarna sparas i det här läget.</span><span class="sxs-lookup"><span data-stu-id="16cd8-161">The settings are saved at this point.</span></span>  <span data-ttu-id="16cd8-162">Stäng inte webbläsarfönstret förrän inställningarna är klar sparar.</span><span class="sxs-lookup"><span data-stu-id="16cd8-162">Do not close the browser window until the settings have finished saving.</span></span>
<span data-ttu-id="16cd8-163">Lägg även till dessa loopback-URL: er CORS-inställningarna för din Apptjänst:</span><span class="sxs-lookup"><span data-stu-id="16cd8-163">Also add these loopback URLs to the CORS settings for your App Service:</span></span>

1. <span data-ttu-id="16cd8-164">Logga in på [Azure-portalen]</span><span class="sxs-lookup"><span data-stu-id="16cd8-164">Log in to the [Azure portal]</span></span>
2. <span data-ttu-id="16cd8-165">Välj **alla resurser** eller **Apptjänster** klicka sedan på namnet på din Mobilapp.</span><span class="sxs-lookup"><span data-stu-id="16cd8-165">Select **All resources** or **App Services** then click the name of your Mobile App.</span></span>
3. <span data-ttu-id="16cd8-166">Inställningsbladet öppnas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="16cd8-166">The Settings blade opens automatically.</span></span>  <span data-ttu-id="16cd8-167">Om den inte, klickar du på **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="16cd8-167">If it doesn't, click **All Settings**.</span></span>
4. <span data-ttu-id="16cd8-168">Klicka på **CORS** API-menyn.</span><span class="sxs-lookup"><span data-stu-id="16cd8-168">Click **CORS** under the API menu.</span></span>
5. <span data-ttu-id="16cd8-169">Ange den URL som du vill lägga till i rutan som visas och tryck RETUR.</span><span class="sxs-lookup"><span data-stu-id="16cd8-169">Enter the URL that you wish to add in the box provided and press Enter.</span></span>
6. <span data-ttu-id="16cd8-170">Ange ytterligare URL: er som behövs.</span><span class="sxs-lookup"><span data-stu-id="16cd8-170">Enter additional URLs as needed.</span></span>
7. <span data-ttu-id="16cd8-171">Klicka på **spara** spara inställningarna.</span><span class="sxs-lookup"><span data-stu-id="16cd8-171">Click **Save** to save the settings.</span></span>

<span data-ttu-id="16cd8-172">Det tar cirka 10 – 15 sekunder för de nya inställningarna ska börja gälla.</span><span class="sxs-lookup"><span data-stu-id="16cd8-172">It takes approximately 10-15 seconds for the new settings to take effect.</span></span>

## <span data-ttu-id="16cd8-173"><a name="register-for-push"></a>Så här: registrera för push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="16cd8-173"><a name="register-for-push"></a>How to: Register for push notifications</span></span>
<span data-ttu-id="16cd8-174">Installera den [phonegap-plugin-push] att hantera push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="16cd8-174">Install the [phonegap-plugin-push] to handle push notifications.</span></span>  <span data-ttu-id="16cd8-175">Det här plugin-programmet kan enkelt läggas till med hjälp av den `cordova plugin add` kommandot på kommandoraden eller via installationsprogrammet för Git-plugin-program i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16cd8-175">This plugin can be easily added using the `cordova plugin add` command on the command line, or via the Git plugin installer within Visual Studio.</span></span>  <span data-ttu-id="16cd8-176">Följande kod i din Apache Cordova-app registrerar din enhet för push-meddelanden:</span><span class="sxs-lookup"><span data-stu-id="16cd8-176">The following code in your Apache Cordova app registers your device for push notifications:</span></span>

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
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

<span data-ttu-id="16cd8-177">Använda Notification Hubs SDK för att skicka push-meddelanden från servern.</span><span class="sxs-lookup"><span data-stu-id="16cd8-177">Use the Notification Hubs SDK to send push notifications from the server.</span></span>  <span data-ttu-id="16cd8-178">Aldrig skicka push-meddelanden direkt från klienter.</span><span class="sxs-lookup"><span data-stu-id="16cd8-178">Never send push notifications directly from clients.</span></span> <span data-ttu-id="16cd8-179">Detta kan användas för att utlösa en denial of service-attack mot Meddelandehubbar och pns-systemet.</span><span class="sxs-lookup"><span data-stu-id="16cd8-179">Doing so could be used to trigger a denial of service attack against Notification Hubs or the PNS.</span></span>  <span data-ttu-id="16cd8-180">Pns-systemet kunde förbud trafiken till följd av sådana attacker.</span><span class="sxs-lookup"><span data-stu-id="16cd8-180">The PNS could ban your traffic as a result of such attacks.</span></span>

## <a name="more-information"></a><span data-ttu-id="16cd8-181">Mer information</span><span class="sxs-lookup"><span data-stu-id="16cd8-181">More information</span></span>

<span data-ttu-id="16cd8-182">Du hittar detaljerad information om API i vår [API-dokumentationen](http://azure.github.io/azure-mobile-apps-js-client/).</span><span class="sxs-lookup"><span data-stu-id="16cd8-182">You can find detailed API details in our [API documentation](http://azure.github.io/azure-mobile-apps-js-client/).</span></span>

<!-- URLs. -->
<span data-ttu-id="16cd8-183">[Azure-portalen]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="16cd8-183">[Azure portal]: https://portal.azure.com</span></span>
<span data-ttu-id="16cd8-184">[Azure Mobile Apps Snabbstart]: app-service-mobile-cordova-get-started.md</span><span class="sxs-lookup"><span data-stu-id="16cd8-184">[Azure Mobile Apps Quick Start]: app-service-mobile-cordova-get-started.md</span></span>
<span data-ttu-id="16cd8-185">[komma igång med autentisering]: app-service-mobile-cordova-get-started-users.md</span><span class="sxs-lookup"><span data-stu-id="16cd8-185">[Get started with authentication]: app-service-mobile-cordova-get-started-users.md</span></span>
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

<span data-ttu-id="16cd8-186">[Apache Cordova-pluginprogrammet för Azure Mobile Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps</span><span class="sxs-lookup"><span data-stu-id="16cd8-186">[Apache Cordova Plugin for Azure Mobile Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps</span></span>
<span data-ttu-id="16cd8-187">[din första Apache Cordova-app]: http://cordova.apache.org/#getstarted</span><span class="sxs-lookup"><span data-stu-id="16cd8-187">[your first Apache Cordova app]: http://cordova.apache.org/#getstarted</span></span>
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
<span data-ttu-id="16cd8-188">[phonegap-plugin-push]: https://www.npmjs.com/package/phonegap-plugin-push</span><span class="sxs-lookup"><span data-stu-id="16cd8-188">[phonegap-plugin-push]: https://www.npmjs.com/package/phonegap-plugin-push</span></span>
<span data-ttu-id="16cd8-189">[cordova-plugin-enhet]: https://www.npmjs.com/package/cordova-plugin-device</span><span class="sxs-lookup"><span data-stu-id="16cd8-189">[cordova-plugin-device]: https://www.npmjs.com/package/cordova-plugin-device</span></span>
<span data-ttu-id="16cd8-190">[cordova-plugin-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser</span><span class="sxs-lookup"><span data-stu-id="16cd8-190">[cordova-plugin-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser</span></span>
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
