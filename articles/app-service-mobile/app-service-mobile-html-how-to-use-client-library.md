---
title: "aaaHow tooUse hello JavaScript SDK för Azure Mobile Apps"
description: "Hur tooUse v för Azure Mobile Apps"
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 53b78965-caa3-4b22-bb67-5bd5c19d03c4
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 3fcbb0c5bd6918a285bdafa1946ba0bd47bb21b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-javascript-client-library-for-azure-mobile-apps"></a><span data-ttu-id="71c49-103">Hur tooUse hello JavaScript-klientbiblioteket för Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="71c49-103">How tooUse hello JavaScript client library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="71c49-104">Den här guiden lär du dig tooperform vanliga scenarier med hjälp av hello senaste [JavaScript SDK för Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="71c49-104">This guide teaches you tooperform common scenarios using hello latest [JavaScript SDK for Azure Mobile Apps].</span></span> <span data-ttu-id="71c49-105">Om du är ny tooAzure Mobile Apps först slutföra [Azure Mobile Apps Snabbstart] toocreate en serverdel och skapa en tabell.</span><span class="sxs-lookup"><span data-stu-id="71c49-105">If you are new tooAzure Mobile Apps, first complete [Azure Mobile Apps Quick Start] toocreate a backend and create a table.</span></span> <span data-ttu-id="71c49-106">I den här guiden fokuserar vi på med hello mobilserverdel i HTML/JavaScript-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="71c49-106">In this guide, we focus on using hello mobile backend in HTML/JavaScript Web applications.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="71c49-107">Plattformar som stöds</span><span class="sxs-lookup"><span data-stu-id="71c49-107">Supported platforms</span></span>
<span data-ttu-id="71c49-108">Vi begränsa webbläsare stöd toohello aktuella och senaste versioner av hello större webbläsare: Google Chrome, Microsoft Edge, Microsoft Internet Explorer och Mozilla Firefox.</span><span class="sxs-lookup"><span data-stu-id="71c49-108">We limit browser support toohello current and last versions of hello major browsers:  Google Chrome, Microsoft Edge, Microsoft Internet Explorer, and Mozilla Firefox.</span></span>  <span data-ttu-id="71c49-109">Vi räknar hello SDK toofunction med relativt moderna webbläsare.</span><span class="sxs-lookup"><span data-stu-id="71c49-109">We expect hello SDK toofunction with any relatively modern browser.</span></span>

<span data-ttu-id="71c49-110">hello paketet distribueras som en Universal JavaScript-modul, så att den stöder globals AMD, och CommonJS format.</span><span class="sxs-lookup"><span data-stu-id="71c49-110">hello package is distributed as a Universal JavaScript Module, so it supports globals, AMD, and CommonJS formats.</span></span>

## <span data-ttu-id="71c49-111"><a name="Setup"></a>Installationen och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="71c49-111"><a name="Setup"></a>Setup and prerequisites</span></span>
<span data-ttu-id="71c49-112">Den här handboken förutsätts att du har skapat en serverdel med en tabell.</span><span class="sxs-lookup"><span data-stu-id="71c49-112">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="71c49-113">Den här handboken utgår från tabellen hello hello samma schema som hello tabeller i dessa självstudier.</span><span class="sxs-lookup"><span data-stu-id="71c49-113">This guide assumes that hello table has hello same schema as hello tables in those tutorials.</span></span>

<span data-ttu-id="71c49-114">Installera hello Azure Mobile Apps JavaScript SDK kan göras via hello `npm` kommando:</span><span class="sxs-lookup"><span data-stu-id="71c49-114">Installing hello Azure Mobile Apps JavaScript SDK can be done via hello `npm` command:</span></span>

```
npm install azure-mobile-apps-client --save
```

<span data-ttu-id="71c49-115">hello biblioteket kan också användas som en ES2015-modul i CommonJS miljöer, till exempel Browserify och Webpack och som en AMD-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="71c49-115">hello library can also be used as an ES2015 module, within CommonJS environments such as Browserify and Webpack and as an AMD library.</span></span>  <span data-ttu-id="71c49-116">Exempel:</span><span class="sxs-lookup"><span data-stu-id="71c49-116">For example:</span></span>

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

<span data-ttu-id="71c49-117">Du kan också använda en förskapad hello SDK-version genom att hämta direkt från våra CDN:</span><span class="sxs-lookup"><span data-stu-id="71c49-117">You can also use a pre-built version of hello SDK by downloading directly from our CDN:</span></span>

```html
<script src="https://zumo.blob.core.windows.net/sdk/azure-mobile-apps-client.min.js"></script>
```

