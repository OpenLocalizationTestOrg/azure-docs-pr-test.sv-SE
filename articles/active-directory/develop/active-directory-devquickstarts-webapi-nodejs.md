---
title: "aaaAzure AD Node.js komma igång | Microsoft Docs"
description: "Hur toobuild en Node.js REST webb-API som kan integreras med Azure AD för autentisering."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 512ae6de9acfde8b58c0447ab4a6b573fb6407c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-web-apis-for-nodejs"></a><span data-ttu-id="9b3ea-103">Komma igång med web API för Node.js</span><span class="sxs-lookup"><span data-stu-id="9b3ea-103">Get started with web APIs for Node.js</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="9b3ea-104">*Passport* är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-104">*Passport* is authentication middleware for Node.js.</span></span> <span data-ttu-id="9b3ea-105">Flexibel och modulära, Passport diskret kan släppas i tooany Express-baserade eller Restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-105">Flexible and modular, Passport can be unobtrusively dropped in tooany Express-based or Restify web application.</span></span> <span data-ttu-id="9b3ea-106">En omfattande uppsättning strategier stöder autentisering med ett användarnamn och lösenord, Facebook, Twitter och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-106">A comprehensive set of strategies support authentication with a username and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="9b3ea-107">Vi har utvecklat en strategi för Microsoft Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9b3ea-107">We have developed a strategy for Microsoft Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="9b3ea-108">Vi installerar den här modulen och lägger sedan till hello Microsoft Azure Active Directory `passport-azure-ad` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-108">We install this module and then add hello Microsoft Azure Active Directory `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="9b3ea-109">toodo, måste du:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-109">toodo this, you need to:</span></span>

1. <span data-ttu-id="9b3ea-110">Registrera ett program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-110">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="9b3ea-111">Konfigurera din app toouse Passport's `passport-azure-ad` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-111">Set up your app toouse Passport's `passport-azure-ad` plug-in.</span></span>
3. <span data-ttu-id="9b3ea-112">Konfigurera en klient programmet toocall hello tooDo lista webb-API.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-112">Configure a client application toocall hello tooDo List web API.</span></span>

<span data-ttu-id="9b3ea-113">hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/Azure-Samples/active-directory-node-webapi).</span><span class="sxs-lookup"><span data-stu-id="9b3ea-113">hello code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-node-webapi).</span></span>

> [!NOTE]
> <span data-ttu-id="9b3ea-114">Den här artikeln inte beskriver hur tooimplement inloggning, registrering, eller hantering med Azure AD B2C-profilen.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-114">This article doesn't cover how tooimplement sign-in, sign-up, or profile management with Azure AD B2C.</span></span> <span data-ttu-id="9b3ea-115">Den fokuserar på anropande webb-API: er när hello användaren redan är autentiserad.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-115">It focuses on calling web APIs after hello user is already authenticated.</span></span>  <span data-ttu-id="9b3ea-116">Vi rekommenderar att du börjar med [hur toointegrate med Azure Active Directory dokument](active-directory-how-to-integrate.md) toolearn om hello grunderna i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-116">We recommend that you start with [How toointegrate with Azure Active Directory document](active-directory-how-to-integrate.md) toolearn about hello basics of Azure Active Directory.</span></span>
>
>

<span data-ttu-id="9b3ea-117">Vi har publicerat alla hello källkoden för det här körs exemplet i GitHub under MIT-licens, så känna sig fria tooclone (eller ännu bättre förgrening) och ge feedback och pull-förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-117">We've released all hello source code for this running example in GitHub under an MIT license, so feel free tooclone (or even better, fork), and provide feedback and pull requests.</span></span>

## <a name="about-nodejs-modules"></a><span data-ttu-id="9b3ea-118">Om Node.js-moduler</span><span class="sxs-lookup"><span data-stu-id="9b3ea-118">About Node.js modules</span></span>
<span data-ttu-id="9b3ea-119">Vi använder Node.js-moduler i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-119">We use Node.js modules in this walkthrough.</span></span> <span data-ttu-id="9b3ea-120">Moduler är inläsningsbar JavaScript-paket som tillhandahåller funktioner för ditt program.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-120">Modules are loadable JavaScript packages that provide specific functionality for your application.</span></span> <span data-ttu-id="9b3ea-121">Vanligtvis installation moduler med hjälp av hello Node.js en NPM-kommandoradsverktyget i installationskatalogen för hello NPM.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-121">You usually install modules by using hello Node.js an NPM command-line tool in hello NPM installation directory.</span></span> <span data-ttu-id="9b3ea-122">Dock med vissa moduler, till exempel hello HTTP-modul i hello core Node.js-paketet.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-122">However, some modules, such as hello HTTP module, are included in hello core Node.js package.</span></span>

<span data-ttu-id="9b3ea-123">Installerade moduler sparas i hello **node_modules** katalogen hello roten i din Node.js-installationskatalogen.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-123">Installed modules are saved in hello **node_modules** directory at hello root of your Node.js installation directory.</span></span> <span data-ttu-id="9b3ea-124">Varje modul i hello **node_modules** directory hanterar en egen **node_modules** katalog som innehåller alla moduler som den är beroende av.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-124">Each module in hello **node_modules** directory maintains its own **node_modules** directory that contains any modules that it depends on.</span></span> <span data-ttu-id="9b3ea-125">Varje modul som krävs har dessutom en **node_modules** directory.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-125">Also, each required module has a **node_modules** directory.</span></span> <span data-ttu-id="9b3ea-126">Den här rekursiv katalogstruktur representerar hello beroendekedjan.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-126">This recursive directory structure represents hello dependency chain.</span></span>

<span data-ttu-id="9b3ea-127">Den här strukturen för beroende kedja resulterar i en större storleken för programmet.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-127">This dependency chain structure results in a larger application footprint.</span></span> <span data-ttu-id="9b3ea-128">Men det garanterar även att alla beroenden är uppfyllda och som hello-versionen av hello moduler som används för att utveckla används också i produktion.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-128">But it also guarantees that all dependencies are met and that hello version of hello modules that's used in development is also used in production.</span></span> <span data-ttu-id="9b3ea-129">Detta gör det mer förutsägbar hello produktion Apps beteende och förhindrar versionshantering problem som kan påverka användare.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-129">This makes hello production app behavior more predictable and prevents versioning problems that might affect users.</span></span>

## <a name="step-1-register-an-azure-ad-tenant"></a><span data-ttu-id="9b3ea-130">Steg 1: Registrera en Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="9b3ea-130">Step 1: Register an Azure AD tenant</span></span>
<span data-ttu-id="9b3ea-131">toouse detta exempel, du behöver en Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-131">toouse this sample, you need an Azure Active Directory tenant.</span></span> <span data-ttu-id="9b3ea-132">Om du inte vet vad en klient eller hur tooget, se [hur tooget en Azure AD-klient](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="9b3ea-132">If you're not sure what a tenant is or how tooget one, see [How tooget an Azure AD tenant](active-directory-howto-tenant.md).</span></span>

## <a name="step-2-create-an-application"></a><span data-ttu-id="9b3ea-133">Steg 2: Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="9b3ea-133">Step 2: Create an application</span></span>
<span data-ttu-id="9b3ea-134">Nu kan du skapa en app i katalogen som ger Azure AD information toosecurely måste kommunicera med din app.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-134">Next you create an app in your directory that gives Azure AD information that it needs toosecurely communicate with your app.</span></span>  <span data-ttu-id="9b3ea-135">Både hello-klientappen och webb-API som representeras av en enda **program-ID** i det här fallet eftersom de omfattar en logisk app.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-135">Both hello client app and web API are represented by a single **Application ID** in this case, because they comprise one logical app.</span></span>  <span data-ttu-id="9b3ea-136">toocreate en app, Följ [instruktionerna](active-directory-how-applications-are-added.md).</span><span class="sxs-lookup"><span data-stu-id="9b3ea-136">toocreate an app, follow [these instructions](active-directory-how-applications-are-added.md).</span></span> <span data-ttu-id="9b3ea-137">Om du skapar en line-of-business-app [dessa ytterligare instruktioner kan vara användbar](../active-directory-applications-guiding-developers-for-lob-applications.md).</span><span class="sxs-lookup"><span data-stu-id="9b3ea-137">If you are building a line-of-business app, [these additional instructions might be useful](../active-directory-applications-guiding-developers-for-lob-applications.md).</span></span>

<span data-ttu-id="9b3ea-138">toocreate ett program:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-138">toocreate an application:</span></span>

1. <span data-ttu-id="9b3ea-139">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9b3ea-139">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="9b3ea-140">Välj ditt konto på hello översta menyn.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-140">On hello top menu, select your account.</span></span> <span data-ttu-id="9b3ea-141">Sedan, under hello **Directory** Välj hello Active Directory-klient där du vill att tooregister ditt program.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-141">Then, under hello **Directory** list, choose hello Active Directory tenant where you want tooregister your application.</span></span>

3. <span data-ttu-id="9b3ea-142">Välj hello menyn hello vänster **fler tjänster**, och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-142">In hello menu on hello left, select **More Services**, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="9b3ea-143">Välj **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-143">Select **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="9b3ea-144">Följ hello prompter toocreate en **webbprogram och/eller WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-144">Follow hello prompts toocreate a **Web Application and/or WebAPI**.</span></span>

      * <span data-ttu-id="9b3ea-145">Hej **namn** av hello programmet beskriver programmet tooend användarna.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-145">hello **name** of hello application describes your application tooend users.</span></span>

      * <span data-ttu-id="9b3ea-146">Hej **inloggnings-URL** är hello bas-URL för din app.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-146">hello **Sign-On URL** is hello base URL of your app.</span></span>  <span data-ttu-id="9b3ea-147">Hej standard webbadressen hello exempelkod `https://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-147">hello default URL of hello sample code is `https://localhost:8080`.</span></span>