[!INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

## <span data-ttu-id="71c49-118"><a name="auth"></a>Så här: autentisera användare</span><span class="sxs-lookup"><span data-stu-id="71c49-118"><a name="auth"></a>How to: Authenticate users</span></span>
<span data-ttu-id="71c49-119">Azure Apptjänst har stöd för autentisering och auktorisering av appanvändare som använder olika externa identitetsleverantörer: Facebook, Google, Account och Twitter.</span><span class="sxs-lookup"><span data-stu-id="71c49-119">Azure App Service supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, and Twitter.</span></span> <span data-ttu-id="71c49-120">Du kan ange behörigheter för tabeller toorestrict åtkomst för specifika åtgärder tooonly autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="71c49-120">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="71c49-121">Du kan också använda hello identiteten för autentiserade användare tooimplement auktoriseringsregler i server-skript.</span><span class="sxs-lookup"><span data-stu-id="71c49-121">You can also use hello identity of authenticated users tooimplement authorization rules in server scripts.</span></span> <span data-ttu-id="71c49-122">Mer information finns i hello [komma igång med autentisering] kursen.</span><span class="sxs-lookup"><span data-stu-id="71c49-122">For more information, see hello [Get started with authentication] tutorial.</span></span>

<span data-ttu-id="71c49-123">Två autentisering flöden stöds: ett flöde för server och ett klient-flöde.</span><span class="sxs-lookup"><span data-stu-id="71c49-123">Two authentication flows are supported: a server flow and a client flow.</span></span>  <span data-ttu-id="71c49-124">hello server flödet ger hello enklaste autentiseringsupplevelse som använder hello providern Webbgränssnitt för autentisering.</span><span class="sxs-lookup"><span data-stu-id="71c49-124">hello server flow provides hello simplest authentication experience, as it relies on hello provider's web authentication interface.</span></span> <span data-ttu-id="71c49-125">hello flödet tillåter djupare integrering med specifika funktioner som enkel inloggning som den förlitar sig på provider-specifik SDK: er.</span><span class="sxs-lookup"><span data-stu-id="71c49-125">hello client flow allows for deeper integration with device-specific capabilities such as single-sign-on as it relies on provider-specific SDKs.</span></span>

[!INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

### <span data-ttu-id="71c49-126"><a name="configure-external-redirect-urls"></a>Så här: konfigurera din mobila App Service för externa omdirigerings-URL.</span><span class="sxs-lookup"><span data-stu-id="71c49-126"><a name="configure-external-redirect-urls"></a>How to: Configure your Mobile App Service for external redirect URLs.</span></span>
<span data-ttu-id="71c49-127">Flera typer av JavaScript-program använder en loopback-funktionen toohandle OAuth UI flödar.</span><span class="sxs-lookup"><span data-stu-id="71c49-127">Several types of JavaScript applications use a loopback capability toohandle OAuth UI flows.</span></span>  <span data-ttu-id="71c49-128">Dessa funktioner är:</span><span class="sxs-lookup"><span data-stu-id="71c49-128">These capabilities include:</span></span>

* <span data-ttu-id="71c49-129">Kör tjänsten lokalt</span><span class="sxs-lookup"><span data-stu-id="71c49-129">Running your service locally</span></span>
* <span data-ttu-id="71c49-130">Med hello joniska Framework ladda Live</span><span class="sxs-lookup"><span data-stu-id="71c49-130">Using Live Reload with hello Ionic Framework</span></span>
* <span data-ttu-id="71c49-131">Omdirigera tooApp Service för autentisering.</span><span class="sxs-lookup"><span data-stu-id="71c49-131">Redirecting tooApp Service for authentication.</span></span>

<span data-ttu-id="71c49-132">Kör lokalt kan orsaka problem Eftersom som standard App Service-autentisering är endast konfigurerat tooallow åtkomst från din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="71c49-132">Running locally can cause problems because, by default, App Service authentication is only configured tooallow access from your Mobile App backend.</span></span> <span data-ttu-id="71c49-133">Använd hello följande steg toochange hello Apptjänst inställningar tooenable autentisering när du kör hello server lokalt:</span><span class="sxs-lookup"><span data-stu-id="71c49-133">Use hello following steps toochange hello App Service settings tooenable authentication when running hello server locally:</span></span>

1. <span data-ttu-id="71c49-134">Logga in toohello [Azure-portalen]</span><span class="sxs-lookup"><span data-stu-id="71c49-134">Log in toohello [Azure portal]</span></span>
2. <span data-ttu-id="71c49-135">Navigera tooyour mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="71c49-135">Navigate tooyour Mobile App backend.</span></span>
3. <span data-ttu-id="71c49-136">Välj **resursläsaren** i hello **UTVECKLINGSVERKTYG** menyn.</span><span class="sxs-lookup"><span data-stu-id="71c49-136">Select **Resource explorer** in hello **DEVELOPMENT TOOLS** menu.</span></span>
4. <span data-ttu-id="71c49-137">Klicka på **Gå** tooopen hello resursutforskaren för din mobilappsserverdel i en ny flik eller ett fönster.</span><span class="sxs-lookup"><span data-stu-id="71c49-137">Click **Go** tooopen hello resource explorer for your Mobile App backend in a new tab or window.</span></span>
5. <span data-ttu-id="71c49-138">Expandera hello **config** > **authsettings** noden för din app.</span><span class="sxs-lookup"><span data-stu-id="71c49-138">Expand hello **config** > **authsettings** node for your app.</span></span>
6. <span data-ttu-id="71c49-139">Klicka på hello **redigera** knappen tooenable redigering av hello resurs.</span><span class="sxs-lookup"><span data-stu-id="71c49-139">Click hello **Edit** button tooenable editing of hello resource.</span></span>
7. <span data-ttu-id="71c49-140">Hitta hello **allowedExternalRedirectUrls** element som ska vara null.</span><span class="sxs-lookup"><span data-stu-id="71c49-140">Find hello **allowedExternalRedirectUrls** element, which should be null.</span></span> <span data-ttu-id="71c49-141">Lägg till URL-adresserna i en matris:</span><span class="sxs-lookup"><span data-stu-id="71c49-141">Add your URLs in an array:</span></span>

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    <span data-ttu-id="71c49-142">Ersätt hello URL: er i hello matris med hello URL: er för tjänsten, som i det här exemplet är `http://localhost:3000` för hello lokala Node.js exempel tjänsten.</span><span class="sxs-lookup"><span data-stu-id="71c49-142">Replace hello URLs in hello array with hello URLs of your service, which in this example is `http://localhost:3000` for hello local Node.js sample service.</span></span> <span data-ttu-id="71c49-143">Du kan också använda `http://localhost:4400` för hello små service eller några andra URL, beroende på hur din app har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="71c49-143">You could also use `http://localhost:4400` for hello Ripple service or some other URL, depending on how your app is configured.</span></span>
8. <span data-ttu-id="71c49-144">Hello överst på hello-sidan, klickar du på **Skrivskyddstyp**, klicka sedan på **PLACERA** toosave uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="71c49-144">At hello top of hello page, click **Read/Write**, then click **PUT** toosave your updates.</span></span>

<span data-ttu-id="71c49-145">Du måste också tooadd hello samma loopback-URL: er toohello CORS godkända inställningar:</span><span class="sxs-lookup"><span data-stu-id="71c49-145">You also need tooadd hello same loopback URLs toohello CORS whitelist settings:</span></span>

1. <span data-ttu-id="71c49-146">Gå tillbaka toohello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="71c49-146">Navigate back toohello [Azure portal].</span></span>
2. <span data-ttu-id="71c49-147">Navigera tooyour mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="71c49-147">Navigate tooyour Mobile App backend.</span></span>
3. <span data-ttu-id="71c49-148">Klicka på **CORS** i hello **API** menyn.</span><span class="sxs-lookup"><span data-stu-id="71c49-148">Click **CORS** in hello **API** menu.</span></span>
4. <span data-ttu-id="71c49-149">Ange varje URL i hello tom **tillåtna ursprung** textruta.</span><span class="sxs-lookup"><span data-stu-id="71c49-149">Enter each URL in hello empty **Allowed Origins** text box.</span></span>  <span data-ttu-id="71c49-150">En ny textruta skapas.</span><span class="sxs-lookup"><span data-stu-id="71c49-150">A new text box is created.</span></span>
5. <span data-ttu-id="71c49-151">Klicka på **spara**</span><span class="sxs-lookup"><span data-stu-id="71c49-151">Click **SAVE**</span></span>

<span data-ttu-id="71c49-152">När hello backend uppdateringar, kommer du att kunna toouse hello nya loopback-URL: er i din app.</span><span class="sxs-lookup"><span data-stu-id="71c49-152">After hello backend updates, you will be able toouse hello new loopback URLs in your app.</span></span>

<!-- URLs. -->
[Azure Mobile Apps Snabbstart]: app-service-mobile-cordova-get-started.md
[komma igång med autentisering]: app-service-mobile-cordova-get-started-users.md
[Add authentication tooyour app]: app-service-mobile-cordova-get-started-users.md

[Azure-portalen]: https://portal.azure.com/
[JavaScript SDK för Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