6. <span data-ttu-id="9b3ea-148">När du registrerar tilldelar Azure AD appen ett unikt program-ID.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-148">After you register, Azure AD assigns your app a unique Application ID.</span></span> <span data-ttu-id="9b3ea-149">Du behöver det här värdet i nästa avsnitt hello så kopiera den från hello appen på sidan.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-149">You need this value in hello next sections, so copy it from hello application page.</span></span>

7. <span data-ttu-id="9b3ea-150">Från hello **inställningar** -> **egenskaper** för programmet, uppdatera hello App-ID-URI.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-150">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="9b3ea-151">Hej **App-ID URI** är en unik identifierare för programmet.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-151">hello **App ID URI** is a unique identifier for your application.</span></span> <span data-ttu-id="9b3ea-152">hello konventionen är toouse `https://<tenant-domain>/<app-name>`, till exempel: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-152">hello convention is toouse `https://<tenant-domain>/<app-name>`, for example: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span></span>

8. <span data-ttu-id="9b3ea-153">Skapa en **nyckeln** för programmet från hello **inställningar** sida och kopiera den någonstans.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-153">Create a **Key** for your application from hello **Settings** page, and then copy it somewhere.</span></span> <span data-ttu-id="9b3ea-154">Du behöver den snart.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-154">You'll need it shortly.</span></span>

## <a name="step-3-download-nodejs-for-your-platform"></a><span data-ttu-id="9b3ea-155">Steg 3: Ladda ned Node.js för din plattform</span><span class="sxs-lookup"><span data-stu-id="9b3ea-155">Step 3: Download Node.js for your platform</span></span>
<span data-ttu-id="9b3ea-156">toosuccessfully använda det här exemplet måste du ha en fungerande installation av Node.js.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-156">toosuccessfully use this sample, you must have a working installation of Node.js.</span></span>

<span data-ttu-id="9b3ea-157">Installera Node.js från [http://nodejs.org](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="9b3ea-157">Install Node.js from [http://nodejs.org](http://nodejs.org).</span></span>

## <a name="step-4-install-mongodb-on-your-platform"></a><span data-ttu-id="9b3ea-158">Steg 4: Installera MongoDB för din plattform</span><span class="sxs-lookup"><span data-stu-id="9b3ea-158">Step 4: Install MongoDB on your platform</span></span>
<span data-ttu-id="9b3ea-159">toosuccessfully använda det här exemplet måste du ha en fungerande installation av MongoDB.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-159">toosuccessfully use this sample, you must have a working installation of MongoDB.</span></span> <span data-ttu-id="9b3ea-160">Du kan använda MongoDB toomake hello REST API beständiga över serverinstanser.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-160">You use MongoDB toomake hello REST API persistent across server instances.</span></span>

<span data-ttu-id="9b3ea-161">Installera MongoDB från [http://mongodb.org](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="9b3ea-161">Install MongoDB from [http://mongodb.org](http://www.mongodb.org).</span></span>

> [!NOTE]
> <span data-ttu-id="9b3ea-162">Den här genomgången förutsätter att du använder hello installation och server standardslutpunkterna för MongoDB, vilket när detta skrivs hello är mongodb://localhost.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-162">This walkthrough assumes that you use hello default installation and server endpoints for MongoDB, which at hello time of this writing is mongodb://localhost.</span></span>
>
>

## <a name="step-5-install-hello-restify-modules-in-your-web-api"></a><span data-ttu-id="9b3ea-163">Steg 5: Installera hello Restify-modulerna i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="9b3ea-163">Step 5: Install hello Restify modules in your web API</span></span>
<span data-ttu-id="9b3ea-164">Vi använder Restify toobuild vårt REST-API.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-164">We are using Restify toobuild our REST API.</span></span> <span data-ttu-id="9b3ea-165">Restify är ett minimalt och flexibelt Node.js programramverk som härleds från Express.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-165">Restify is a minimal and flexible Node.js application framework that's derived from Express.</span></span> <span data-ttu-id="9b3ea-166">Det har kraftfulla och stabila funktioner för att skapa REST API:er utöver Connect.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-166">It has a robust set of features for building REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="9b3ea-167">Installera Restify</span><span class="sxs-lookup"><span data-stu-id="9b3ea-167">Install Restify</span></span>
1. <span data-ttu-id="9b3ea-168">Från hello Kommandotolken, ändra kataloger toohello **azuread** directory.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-168">From hello command line, change directories toohello **azuread** directory.</span></span> <span data-ttu-id="9b3ea-169">Om hello **azuread** katalogen inte finns, skapar du den.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-169">If hello **azuread** directory does not exist, create it.</span></span>

        `cd azuread - or- mkdir azuread; cd azuread`

2. <span data-ttu-id="9b3ea-170">Skriv följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-170">Type hello following command:</span></span>

    `npm install restify`

    <span data-ttu-id="9b3ea-171">Restify installeras med det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-171">This command installs Restify.</span></span>

#### <a name="did-you-get-an-error"></a><span data-ttu-id="9b3ea-172">Fick du ett felmeddelande?</span><span class="sxs-lookup"><span data-stu-id="9b3ea-172">Did you get an error?</span></span>
<span data-ttu-id="9b3ea-173">När du använder NPM i vissa operativsystem, kan ett felmeddelande som säger **fel: EPERM chmod ”/ usr/lokal/bin /...”**</span><span class="sxs-lookup"><span data-stu-id="9b3ea-173">When you use NPM on some operating systems, you might receive an error that says **Error: EPERM, chmod '/usr/local/bin/..'**</span></span> <span data-ttu-id="9b3ea-174">och ett förslag som du försöker köra hello kontot som administratör.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-174">and a suggestion that you try running hello account as an administrator.</span></span> <span data-ttu-id="9b3ea-175">Om detta inträffar kan du använda hello sudo-kommando toorun NPM på en högre Privilegienivå.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-175">If this occurs, use hello sudo command toorun NPM at a higher privilege level.</span></span>

#### <a name="did-you-get-an-error-regarding-dtrace"></a><span data-ttu-id="9b3ea-176">Fick du ett felmeddelande om DTRACE?</span><span class="sxs-lookup"><span data-stu-id="9b3ea-176">Did you get an error regarding DTRACE?</span></span>
<span data-ttu-id="9b3ea-177">Du kan se ett fel så här när du installerar Restify:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-177">You might see an error like this when installing Restify:</span></span>

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```
<span data-ttu-id="9b3ea-178">Restify erbjuder en kraftfull mekanism för att spåra REST-anrop med DTrace.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-178">Restify provides a powerful mechanism for tracing REST calls by using DTrace.</span></span> <span data-ttu-id="9b3ea-179">Många operativsystem har dock inte DTrace.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-179">However, many operating systems do not have DTrace.</span></span> <span data-ttu-id="9b3ea-180">Du kan ignorera dessa fel utan att oroa dig.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-180">You can safely ignore these errors.</span></span>

<span data-ttu-id="9b3ea-181">hello utdata från kommandot ska se ut ungefär toohello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-181">hello output of this command should look similar toohello following output:</span></span>

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)


## <a name="step-6-install-passportjs-in-your-web-api"></a><span data-ttu-id="9b3ea-182">Steg 6: Installera Passport.js i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="9b3ea-182">Step 6: Install Passport.js in your web API</span></span>
<span data-ttu-id="9b3ea-183">[Passport](http://passportjs.org/) är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-183">[Passport](http://passportjs.org/) is authentication middleware for Node.js.</span></span> <span data-ttu-id="9b3ea-184">Flexibel och modulära, Passport diskret kan släppas i tooany Express-baserade eller Restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-184">Flexible and modular, Passport can be unobtrusively dropped in tooany Express-based or Restify web application.</span></span> <span data-ttu-id="9b3ea-185">En omfattande uppsättning strategier stöder autentisering med ett användarnamn och lösenord, Facebook, Twitter och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-185">A comprehensive set of strategies support authentication with a username and password, Facebook, Twitter, and more.</span></span>

<span data-ttu-id="9b3ea-186">Vi har utvecklat en strategi för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-186">We have developed a strategy for Azure Active Directory.</span></span> <span data-ttu-id="9b3ea-187">Vi installerar den här modulen och lägger sedan till hello Azure Active Directory-strategi plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-187">We install this module and then add hello Azure Active Directory strategy plug-in.</span></span>

1. <span data-ttu-id="9b3ea-188">Från hello Kommandotolken, ändra kataloger toohello **azuread** directory.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-188">From hello command line, change directories toohello **azuread** directory.</span></span>

2. <span data-ttu-id="9b3ea-189">tooinstall passport.js ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-189">tooinstall passport.js, enter hello following command :</span></span>

    `npm install passport`

    <span data-ttu-id="9b3ea-190">hello hello kommandots utdata ska se ut ungefär toohello följande:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-190">hello output of hello command should look similar toohello following:</span></span>

``
        passport@0.1.17 node_modules\passport
        ├── pause@0.0.1
        └── pkginfo@0.2.3
``

## <a name="step-7-add-passport-azure-ad-tooyour-web-api"></a><span data-ttu-id="9b3ea-191">Steg 7: Lägg till Passport-Azure-AD tooyour webb-API</span><span class="sxs-lookup"><span data-stu-id="9b3ea-191">Step 7: Add Passport-Azure-AD tooyour web API</span></span>
<span data-ttu-id="9b3ea-192">Nästa vi lägga till hello OAuth-strategin genom att använda `passport-azure-ad`, en uppsättning strategier som ansluter Azure Active Directory tooPassport.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-192">Next we add hello OAuth strategy by using `passport-azure-ad`, a suite of strategies that connect Azure Active Directory tooPassport.</span></span> <span data-ttu-id="9b3ea-193">Vi använder den här strategin för ägar-token i REST API-exemplet.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-193">We use this strategy for bearer tokens in this REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="9b3ea-194">Även om OAuth2 erbjuder ett ramverk där alla kända token-typer kan utfärdas, används vanligtvis bara vissa token-typer.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-194">Although OAuth2 provides a framework in which any known token type can be issued, only certain token types are commonly used.</span></span> <span data-ttu-id="9b3ea-195">Ägar-token är de vanligaste hello token som skyddar slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-195">Bearer tokens are hello most commonly used tokens for protecting endpoints.</span></span> <span data-ttu-id="9b3ea-196">De är mest utfärdat hello typ av token i OAuth2.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-196">They are hello most widely issued type of token in OAuth2.</span></span> <span data-ttu-id="9b3ea-197">Många olika implementeringar förutsätter att ägar-token hello enda typ av token som utfärdas.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-197">Many implementations assume that bearer tokens are hello only type of tokens that are issued.</span></span>
>
>

<span data-ttu-id="9b3ea-198">Från hello Kommandotolken, ändra kataloger toohello **azuread** directory.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-198">From hello command line, change directories toohello **azuread** directory.</span></span>

<span data-ttu-id="9b3ea-199">Typen hello följande kommando tooinstall hello Passport.js `passport-azure-ad module`:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-199">Type hello following command tooinstall hello Passport.js `passport-azure-ad module`:</span></span>

`npm install passport-azure-ad`

<span data-ttu-id="9b3ea-200">hello hello kommandots utdata ska se ut ungefär toohello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-200">hello output of hello command should look similar toohello following output:</span></span>


    passport-azure-ad@1.0.0 node_modules/passport-azure-ad
    ├── xtend@4.0.0
    ├── xmldom@0.1.19
    ├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
    ├── underscore@1.8.3
    ├── async@1.3.0
    ├── jsonwebtoken@5.0.2
    ├── xml-crypto@0.5.27 (xpath.js@1.0.6)
    ├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
    ├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
    ├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    └── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)



## <a name="step-8-add-mongodb-modules-tooyour-web-api"></a><span data-ttu-id="9b3ea-201">Steg 8: Lägg till MongoDB-moduler tooyour webb-API</span><span class="sxs-lookup"><span data-stu-id="9b3ea-201">Step 8: Add MongoDB modules tooyour web API</span></span>
<span data-ttu-id="9b3ea-202">Vi använder MongoDB som våra datalagret.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-202">We use MongoDB as our datastore.</span></span> <span data-ttu-id="9b3ea-203">Därför måste tooinstall hello ofta används plugin-programmet heter Mongoose toomanage modeller och scheman.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-203">For that reason, we need tooinstall hello widely used plug-in called Mongoose toomanage models and schemas.</span></span> <span data-ttu-id="9b3ea-204">Vi behöver också tooinstall hello databasdrivrutinen för MongoDB (som också kallas MongoDB).</span><span class="sxs-lookup"><span data-stu-id="9b3ea-204">We also need tooinstall hello database driver for MongoDB (which is also called MongoDB).</span></span>

 `npm install mongoose`

## <a name="step-9-install-additional-modules"></a><span data-ttu-id="9b3ea-205">Steg 9: Installera ytterligare moduler</span><span class="sxs-lookup"><span data-stu-id="9b3ea-205">Step 9: Install additional modules</span></span>
<span data-ttu-id="9b3ea-206">Vi installera bredvid hello resterande moduler som krävs.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-206">Next we install hello remaining required modules.</span></span>

1. <span data-ttu-id="9b3ea-207">Från hello Kommandotolken, ändra kataloger toohello **azuread** mappen om du inte visas.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-207">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="9b3ea-208">Ange följande kommandon tooinstall hello modulerna i din **node_modules** directory:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-208">Enter hello following commands tooinstall these modules in your **node_modules** directory:</span></span>

    * `npm install assert-plus`
    * `npm install bunyan`
    * `npm update`

## <a name="step-10-create-a-serverjs-with-your-dependencies"></a><span data-ttu-id="9b3ea-209">Steg 10: Skapa en server.js med dina beroenden</span><span class="sxs-lookup"><span data-stu-id="9b3ea-209">Step 10: Create a server.js with your dependencies</span></span>
<span data-ttu-id="9b3ea-210">Hej server.js-fil innehåller de flesta av hello funktioner för våra API-webbserver.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-210">hello server.js file provides most of hello functionality for our web API server.</span></span> <span data-ttu-id="9b3ea-211">Vi lägga till de flesta av våra toothis kodfilen.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-211">We add most of our code toothis file.</span></span> <span data-ttu-id="9b3ea-212">Vi rekommenderar att du flytta hello funktionerna i mindre filer, till exempel separata vägar och styrenheter för produktion.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-212">For production purposes, we recommend that you refactor hello functionality into smaller files, such as separate routes and controllers.</span></span> <span data-ttu-id="9b3ea-213">I den här demon använder vi du server.js för den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-213">In this demo, we use server.js for this functionality.</span></span>

1. <span data-ttu-id="9b3ea-214">Från hello Kommandotolken, ändra kataloger toohello **azuread** mappen om du inte visas.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-214">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="9b3ea-215">Skapa en `server.js` filen i din favorit-redigeraren och Lägg sedan till hello följande information:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-215">Create a `server.js` file in your favorite editor, and then add hello following information:</span></span>

    ```Javascript
        'use strict';

        /**
         * Module dependencies.
         */

        var fs = require('fs');
        var path = require('path');
        var util = require('util');
        var assert = require('assert-plus');
        var bunyan = require('bunyan');
        var getopt = require('posix-getopt');
        var mongoose = require('mongoose/');
        var restify = require('restify');
        var passport = require('passport');
      var BearerStrategy = require('passport-azure-ad').BearerStrategy;
    ```

3. <span data-ttu-id="9b3ea-216">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-216">Save hello file.</span></span> <span data-ttu-id="9b3ea-217">Returnerar vi tooit inom kort.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-217">We'll return tooit shortly.</span></span>

## <a name="step-11-create-a-config-file-toostore-your-azure-ad-settings"></a><span data-ttu-id="9b3ea-218">Steg 11: Skapa en config-fil toostore Azure AD-inställningar</span><span class="sxs-lookup"><span data-stu-id="9b3ea-218">Step 11: Create a config file toostore your Azure AD settings</span></span>
<span data-ttu-id="9b3ea-219">Den här kodfilen skickar hello konfigurationsparametrar från tooPassport.js din Azure Active Directory-portalen.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-219">This code file passes hello configuration parameters from your Azure Active Directory portal tooPassport.js.</span></span> <span data-ttu-id="9b3ea-220">Du har skapat konfigurationsvärdena när du lade till hello webbportalen API toohello hello första delen av genomgången hello.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-220">You created these configuration values when you added hello web API toohello portal in hello first part of hello walkthrough.</span></span> <span data-ttu-id="9b3ea-221">Vi förklarar vilka tooput i hello värdena för dessa parametrar när du har kopierat hello kod.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-221">We explain what tooput in hello values of these parameters after you copy hello code.</span></span>

1. <span data-ttu-id="9b3ea-222">Från hello Kommandotolken, ändra kataloger toohello **azuread** mappen om du inte visas.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-222">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="9b3ea-223">Skapa en `config.js` filen i din favorit-redigeraren och Lägg sedan till hello följande information:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-223">Create a `config.js` file in your favorite editor, and then add hello following information:</span></span>

    ```Javascript
         exports.creds = {
             mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
             clientID: 'your client ID',
             audience: 'your application URL',
            // you cannot have users from multiple tenants sign in tooyour server unless you use hello common endpoint
          // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
             identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
             validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in tooyour server
             passReqToCallback: false,
             loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes toostderr in Unix.

         };
    ```
3. <span data-ttu-id="9b3ea-224">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-224">Save hello file.</span></span>

## <a name="step-12-add-configuration-values-tooyour-serverjs-file"></a><span data-ttu-id="9b3ea-225">Steg 12: Lägga till värden tooyour server.js konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="9b3ea-225">Step 12: Add configuration values tooyour server.js file</span></span>
<span data-ttu-id="9b3ea-226">Vi behöver tooread värdena från hello .config-fil som du skapade i vårt program.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-226">We need tooread these values from hello .config file that you created across our application.</span></span> <span data-ttu-id="9b3ea-227">toodo kan vi lägga till hello .config-filen som en nödvändig resurs i vårt program.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-227">toodo this, we add hello .config file as a required resource in our application.</span></span> <span data-ttu-id="9b3ea-228">Vi ange hello globala variabler toomatch hello variabler i hello config.js dokumentet.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-228">Then we set hello global variables toomatch hello variables in hello config.js document.</span></span>

1. <span data-ttu-id="9b3ea-229">Från hello Kommandotolken, ändra kataloger toohello **azuread** mappen om du inte visas.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-229">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="9b3ea-230">Öppna din `server.js` filen i din favorit-redigeraren och Lägg sedan till hello följande information:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-230">Open your `server.js` file in your favorite editor, and then add hello following information:</span></span>

    ```Javascript
    var config = require('./config');
    ```
3. <span data-ttu-id="9b3ea-231">Lägg till ett nytt avsnitt för`server.js` med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-231">Then add a new section too`server.js` with hello following code:</span></span>

    ```Javascript
    var options = {
        // hello URL of hello metadata document for your app. We will put hello keys for token validation from hello URL found in hello jwks_uri tag of hello in hello metadata.
        identityMetadata: config.creds.identityMetadata,
        clientID: config.creds.clientID,
        validateIssuer: config.creds.validateIssuer,
        audience: config.creds.audience,
        passReqToCallback: config.creds.passReqToCallback,
        loggingLevel: config.creds.loggingLevel

    };

    // Array toohold logged in users and hello current logged in user (owner).
    var users = [];
    var owner = null;

    // Our logger.
    var log = bunyan.createLogger({
        name: 'Azure Active Directory Bearer Sample',
             streams: [
            {
                stream: process.stderr,
                level: "error",
                name: "error"
            },
            {
                stream: process.stdout,
                level: "warn",
                name: "console"
            }, ]
    });

      // If hello logging level is specified, switch tooit.
      if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    ```

4. <span data-ttu-id="9b3ea-232">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-232">Save hello file.</span></span>

## <a name="step-13-add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="9b3ea-233">Steg 13: Lägg till hello MongoDB-modellen och schemat information genom att använda Mongoose</span><span class="sxs-lookup"><span data-stu-id="9b3ea-233">Step 13: Add hello MongoDB Model and schema information by using Mongoose</span></span>
<span data-ttu-id="9b3ea-234">Nu kommer den här förberedelsen toostart betala av som vi kombinera dessa tre filer i en REST API-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-234">Now all this preparation is going toostart paying off as we combine these three files into a REST API service.</span></span>

<span data-ttu-id="9b3ea-235">För den här genomgången ska använda vi MongoDB toostore våra uppgifter som beskrivs i steg 4.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-235">For this walkthrough, we use MongoDB toostore our tasks as discussed in Step 4.</span></span>

<span data-ttu-id="9b3ea-236">I hello `config.js` -filen som vi skapade i steg 11, vi har ringt vår databas `tasklist`eftersom som har vi gjort hello slutet av våra **mogoose_auth_local** anslutnings-URL.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-236">In hello `config.js` file that we created in Step 11, we called our database `tasklist`, because that was what we put at hello end of our **mogoose_auth_local** connection URL.</span></span> <span data-ttu-id="9b3ea-237">Du behöver inte toocreate databasen i förväg i MongoDB.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-237">You don't need toocreate this database beforehand in MongoDB.</span></span> <span data-ttu-id="9b3ea-238">I stället skapar MongoDB det för oss på hello först kör av vårt serverprogram (förutsatt att hello-databasen inte redan finns).</span><span class="sxs-lookup"><span data-stu-id="9b3ea-238">Instead, MongoDB creates this for us on hello first run of our server application (assuming that hello database doesn't already exist).</span></span>

<span data-ttu-id="9b3ea-239">Nu när vi har redan nämnt hello servern vilken MongoDB-databas vill vi toouse måste vi toowrite vissa ytterligare kod toocreate hello modellen och schemat för vår server uppgifter.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-239">Now that we've told hello server which MongoDB database we'd like toouse, we need toowrite some additional code toocreate hello model and schema for our server's tasks.</span></span>

### <a name="discussion-of-hello-model"></a><span data-ttu-id="9b3ea-240">Beskrivning av hello modellen</span><span class="sxs-lookup"><span data-stu-id="9b3ea-240">Discussion of hello model</span></span>
<span data-ttu-id="9b3ea-241">Vår schemamodellen är enkelt.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-241">Our schema model is simple.</span></span> <span data-ttu-id="9b3ea-242">Du kan expandera den efter behov.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-242">You expand it as required.</span></span>

<span data-ttu-id="9b3ea-243">NAMN: hello namn på hello person som har tilldelats toohello aktivitet.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-243">NAME: hello name of hello person who is assigned toohello task.</span></span> <span data-ttu-id="9b3ea-244">En **sträng**.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-244">A **String**.</span></span>

<span data-ttu-id="9b3ea-245">UPPGIFT: hello själva aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-245">TASK: hello task itself.</span></span> <span data-ttu-id="9b3ea-246">En **sträng**.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-246">A **String**.</span></span>

<span data-ttu-id="9b3ea-247">DATUM: hello datum hello uppgiften förfaller.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-247">DATE: hello date that hello task is due.</span></span> <span data-ttu-id="9b3ea-248">EN **DATETIME**.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-248">A **DATETIME**.</span></span>

<span data-ttu-id="9b3ea-249">SLUTFÖRT: Om hello aktivitet har slutförts eller inte.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-249">COMPLETED: If hello task has been completed or not.</span></span> <span data-ttu-id="9b3ea-250">EN **BOOLESKT**.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-250">A **BOOLEAN**.</span></span>

### <a name="creating-hello-schema-in-hello-code"></a><span data-ttu-id="9b3ea-251">Skapa hello schemat i hello kod</span><span class="sxs-lookup"><span data-stu-id="9b3ea-251">Creating hello schema in hello code</span></span>
1. <span data-ttu-id="9b3ea-252">Från hello Kommandotolken, ändra kataloger toohello **azuread** mappen om du inte visas.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-252">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="9b3ea-253">Öppna din `server.js` filen i din favorit-redigeraren och Lägg sedan till följande information under hello konfigurationspost hello:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-253">Open your `server.js` file in your favorite editor, and then add hello following information below hello configuration entry:</span></span>

    ```Javascript
    // Connect tooMongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');

    // Here we create a schema toostore our tasks and users. It's a fairly simple schema for now.
    var TaskSchema = new Schema({
        owner: String,
        task: String,
        completed: Boolean,
        date: Date
    });

    // Use hello schema tooregister a model.
    mongoose.model('Task', TaskSchema);
    var Task = mongoose.model('Task');
    ```
<span data-ttu-id="9b3ea-254">Som du ser från hello kod skapar vi vårt schemat först.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-254">As you can tell from hello code, we create our schema first.</span></span> <span data-ttu-id="9b3ea-255">Sedan skapar vi ett modellobjekt som vi använder toostore våra data i hela hello code när vi definierar vår **vägar**.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-255">Then we create a model object that we use toostore our data throughout hello code when we define our **Routes**.</span></span>

## <a name="step-14-add-our-routes-for-our-task-rest-api-server"></a><span data-ttu-id="9b3ea-256">Steg 14: Lägga till våra vägar för våra REST API-aktivitetsserver</span><span class="sxs-lookup"><span data-stu-id="9b3ea-256">Step 14: Add our routes for our task REST API server</span></span>
<span data-ttu-id="9b3ea-257">Nu när vi har en modell toowork i databasen med Lägg till hello vägar vi används kommer för vårt REST API-servern.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-257">Now that we have a database model toowork with, let's add hello routes we are going use for our REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="9b3ea-258">Om vägar i Restify</span><span class="sxs-lookup"><span data-stu-id="9b3ea-258">About routes in Restify</span></span>
<span data-ttu-id="9b3ea-259">Vägar fungerar i Restify hello samma sätt som de i hello Express stacken.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-259">Routes work in Restify hello same way they do in hello Express stack.</span></span> <span data-ttu-id="9b3ea-260">Du definierar vägar genom att använda hello URI som du förväntar dig hello klienten program toocall.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-260">You define routes by using hello URI that you expect hello client applications toocall.</span></span> <span data-ttu-id="9b3ea-261">Vanligtvis kan du definiera vägarna i en separat fil.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-261">Usually, you define your routes in a separate file.</span></span> <span data-ttu-id="9b3ea-262">För våra ändamål låter vi vårt vägar i hello server.js-fil.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-262">For our purposes, we put our routes in hello server.js file.</span></span> <span data-ttu-id="9b3ea-263">Vi rekommenderar att du strukturerar dessa vägar till sina egna fil för produktion.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-263">We recommend that you factor these routes into their own file for production use.</span></span>

<span data-ttu-id="9b3ea-264">Ett typiskt mönster för en Restify-väg är följande:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-264">A typical pattern for a Restify route is as follows:</span></span>

```Javascript
function createObject(req, res, next) {

// Do work on object.

 _object.name = req.params.object; // passed value is in req.params under object

 ///...

return next(); // Keep hello server going.
}

....

server.post('/service/:add/:object', createObject); // Calls createObject on routes that match this.

```


<span data-ttu-id="9b3ea-265">Detta är hello mönstret på den mest grundläggande nivån.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-265">This is hello pattern at its most basic level.</span></span> <span data-ttu-id="9b3ea-266">Restify (och Express) ger mycket mer ingående funktioner, till exempel definiera programtyper och tillhandahåller komplex Routning över olika slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-266">Restify (and Express) provide much deeper functionality, such as defining application types and providing complex routing across different endpoints.</span></span> <span data-ttu-id="9b3ea-267">För våra ändamål hindrar VI vägarna enkla.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-267">For our purposes, we are keeping these routes simple.</span></span>

### <a name="add-default-routes-tooour-server"></a><span data-ttu-id="9b3ea-268">Lägg till standard vägar tooour server</span><span class="sxs-lookup"><span data-stu-id="9b3ea-268">Add default routes tooour server</span></span>
<span data-ttu-id="9b3ea-269">Vi nu lägga till hello grundläggande CRUD-vägarna för skapa, hämta, uppdatera och ta bort.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-269">We now add hello basic CRUD routes of Create, Retrieve, Update, and Delete.</span></span>

1. <span data-ttu-id="9b3ea-270">Från hello Kommandotolken, ändra kataloger toohello **azuread** mapp om du inte redan det:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-270">From hello command line, change directories toohello **azuread** folder if you're not already there:</span></span>

    `cd azuread`

2. <span data-ttu-id="9b3ea-271">Öppna hello `server.js` filen i din favorit-redigeraren och Lägg sedan till följande information under hello tidigare databasposter du gjorde hello:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-271">Open hello `server.js` file in your favorite editor, and then add hello following information below hello previous database entries that you made:</span></span>

```Javascript

/**
 *
 * APIs for our REST Task server.
 */

// Create a task.

function createTask(req, res, next) {

    // Restify currently has a bug which doesn't allow you tooset default headers.
    // These headers comply with CORS and allow us toomongodbServer our response tooany origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it, and save it tooMongodb.
    var _task = new Task();

    if (!req.params.task) {
        req.log.warn('createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.task = req.params.task;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable toosave');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}


// Delete a task by name.

function removeTask(req, res, next) {

    Task.remove({
        task: req.params.task,
        owner: owner
    }, function(err) {
        if (err) {
            req.log.warn(err,
                'removeTask: unable toodelete %s',
                req.params.task);
            next(err);
        } else {
            log.info('Deleted task:', req.params.task);
            res.send(204);
            next();
        }
    });
}

// Delete all tasks.

function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
}


// Get a specific task based on name.

function getTask(req, res, next) {

    log.info('getTask was called for: ', owner);
    Task.find({
        owner: owner
    }, function(err, data) {
        if (err) {
            req.log.warn(err, 'get: unable tooread %s', owner);
            next(err);
            return;
        }

        res.json(data);
    });

    return next();
}

/// Simple returns hello list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Restify currently has a bug which doesn't allow you tooset default headers.
    // These headers comply with CORS and allow us toomongodbServer our response tooany origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in hello database. Did you initialize hello database as stated in hello README?");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}

```

### <a name="add-error-handling-in-our-apis"></a><span data-ttu-id="9b3ea-272">Lägg till felhantering i våra API: er</span><span class="sxs-lookup"><span data-stu-id="9b3ea-272">Add error handling in our APIs</span></span>
```

///--- Errors for communicating something interesting back toohello client.

function MissingTaskError() {
    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'MissingTask',
        message: '"task" is a required parameter',
        constructorOpt: MissingTaskError
    });

    this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);


function TaskExistsError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'TaskExists',
        message: owner + ' already exists',
        constructorOpt: TaskExistsError
    });

    this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);


function TaskNotFoundError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 404,
        restCode: 'TaskNotFound',
        message: owner + ' was not found',
        constructorOpt: TaskNotFoundError
    });

    this.name = 'TaskNotFoundError';
}

util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="step-15-create-your-server"></a><span data-ttu-id="9b3ea-273">Steg 15: Skapa servern</span><span class="sxs-lookup"><span data-stu-id="9b3ea-273">Step 15: Create your server</span></span>
<span data-ttu-id="9b3ea-274">Vi har definierat vår databas och våra vägar på plats.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-274">We have defined our database and our routes are in place.</span></span> <span data-ttu-id="9b3ea-275">hello senaste sak toodo är att lägga till hello serverinstansen som hanterar våra anrop.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-275">hello last thing toodo is add hello server instance that manages our calls.</span></span>

<span data-ttu-id="9b3ea-276">I Restify (och snabb) kan du göra mycket anpassning av en REST API-server, men igen vi toouse hello mest grundläggande installationen för våra ändamål.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-276">In Restify (and Express) you can do a lot of customization for a REST API server, but again we are going toouse hello most basic setup for our purposes.</span></span>

```Javascript
/**
 * Our server.
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads.
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());

// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in).
server.use(restify.requestLogger());

// Allow five requests per second by IP, and burst too10.
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use hello common stuff you probably want.
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allow for JSON mapping tooREST.
```

## <a name="step-16-add-hello-routes-toohello-server-without-authentication-for-now"></a><span data-ttu-id="9b3ea-277">Steg 16: Lägg till hello vägar toohello server (utan autentisering för tillfället)</span><span class="sxs-lookup"><span data-stu-id="9b3ea-277">Step 16: Add hello routes toohello server (without authentication for now)</span></span>
```Javascript
/// Now hello real handlers. Here we just CRUD.
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'.
/* In hello pasport.authenticate() method. We set 'session: false' because REST is stateless and
/* we don't need toomaintain session state. You can experiment with removing API protection
/* by removing hello passport.authenticate() method as follows:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler.
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser too%s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json tooget some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="step-17-run-hello-server-before-adding-oauth-support"></a><span data-ttu-id="9b3ea-278">Steg 17: Kör hello server (innan du lägger till stöd för OAuth)</span><span class="sxs-lookup"><span data-stu-id="9b3ea-278">Step 17: Run hello server (before adding OAuth support)</span></span>
<span data-ttu-id="9b3ea-279">Testa servern innan vi lägger till autentisering.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-279">Test out your server before we add authentication.</span></span>

<span data-ttu-id="9b3ea-280">hello enklaste sättet tootest din server är att använda curl på en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-280">hello easiest way tootest your server is by using curl in a command line.</span></span> <span data-ttu-id="9b3ea-281">Innan vi gör det, måste ett verktyg som gör att vi tooparse utdata som JSON.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-281">Before we do that, we need a utility that allows us tooparse output as JSON.</span></span>

1. <span data-ttu-id="9b3ea-282">Installera hello följande JSON-verktyget (alla hello följande exempel använder det här verktyget):</span><span class="sxs-lookup"><span data-stu-id="9b3ea-282">Install hello following JSON tool (all hello following examples use this tool):</span></span>

    `$npm install -g jsontool`

    <span data-ttu-id="9b3ea-283">Detta installerar hello JSON-verktyget globalt.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-283">This installs hello JSON tool globally.</span></span> <span data-ttu-id="9b3ea-284">Nu när vi har åstadkommas som vi spela upp med hello-server:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-284">Now that we’ve accomplished that, let’s play with hello server:</span></span>

2. <span data-ttu-id="9b3ea-285">Kontrollera först att mongoDB-instansen körs:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-285">First, make sure that your mongoDB instance is running:</span></span>

    `$sudo mongod`

3. <span data-ttu-id="9b3ea-286">Sedan ändra toohello katalogen och starta curling:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-286">Then, change toohello directory and start curling:</span></span>

    <span data-ttu-id="9b3ea-287">`$ cd azuread` `$ node server.js`</span><span class="sxs-lookup"><span data-stu-id="9b3ea-287">`$ cd azuread` `$ node server.js`</span></span>

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 171
    Date: Tue, 14 Jul 2015 05:43:38 GMT
    [
    "GET /",
    "POST /tasks/:owner/:task",
    "POST /tasks (for JSON body)",
    "GET /tasks",
    "PUT /tasks/:owner",
    "GET /tasks/:owner",
    "DELETE /tasks/:owner/:task"
    ]
    ```

4. <span data-ttu-id="9b3ea-288">Sedan vi kan lägga till en aktivitet för det här sättet:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-288">Then, we can add a task this way:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    <span data-ttu-id="9b3ea-289">hello svaret bör vara:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-289">hello response should be:</span></span>

        ```Shell
        HTTP/1.1 201 Created
        Connection: close
        Access-Control-Allow-Origin: *
        Access-Control-Allow-Headers: X-Requested-With
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 5
        Date: Tue, 04 Feb 2014 01:02:26 GMT
        Hello
        ```
    <span data-ttu-id="9b3ea-290">Och vi kan ange aktiviteter för Jens det här sättet:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-290">And we can list tasks for Brandon this way:</span></span>

        `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

<span data-ttu-id="9b3ea-291">Om allt detta fungerar är vi klara tooadd OAuth toohello REST API-servern.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-291">If all this works, we're ready tooadd OAuth toohello REST API server.</span></span>

<span data-ttu-id="9b3ea-292">Du har en REST API-server med MongoDB!</span><span class="sxs-lookup"><span data-stu-id="9b3ea-292">You have a REST API server with MongoDB!</span></span>

## <a name="step-18-add-authentication-tooour-rest-api-server"></a><span data-ttu-id="9b3ea-293">Steg 18: Lägg till autentisering tooour REST API-server</span><span class="sxs-lookup"><span data-stu-id="9b3ea-293">Step 18: Add authentication tooour REST API server</span></span>
<span data-ttu-id="9b3ea-294">Nu när vi har en aktiv REST-API kan vi börjar att det är användbart med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-294">Now that we have a running REST API, let's start making it useful with Azure AD.</span></span>

<span data-ttu-id="9b3ea-295">Från hello Kommandotolken, ändra kataloger toohello **azuread** mappen om du inte visas.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-295">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a><span data-ttu-id="9b3ea-296">Använd hello oidc-Ägarstrategi som ingår i passport-azure-ad</span><span class="sxs-lookup"><span data-stu-id="9b3ea-296">Use hello OIDCBearerStrategy that is included with passport-azure-ad</span></span>
<span data-ttu-id="9b3ea-297">Hittills har vi skapat en typisk REST TODO-server utan någon typ av auktorisering.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-297">So far we have built a typical REST TODO server without any kind of authorization.</span></span> <span data-ttu-id="9b3ea-298">Detta är där vi börja sätta ihop som.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-298">This is where we start putting that together.</span></span>

1. <span data-ttu-id="9b3ea-299">Vi måste först tooindicate som vi vill toouse Passport.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-299">First, we need tooindicate that we want toouse Passport.</span></span> <span data-ttu-id="9b3ea-300">Placera den här rättigheten efter din andra serverkonfiguration:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-300">Put this right after your other server configuration:</span></span>

    ```Javascript
            // Let's start using Passport.js.

            server.use(passport.initialize()); // Starts passport.
            server.use(passport.session()); // Provides session support.
    ```
    > [!TIP]
    > <span data-ttu-id="9b3ea-301">När du skriver att API: er, rekommenderar vi att du alltid länka hello data toosomething unika från hello-token som hello användare inte kan förfalska.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-301">When you write APIs, we recommend that you always link hello data toosomething unique from hello token that hello user can’t spoof.</span></span> <span data-ttu-id="9b3ea-302">När den här servern lagrar TODO-objekt, lagrar dem baserat på hello objekt-ID för hello användare i hello token (anropas via token.oid), som vi har gjort i hello ”ägare” fältet.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-302">When this server stores TODO items, it stores them based on hello object ID of hello user in hello token (called through token.oid), which we put in hello “owner” field.</span></span> <span data-ttu-id="9b3ea-303">Detta säkerställer att bara den användaren kan komma åt sina TODOs.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-303">This ensures that only that user can access their TODOs.</span></span> <span data-ttu-id="9b3ea-304">Exponeras aldrig i hello API för ”ägare”, så en extern användare kan begära hello TODOs av andra, även om de autentiseras.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-304">There is no exposure in hello API of “owner,” so an external user can request hello TODOs of others even if they are authenticated.</span></span>                    

2. <span data-ttu-id="9b3ea-305">Nästa ska vi använda hello ägarstrategi som medföljer `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-305">Next let’s use hello bearer strategy that comes with `passport-azure-ad`.</span></span> <span data-ttu-id="9b3ea-306">Titta på hello koden för tillfället och förklarar vi hello rest inom kort.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-306">Look at hello code for now and we'll explain hello rest shortly.</span></span> <span data-ttu-id="9b3ea-307">Placera det efter vad du klistrade in ovan:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-307">Put this after what you pasted above:</span></span>

```Javascript
    /**
    /*
    /* Calling hello OIDCBearerStrategy and managing users.
    /*
    /* Passport pattern provides hello need toomanage users and info tokens
    /* with a FindorCreate() method that must be provided by hello implementor.
    /* Here we just auto-register any user and implement a FindById().
    /* You'll want toodo something smarter.
    **/

    var findById = function(id, fn) {
        for (var i = 0, len = users.length; i < len; i++) {
            var user = users[i];
            if (user.sub === id) {
                log.info('Found user: ', user);
                return fn(null, user);
            }
        }
        return fn(null, null);
    };


    var bearerStrategy = new BearerStrategy(options,
        function(token, done) {
            log.info('verifying hello user');
            log.info(token, 'was hello token retreived');
            findById(token.sub, function(err, user) {
                if (err) {
                    return done(err);
                }
                if (!user) {
                    // "Auto-registration"
                    log.info('User was added automatically as they were new. Their sub is: ', token.sub);
                    users.push(token);
                    owner = token.sub;
                    return done(null, token);
                }
                owner = token.sub;
                return done(null, user, token);
            });
        }
    );

    passport.use(bearerStrategy);
```

<span data-ttu-id="9b3ea-308">Passport använder ett liknande mönster för alla dess strategier (Twitter, Facebook och så vidare) som alla strategiskrivare följer.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-308">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on) that all strategy writers adhere to.</span></span> <span data-ttu-id="9b3ea-309">Tittar på strategin för hello ser du vi skickar en funktion som har en token och en klart som hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-309">Looking at hello strategy, you see we pass it a function that has a token and a done as hello parameters.</span></span> <span data-ttu-id="9b3ea-310">hello strategin kommer tillbaka toous när den har sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-310">hello strategy comes back toous after it does its work.</span></span> <span data-ttu-id="9b3ea-311">När den har lagra vi hello användar- och stash hello token så att vi inte behöver tooask för den igen.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-311">After it does, we store hello user and stash hello token so we won’t need tooask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9b3ea-312">hello föregående kod tar alla användare som händer tooauthenticate tooour server.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-312">hello previous code takes any user that happens tooauthenticate tooour server.</span></span> <span data-ttu-id="9b3ea-313">Detta kallas för automatisk registrering.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-313">This is known as auto-registration.</span></span> <span data-ttu-id="9b3ea-314">Produktionsservrar bör gå du rekommenderar vi att du inte släpper vem som helst utan att behöva dem igenom en registreringsprocess som du väljer.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-314">In production servers, you we recommend that you don't let anyone in without first having them go through a registration process that you decide on.</span></span> <span data-ttu-id="9b3ea-315">Detta är vanligtvis hello mönster du ser i konsumentappar som gör att du tooregister med Facebook men sedan ber du toofill i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-315">This is usually hello pattern you see in consumer apps, which allow you tooregister with Facebook but then ask you toofill out additional information.</span></span> <span data-ttu-id="9b3ea-316">Om det inte var ett kommandoradsprogram kunnat vi extrahera hello e-post från token hello-objekt som returneras och sedan bett hello användaren toofill i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-316">If this wasn’t a command-line program, we could have extracted hello email from hello token object that is returned and then asked hello user toofill out additional information.</span></span> <span data-ttu-id="9b3ea-317">Eftersom detta är en testserver lägger vi helt enkelt lägga till dem toohello minnesinterna databasen.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-317">Because this is a test server, we simply add them toohello in-memory database.</span></span>
>
>

### <a name="protect-some-endpoints"></a><span data-ttu-id="9b3ea-318">Skydda vissa slutpunkter</span><span class="sxs-lookup"><span data-stu-id="9b3ea-318">Protect some endpoints</span></span>
<span data-ttu-id="9b3ea-319">Du skyddar slutpunkter genom att ange hello `passport.authenticate()` anrop med hello-protokollet som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-319">You protect endpoints by specifying hello `passport.authenticate()` call with hello protocol that you want toouse.</span></span>

<span data-ttu-id="9b3ea-320">toomake våra serverkoden gör något mer intressant vi redigera hello väg.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-320">toomake our server code do something more interesting, let’s edit hello route.</span></span>

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="step-19-run-your-server-application-again-and-ensure-it-rejects-you"></a><span data-ttu-id="9b3ea-321">Steg 19: Kör serverprogrammet igen och se till att det avvisar dig</span><span class="sxs-lookup"><span data-stu-id="9b3ea-321">Step 19: Run your server application again and ensure it rejects you</span></span>
<span data-ttu-id="9b3ea-322">Nu ska vi använda `curl` igen toosee om vi nu har OAuth2-skydd mot vår slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-322">Let's use `curl` again toosee if we now have OAuth2 protection against our endpoints.</span></span> <span data-ttu-id="9b3ea-323">Vi gör det här testet innan du kan köra klient-SDK: er mot den här slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-323">We do this test before running any of our client SDKs against this endpoint.</span></span> <span data-ttu-id="9b3ea-324">hello rubriker som returneras bör vara tillräckligt med tootell oss om vi ned hello rätt sökväg.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-324">hello headers that are returned should be enough tootell us if we're going down hello right path.</span></span>

1. <span data-ttu-id="9b3ea-325">Kontrollera först att mongoDB-instansen körs:</span><span class="sxs-lookup"><span data-stu-id="9b3ea-325">First, make sure that your mongoDB instance is running:</span></span>

    `$sudo mongod`

2. <span data-ttu-id="9b3ea-326">Sedan ändra toohello katalogen och starta curling.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-326">Then, change toohello directory and start curling.</span></span>

      <span data-ttu-id="9b3ea-327">`$ cd azuread` `$ node server.js`</span><span class="sxs-lookup"><span data-stu-id="9b3ea-327">`$ cd azuread` `$ node server.js`</span></span>

3. <span data-ttu-id="9b3ea-328">Försök med ett grundläggande INLÄGG.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-328">Try a basic POST.</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

<span data-ttu-id="9b3ea-329">Ett 401 är hello-svar som du letar efter här.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-329">A 401 is hello response you are looking for here.</span></span> <span data-ttu-id="9b3ea-330">Svaret anger att hello Passport-lagret försöker tooredirect toohello behörighet slutpunkt, vilket är exakt vad du vill.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-330">This response indicates that hello Passport layer is trying tooredirect toohello authorized endpoint, which is exactly what you want.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9b3ea-331">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9b3ea-331">Next steps</span></span>
<span data-ttu-id="9b3ea-332">Du har gjort så mycket du kan med den här servern utan att använda en OAuth2-kompatibel klient.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-332">You've gone as far as you can with this server without using an OAuth2 compatible client.</span></span> <span data-ttu-id="9b3ea-333">Du behöver toogo via en ytterligare genomgången.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-333">You will need toogo through an additional walkthrough.</span></span>

<span data-ttu-id="9b3ea-334">Nu har du lärt dig hur tooimplement en REST-API med Restify och OAuth2.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-334">You've now learned how tooimplement a REST API by using Restify and OAuth2.</span></span> <span data-ttu-id="9b3ea-335">Du har mer än tillräckligt med kod tookeep utveckla tjänsten och lär dig också hur toobuild på det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-335">You also have more than enough code tookeep developing your service and learning how toobuild on this example.</span></span>

<span data-ttu-id="9b3ea-336">Om du är intresserad av hello nästa steg i din ADAL resa, kan vissa klienter som stöds ADAL rekommenderar vi att du fortsätta att arbeta med.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-336">If you are interested in hello next steps in your ADAL journey, here are some supported ADAL clients we recommend that you  keep working with.</span></span>

<span data-ttu-id="9b3ea-337">Klona av tooyour developer datorn och konfigurera enligt beskrivningen i hello genomgången.</span><span class="sxs-lookup"><span data-stu-id="9b3ea-337">Clone down tooyour developer machine and configure as described in hello walkthrough.</span></span>

[<span data-ttu-id="9b3ea-338">ADAL för iOS</span><span class="sxs-lookup"><span data-stu-id="9b3ea-338">ADAL for iOS</span></span>](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[<span data-ttu-id="9b3ea-339">ADAL för Android</span><span class="sxs-lookup"><span data-stu-id="9b3ea-339">ADAL for Android</span></span>](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
