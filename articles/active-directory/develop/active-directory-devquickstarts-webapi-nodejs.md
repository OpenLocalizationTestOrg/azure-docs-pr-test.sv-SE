---
title: "Azure AD Node.js komma igång | Microsoft Docs"
description: "Hur du skapar en Node.js-REST-webb-API som kan integreras med Azure AD för autentisering."
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
ms.openlocfilehash: 4f58177f540c14172d7ece8b4bc8c8a2b9787f8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-web-apis-for-nodejs"></a><span data-ttu-id="3f7e4-103">Komma igång med web API för Node.js</span><span class="sxs-lookup"><span data-stu-id="3f7e4-103">Get started with web APIs for Node.js</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="3f7e4-104">*Passport* är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-104">*Passport* is authentication middleware for Node.js.</span></span> <span data-ttu-id="3f7e4-105">Flexibel och modulära, Passport diskret kan släppas till alla Express-baserade eller Restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-105">Flexible and modular, Passport can be unobtrusively dropped in to any Express-based or Restify web application.</span></span> <span data-ttu-id="3f7e4-106">En omfattande uppsättning strategier stöder autentisering med ett användarnamn och lösenord, Facebook, Twitter och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-106">A comprehensive set of strategies support authentication with a username and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="3f7e4-107">Vi har utvecklat en strategi för Microsoft Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3f7e4-107">We have developed a strategy for Microsoft Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="3f7e4-108">Vi installerar den här modulen och lägger sedan till Microsoft Azure Active Directory `passport-azure-ad` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-108">We install this module and then add the Microsoft Azure Active Directory `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="3f7e4-109">Om du vill göra det måste du:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-109">To do this, you need to:</span></span>

1. <span data-ttu-id="3f7e4-110">Registrera ett program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-110">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="3f7e4-111">Konfigurera din app att använda Passport `passport-azure-ad` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-111">Set up your app to use Passport's `passport-azure-ad` plug-in.</span></span>
3. <span data-ttu-id="3f7e4-112">Konfigurera ett klientprogram att anropa att göra-lista webb-API.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-112">Configure a client application to call the To Do List web API.</span></span>

<span data-ttu-id="3f7e4-113">Koden för den här självstudiekursen [finns på GitHub](https://github.com/Azure-Samples/active-directory-node-webapi).</span><span class="sxs-lookup"><span data-stu-id="3f7e4-113">The code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-node-webapi).</span></span>

> [!NOTE]
> <span data-ttu-id="3f7e4-114">Den här artikeln beskriver inte hur du implementerar inloggning, registrering och profilhantering med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-114">This article doesn't cover how to implement sign-in, sign-up, or profile management with Azure AD B2C.</span></span> <span data-ttu-id="3f7e4-115">Den fokuserar på anropande webb-API: er när användaren redan autentiserats.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-115">It focuses on calling web APIs after the user is already authenticated.</span></span>  <span data-ttu-id="3f7e4-116">Vi rekommenderar att du börjar med [att integrera med Azure Active Directory-dokumentet](active-directory-how-to-integrate.md) att lära dig grunderna i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-116">We recommend that you start with [How to integrate with Azure Active Directory document](active-directory-how-to-integrate.md) to learn about the basics of Azure Active Directory.</span></span>
>
>

<span data-ttu-id="3f7e4-117">Vi har publicerat källkoden exempelvis den här körs i GitHub under MIT-licens, så passa på att klona (eller ännu bättre förgrening) och ge feedback och pull-förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-117">We've released all the source code for this running example in GitHub under an MIT license, so feel free to clone (or even better, fork), and provide feedback and pull requests.</span></span>

## <a name="about-nodejs-modules"></a><span data-ttu-id="3f7e4-118">Om Node.js-moduler</span><span class="sxs-lookup"><span data-stu-id="3f7e4-118">About Node.js modules</span></span>
<span data-ttu-id="3f7e4-119">Vi använder Node.js-moduler i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-119">We use Node.js modules in this walkthrough.</span></span> <span data-ttu-id="3f7e4-120">Moduler är inläsningsbar JavaScript-paket som tillhandahåller funktioner för ditt program.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-120">Modules are loadable JavaScript packages that provide specific functionality for your application.</span></span> <span data-ttu-id="3f7e4-121">Du installerar vanligtvis moduler med Node.js en NPM-kommandoradsverktyget i NPM-installationskatalogen.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-121">You usually install modules by using the Node.js an NPM command-line tool in the NPM installation directory.</span></span> <span data-ttu-id="3f7e4-122">Dock ingår vissa moduler, till exempel HTTP-modul i core Node.js-paketet.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-122">However, some modules, such as the HTTP module, are included in the core Node.js package.</span></span>

<span data-ttu-id="3f7e4-123">Installerade moduler sparas i den **node_modules** katalogen i roten på din Node.js-installationskatalogen.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-123">Installed modules are saved in the **node_modules** directory at the root of your Node.js installation directory.</span></span> <span data-ttu-id="3f7e4-124">Varje modul i den **node_modules** directory hanterar en egen **node_modules** katalog som innehåller alla moduler som den är beroende av.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-124">Each module in the **node_modules** directory maintains its own **node_modules** directory that contains any modules that it depends on.</span></span> <span data-ttu-id="3f7e4-125">Varje modul som krävs har dessutom en **node_modules** directory.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-125">Also, each required module has a **node_modules** directory.</span></span> <span data-ttu-id="3f7e4-126">Den här rekursiv katalogstruktur representerar beroendekedjan.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-126">This recursive directory structure represents the dependency chain.</span></span>

<span data-ttu-id="3f7e4-127">Den här strukturen för beroende kedja resulterar i en större storleken för programmet.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-127">This dependency chain structure results in a larger application footprint.</span></span> <span data-ttu-id="3f7e4-128">Men det garanterar även att alla beroenden är uppfyllda och att den version av moduler som används för att utveckla används också i produktion.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-128">But it also guarantees that all dependencies are met and that the version of the modules that's used in development is also used in production.</span></span> <span data-ttu-id="3f7e4-129">Detta gör det mer förutsägbar produktion Apps beteende och förhindrar versionshantering problem som kan påverka användare.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-129">This makes the production app behavior more predictable and prevents versioning problems that might affect users.</span></span>

## <a name="step-1-register-an-azure-ad-tenant"></a><span data-ttu-id="3f7e4-130">Steg 1: Registrera en Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="3f7e4-130">Step 1: Register an Azure AD tenant</span></span>
<span data-ttu-id="3f7e4-131">Om du vill använda det här exemplet behöver du en Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-131">To use this sample, you need an Azure Active Directory tenant.</span></span> <span data-ttu-id="3f7e4-132">Om du inte är säker på vilken en klient eller hur du hämtar ett, finns [hur du hämtar en Azure AD-klient](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="3f7e4-132">If you're not sure what a tenant is or how to get one, see [How to get an Azure AD tenant](active-directory-howto-tenant.md).</span></span>

## <a name="step-2-create-an-application"></a><span data-ttu-id="3f7e4-133">Steg 2: Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="3f7e4-133">Step 2: Create an application</span></span>
<span data-ttu-id="3f7e4-134">Nu kan du skapa en app i katalogen som ger Azure AD den information som krävs för att kommunicera säkert med din app.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-134">Next you create an app in your directory that gives Azure AD information that it needs to securely communicate with your app.</span></span>  <span data-ttu-id="3f7e4-135">Både klientappen och webb-API som representeras av ett enda **program-ID** i det här fallet eftersom de omfattar en logisk app.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-135">Both the client app and web API are represented by a single **Application ID** in this case, because they comprise one logical app.</span></span>  <span data-ttu-id="3f7e4-136">Du skapar en app genom att följa [dessa anvisningar](active-directory-how-applications-are-added.md).</span><span class="sxs-lookup"><span data-stu-id="3f7e4-136">To create an app, follow [these instructions](active-directory-how-applications-are-added.md).</span></span> <span data-ttu-id="3f7e4-137">Om du skapar en line-of-business-app [dessa ytterligare instruktioner kan vara användbar](../active-directory-applications-guiding-developers-for-lob-applications.md).</span><span class="sxs-lookup"><span data-stu-id="3f7e4-137">If you are building a line-of-business app, [these additional instructions might be useful](../active-directory-applications-guiding-developers-for-lob-applications.md).</span></span>

<span data-ttu-id="3f7e4-138">Skapa ett program:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-138">To create an application:</span></span>

1. <span data-ttu-id="3f7e4-139">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3f7e4-139">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="3f7e4-140">Välj ditt konto på den översta menyn.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-140">On the top menu, select your account.</span></span> <span data-ttu-id="3f7e4-141">Sedan, under den **Directory** Välj Active Directory-klient som du vill registrera ditt program.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-141">Then, under the **Directory** list, choose the Active Directory tenant where you want to register your application.</span></span>

3. <span data-ttu-id="3f7e4-142">Välj på menyn till vänster **fler tjänster**, och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-142">In the menu on the left, select **More Services**, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="3f7e4-143">Välj **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-143">Select **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="3f7e4-144">Följ anvisningarna för att skapa en **webbprogram och/eller WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-144">Follow the prompts to create a **Web Application and/or WebAPI**.</span></span>

      * <span data-ttu-id="3f7e4-145">Den **namn** beskriver ditt program för slutanvändare av programmet.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-145">The **name** of the application describes your application to end users.</span></span>

      * <span data-ttu-id="3f7e4-146">Den **inloggnings-URL** är den grundläggande Webbadressen för din app.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-146">The **Sign-On URL** is the base URL of your app.</span></span>  <span data-ttu-id="3f7e4-147">Standard-URL i exempelkoden är `https://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-147">The default URL of the sample code is `https://localhost:8080`.</span></span>

6. <span data-ttu-id="3f7e4-148">När du registrerar tilldelar Azure AD appen ett unikt program-ID.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-148">After you register, Azure AD assigns your app a unique Application ID.</span></span> <span data-ttu-id="3f7e4-149">Du behöver det här värdet i nästa avsnitt så kopiera den från appen på sidan.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-149">You need this value in the next sections, so copy it from the application page.</span></span>

7. <span data-ttu-id="3f7e4-150">Från den **inställningar** -> **egenskaper** för programmet, uppdatera App-ID-URI.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-150">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="3f7e4-151">Den **App-ID URI** är en unik identifierare för programmet.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-151">The **App ID URI** is a unique identifier for your application.</span></span> <span data-ttu-id="3f7e4-152">Konventionen är att använda `https://<tenant-domain>/<app-name>`, till exempel: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-152">The convention is to use `https://<tenant-domain>/<app-name>`, for example: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span></span>

8. <span data-ttu-id="3f7e4-153">Skapa en **nyckeln** för programmet från den **inställningar** sida och kopiera den någonstans.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-153">Create a **Key** for your application from the **Settings** page, and then copy it somewhere.</span></span> <span data-ttu-id="3f7e4-154">Du behöver den snart.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-154">You'll need it shortly.</span></span>

## <a name="step-3-download-nodejs-for-your-platform"></a><span data-ttu-id="3f7e4-155">Steg 3: Ladda ned Node.js för din plattform</span><span class="sxs-lookup"><span data-stu-id="3f7e4-155">Step 3: Download Node.js for your platform</span></span>
<span data-ttu-id="3f7e4-156">Du måste ha en fungerande installation av Node.js. för att kunna använda det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-156">To successfully use this sample, you must have a working installation of Node.js.</span></span>

<span data-ttu-id="3f7e4-157">Installera Node.js från [http://nodejs.org](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="3f7e4-157">Install Node.js from [http://nodejs.org](http://nodejs.org).</span></span>

## <a name="step-4-install-mongodb-on-your-platform"></a><span data-ttu-id="3f7e4-158">Steg 4: Installera MongoDB för din plattform</span><span class="sxs-lookup"><span data-stu-id="3f7e4-158">Step 4: Install MongoDB on your platform</span></span>
<span data-ttu-id="3f7e4-159">Du måste ha en fungerande installation av MongoDB för att kunna använda det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-159">To successfully use this sample, you must have a working installation of MongoDB.</span></span> <span data-ttu-id="3f7e4-160">Du kan använda MongoDB för att göra det REST API beständiga över serverinstanser.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-160">You use MongoDB to make the REST API persistent across server instances.</span></span>

<span data-ttu-id="3f7e4-161">Installera MongoDB från [http://mongodb.org](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="3f7e4-161">Install MongoDB from [http://mongodb.org](http://www.mongodb.org).</span></span>

> [!NOTE]
> <span data-ttu-id="3f7e4-162">Den här genomgången förutsätter att du använder standardslutpunkterna för installation och server för MongoDB, som vid tidpunkten för detta skrivs är mongodb://localhost.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-162">This walkthrough assumes that you use the default installation and server endpoints for MongoDB, which at the time of this writing is mongodb://localhost.</span></span>
>
>

## <a name="step-5-install-the-restify-modules-in-your-web-api"></a><span data-ttu-id="3f7e4-163">Steg 5: Installera Restify-modulerna i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="3f7e4-163">Step 5: Install the Restify modules in your web API</span></span>
<span data-ttu-id="3f7e4-164">Vi använder Restify för att skapa vårt REST-API.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-164">We are using Restify to build our REST API.</span></span> <span data-ttu-id="3f7e4-165">Restify är ett minimalt och flexibelt Node.js programramverk som härleds från Express.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-165">Restify is a minimal and flexible Node.js application framework that's derived from Express.</span></span> <span data-ttu-id="3f7e4-166">Det har kraftfulla och stabila funktioner för att skapa REST API:er utöver Connect.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-166">It has a robust set of features for building REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="3f7e4-167">Installera Restify</span><span class="sxs-lookup"><span data-stu-id="3f7e4-167">Install Restify</span></span>
1. <span data-ttu-id="3f7e4-168">Från kommandoraden ändrar du katalog till den **azuread** directory.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-168">From the command line, change directories to the **azuread** directory.</span></span> <span data-ttu-id="3f7e4-169">Om den **azuread** katalogen inte finns, skapar du den.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-169">If the **azuread** directory does not exist, create it.</span></span>

        `cd azuread - or- mkdir azuread; cd azuread`

2. <span data-ttu-id="3f7e4-170">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-170">Type the following command:</span></span>

    `npm install restify`

    <span data-ttu-id="3f7e4-171">Restify installeras med det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-171">This command installs Restify.</span></span>

#### <a name="did-you-get-an-error"></a><span data-ttu-id="3f7e4-172">Fick du ett felmeddelande?</span><span class="sxs-lookup"><span data-stu-id="3f7e4-172">Did you get an error?</span></span>
<span data-ttu-id="3f7e4-173">När du använder NPM i vissa operativsystem, kan ett felmeddelande som säger **fel: EPERM chmod ”/ usr/lokal/bin /...”**</span><span class="sxs-lookup"><span data-stu-id="3f7e4-173">When you use NPM on some operating systems, you might receive an error that says **Error: EPERM, chmod '/usr/local/bin/..'**</span></span> <span data-ttu-id="3f7e4-174">och förslag du försöka köra kontot som administratör.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-174">and a suggestion that you try running the account as an administrator.</span></span> <span data-ttu-id="3f7e4-175">Om detta inträffar kan du använda sudo-kommando för att köra NPM på en högre Privilegienivå.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-175">If this occurs, use the sudo command to run NPM at a higher privilege level.</span></span>

#### <a name="did-you-get-an-error-regarding-dtrace"></a><span data-ttu-id="3f7e4-176">Fick du ett felmeddelande om DTRACE?</span><span class="sxs-lookup"><span data-stu-id="3f7e4-176">Did you get an error regarding DTRACE?</span></span>
<span data-ttu-id="3f7e4-177">Du kan se ett fel så här när du installerar Restify:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-177">You might see an error like this when installing Restify:</span></span>

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
<span data-ttu-id="3f7e4-178">Restify erbjuder en kraftfull mekanism för att spåra REST-anrop med DTrace.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-178">Restify provides a powerful mechanism for tracing REST calls by using DTrace.</span></span> <span data-ttu-id="3f7e4-179">Många operativsystem har dock inte DTrace.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-179">However, many operating systems do not have DTrace.</span></span> <span data-ttu-id="3f7e4-180">Du kan ignorera dessa fel utan att oroa dig.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-180">You can safely ignore these errors.</span></span>

<span data-ttu-id="3f7e4-181">Kommandots utdata bör likna följande utdata:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-181">The output of this command should look similar to the following output:</span></span>

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


## <a name="step-6-install-passportjs-in-your-web-api"></a><span data-ttu-id="3f7e4-182">Steg 6: Installera Passport.js i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="3f7e4-182">Step 6: Install Passport.js in your web API</span></span>
<span data-ttu-id="3f7e4-183">[Passport](http://passportjs.org/) är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-183">[Passport](http://passportjs.org/) is authentication middleware for Node.js.</span></span> <span data-ttu-id="3f7e4-184">Flexibel och modulära, Passport diskret kan släppas till alla Express-baserade eller Restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-184">Flexible and modular, Passport can be unobtrusively dropped in to any Express-based or Restify web application.</span></span> <span data-ttu-id="3f7e4-185">En omfattande uppsättning strategier stöder autentisering med ett användarnamn och lösenord, Facebook, Twitter och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-185">A comprehensive set of strategies support authentication with a username and password, Facebook, Twitter, and more.</span></span>

<span data-ttu-id="3f7e4-186">Vi har utvecklat en strategi för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-186">We have developed a strategy for Azure Active Directory.</span></span> <span data-ttu-id="3f7e4-187">Vi installerar den här modulen och lägger sedan till plugin-programmet Azure Active Directory strategin.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-187">We install this module and then add the Azure Active Directory strategy plug-in.</span></span>

1. <span data-ttu-id="3f7e4-188">Från kommandoraden ändrar du katalog till den **azuread** directory.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-188">From the command line, change directories to the **azuread** directory.</span></span>

2. <span data-ttu-id="3f7e4-189">Ange följande kommando för att installera passport.js:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-189">To install passport.js, enter the following command :</span></span>

    `npm install passport`

    <span data-ttu-id="3f7e4-190">Resultatet av kommandot ska se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-190">The output of the command should look similar to the following:</span></span>

``
        passport@0.1.17 node_modules\passport
        ├── pause@0.0.1
        └── pkginfo@0.2.3
``

## <a name="step-7-add-passport-azure-ad-to-your-web-api"></a><span data-ttu-id="3f7e4-191">Steg 7: Lägg till Passport-Azure-AD i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="3f7e4-191">Step 7: Add Passport-Azure-AD to your web API</span></span>
<span data-ttu-id="3f7e4-192">Nästa vi lägga till OAuth-strategin genom att använda `passport-azure-ad`, en uppsättning strategier som ansluter Azure Active Directory till Passport.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-192">Next we add the OAuth strategy by using `passport-azure-ad`, a suite of strategies that connect Azure Active Directory to Passport.</span></span> <span data-ttu-id="3f7e4-193">Vi använder den här strategin för ägar-token i REST API-exemplet.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-193">We use this strategy for bearer tokens in this REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="3f7e4-194">Även om OAuth2 erbjuder ett ramverk där alla kända token-typer kan utfärdas, används vanligtvis bara vissa token-typer.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-194">Although OAuth2 provides a framework in which any known token type can be issued, only certain token types are commonly used.</span></span> <span data-ttu-id="3f7e4-195">Ägar-token är de vanligaste token som skyddar slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-195">Bearer tokens are the most commonly used tokens for protecting endpoints.</span></span> <span data-ttu-id="3f7e4-196">De är de mest typ token i OAuth2.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-196">They are the most widely issued type of token in OAuth2.</span></span> <span data-ttu-id="3f7e4-197">Många olika implementeringar utgår ifrån att ägar-token är den enda typen av token som utfärdas.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-197">Many implementations assume that bearer tokens are the only type of tokens that are issued.</span></span>
>
>

<span data-ttu-id="3f7e4-198">Från kommandoraden ändrar du katalog till den **azuread** directory.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-198">From the command line, change directories to the **azuread** directory.</span></span>

<span data-ttu-id="3f7e4-199">Skriv följande kommando för att installera Passport.js `passport-azure-ad module`:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-199">Type the following command to install the Passport.js `passport-azure-ad module`:</span></span>

`npm install passport-azure-ad`

<span data-ttu-id="3f7e4-200">Utdata från kommandot bör likna följande utdata:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-200">The output of the command should look similar to the following output:</span></span>


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



## <a name="step-8-add-mongodb-modules-to-your-web-api"></a><span data-ttu-id="3f7e4-201">Steg 8: Lägga till MongoDB-moduler i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="3f7e4-201">Step 8: Add MongoDB modules to your web API</span></span>
<span data-ttu-id="3f7e4-202">Vi använder MongoDB som våra datalagret.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-202">We use MongoDB as our datastore.</span></span> <span data-ttu-id="3f7e4-203">Därför som vi behöver installera vanligt plugin-programmet kallas Mongoose för att hantera modeller och scheman.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-203">For that reason, we need to install the widely used plug-in called Mongoose to manage models and schemas.</span></span> <span data-ttu-id="3f7e4-204">Vi behöver också installerar databasdrivrutinen för MongoDB (som också kallas MongoDB).</span><span class="sxs-lookup"><span data-stu-id="3f7e4-204">We also need to install the database driver for MongoDB (which is also called MongoDB).</span></span>

 `npm install mongoose`

## <a name="step-9-install-additional-modules"></a><span data-ttu-id="3f7e4-205">Steg 9: Installera ytterligare moduler</span><span class="sxs-lookup"><span data-stu-id="3f7e4-205">Step 9: Install additional modules</span></span>
<span data-ttu-id="3f7e4-206">Vi installera sedan resterande moduler som krävs.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-206">Next we install the remaining required modules.</span></span>

1. <span data-ttu-id="3f7e4-207">Från kommandoraden ändrar du katalog till den **azuread** mappen om du inte visas.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-207">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="3f7e4-208">Ange följande kommandon för att installera modulerna i din **node_modules** directory:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-208">Enter the following commands to install these modules in your **node_modules** directory:</span></span>

    * `npm install assert-plus`
    * `npm install bunyan`
    * `npm update`

## <a name="step-10-create-a-serverjs-with-your-dependencies"></a><span data-ttu-id="3f7e4-209">Steg 10: Skapa en server.js med dina beroenden</span><span class="sxs-lookup"><span data-stu-id="3f7e4-209">Step 10: Create a server.js with your dependencies</span></span>
<span data-ttu-id="3f7e4-210">Server.js-fil innehåller de flesta funktionerna för våra API-webbserver.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-210">The server.js file provides most of the functionality for our web API server.</span></span> <span data-ttu-id="3f7e4-211">Vi lägga till största delen av koden i den här filen.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-211">We add most of our code to this file.</span></span> <span data-ttu-id="3f7e4-212">För produktion rekommenderar vi att du refactor funktionerna i mindre filer, till exempel separata vägar och styrenheter.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-212">For production purposes, we recommend that you refactor the functionality into smaller files, such as separate routes and controllers.</span></span> <span data-ttu-id="3f7e4-213">I den här demon använder vi du server.js för den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-213">In this demo, we use server.js for this functionality.</span></span>

1. <span data-ttu-id="3f7e4-214">Från kommandoraden ändrar du katalog till den **azuread** mappen om du inte visas.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-214">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="3f7e4-215">Skapa en `server.js` filen i din favorit-redigeraren och Lägg sedan till följande information:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-215">Create a `server.js` file in your favorite editor, and then add the following information:</span></span>

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

3. <span data-ttu-id="3f7e4-216">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-216">Save the file.</span></span> <span data-ttu-id="3f7e4-217">Vi återkommer till det inom kort.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-217">We'll return to it shortly.</span></span>

## <a name="step-11-create-a-config-file-to-store-your-azure-ad-settings"></a><span data-ttu-id="3f7e4-218">Steg 11: Skapa en konfigurationsfil för att lagra dina Azure AD-inställningar</span><span class="sxs-lookup"><span data-stu-id="3f7e4-218">Step 11: Create a config file to store your Azure AD settings</span></span>
<span data-ttu-id="3f7e4-219">Den här kodfilen skickar konfigurationsparametrarna från din Azure Active Directory-portalen till Passport.js.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-219">This code file passes the configuration parameters from your Azure Active Directory portal to Passport.js.</span></span> <span data-ttu-id="3f7e4-220">Du har skapat konfigurationsvärdena när du lade till webb-API i portalen under den första delen av genomgången.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-220">You created these configuration values when you added the web API to the portal in the first part of the walkthrough.</span></span> <span data-ttu-id="3f7e4-221">När du har kopierat koden förklarar vi vad du ska ange i värdena för dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-221">We explain what to put in the values of these parameters after you copy the code.</span></span>

1. <span data-ttu-id="3f7e4-222">Från kommandoraden ändrar du katalog till den **azuread** mappen om du inte visas.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-222">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="3f7e4-223">Skapa en `config.js` filen i din favorit-redigeraren och Lägg sedan till följande information:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-223">Create a `config.js` file in your favorite editor, and then add the following information:</span></span>

    ```Javascript
         exports.creds = {
             mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
             clientID: 'your client ID',
             audience: 'your application URL',
            // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
          // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
             identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
             validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
             passReqToCallback: false,
             loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

         };
    ```
3. <span data-ttu-id="3f7e4-224">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-224">Save the file.</span></span>

## <a name="step-12-add-configuration-values-to-your-serverjs-file"></a><span data-ttu-id="3f7e4-225">Steg 12: Lägg till konfigurationsvärden i din server.js-fil</span><span class="sxs-lookup"><span data-stu-id="3f7e4-225">Step 12: Add configuration values to your server.js file</span></span>
<span data-ttu-id="3f7e4-226">Vi behöver läsa värdena från .config-filen som du skapade i vårt program.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-226">We need to read these values from the .config file that you created across our application.</span></span> <span data-ttu-id="3f7e4-227">Detta gör vi lägga till .config-filen som en nödvändig resurs i vårt program.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-227">To do this, we add the .config file as a required resource in our application.</span></span> <span data-ttu-id="3f7e4-228">Vi anger globala variabler för att matcha variabler i config.js-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-228">Then we set the global variables to match the variables in the config.js document.</span></span>

1. <span data-ttu-id="3f7e4-229">Från kommandoraden ändrar du katalog till den **azuread** mappen om du inte visas.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-229">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="3f7e4-230">Öppna din `server.js` filen i din favorit-redigeraren och Lägg sedan till följande information:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-230">Open your `server.js` file in your favorite editor, and then add the following information:</span></span>

    ```Javascript
    var config = require('./config');
    ```
3. <span data-ttu-id="3f7e4-231">Lägg till ett nytt avsnitt som `server.js` med följande kod:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-231">Then add a new section to `server.js` with the following code:</span></span>

    ```Javascript
    var options = {
        // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
        identityMetadata: config.creds.identityMetadata,
        clientID: config.creds.clientID,
        validateIssuer: config.creds.validateIssuer,
        audience: config.creds.audience,
        passReqToCallback: config.creds.passReqToCallback,
        loggingLevel: config.creds.loggingLevel

    };

    // Array to hold logged in users and the current logged in user (owner).
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

      // If the logging level is specified, switch to it.
      if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    ```

4. <span data-ttu-id="3f7e4-232">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-232">Save the file.</span></span>

## <a name="step-13-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="3f7e4-233">Steg 13: Lägg till information i MongoDB-modellen och schemat genom att använda Mongoose</span><span class="sxs-lookup"><span data-stu-id="3f7e4-233">Step 13: Add The MongoDB Model and schema information by using Mongoose</span></span>
<span data-ttu-id="3f7e4-234">Nu kommer dessa förberedelser att starta betala av som vi kombinera dessa tre filer i en REST API-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-234">Now all this preparation is going to start paying off as we combine these three files into a REST API service.</span></span>

<span data-ttu-id="3f7e4-235">Vi använder MongoDB för att lagra våra uppgifter som beskrivs i steg 4 i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-235">For this walkthrough, we use MongoDB to store our tasks as discussed in Step 4.</span></span>

<span data-ttu-id="3f7e4-236">I den `config.js` -filen som vi skapade i steg 11, vi har ringt vår databas `tasklist`eftersom som har vi gjort i slutet av våra **mogoose_auth_local** anslutnings-URL.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-236">In the `config.js` file that we created in Step 11, we called our database `tasklist`, because that was what we put at the end of our **mogoose_auth_local** connection URL.</span></span> <span data-ttu-id="3f7e4-237">Du behöver inte skapa den här databasen i förväg i MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-237">You don't need to create this database beforehand in MongoDB.</span></span> <span data-ttu-id="3f7e4-238">I stället skapar MongoDB det för oss på den första körningen av vårt serverprogram (förutsatt att databasen inte redan finns).</span><span class="sxs-lookup"><span data-stu-id="3f7e4-238">Instead, MongoDB creates this for us on the first run of our server application (assuming that the database doesn't already exist).</span></span>

<span data-ttu-id="3f7e4-239">Nu när vi har redan nämnt servern vilken MongoDB-databas som vi vill använda, behöver vi skriva ytterligare kod för att skapa modellen och schemat för vår server uppgifter.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-239">Now that we've told the server which MongoDB database we'd like to use, we need to write some additional code to create the model and schema for our server's tasks.</span></span>

### <a name="discussion-of-the-model"></a><span data-ttu-id="3f7e4-240">Beskrivning av modellen</span><span class="sxs-lookup"><span data-stu-id="3f7e4-240">Discussion of the model</span></span>
<span data-ttu-id="3f7e4-241">Vår schemamodellen är enkelt.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-241">Our schema model is simple.</span></span> <span data-ttu-id="3f7e4-242">Du kan expandera den efter behov.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-242">You expand it as required.</span></span>

<span data-ttu-id="3f7e4-243">NAMN: Namnet på den person som har tilldelats aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-243">NAME: The name of the person who is assigned to the task.</span></span> <span data-ttu-id="3f7e4-244">En **sträng**.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-244">A **String**.</span></span>

<span data-ttu-id="3f7e4-245">UPPGIFT: Själva aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-245">TASK: The task itself.</span></span> <span data-ttu-id="3f7e4-246">En **sträng**.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-246">A **String**.</span></span>

<span data-ttu-id="3f7e4-247">DATUM: Det datum då uppgiften förfaller.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-247">DATE: The date that the task is due.</span></span> <span data-ttu-id="3f7e4-248">EN **DATETIME**.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-248">A **DATETIME**.</span></span>

<span data-ttu-id="3f7e4-249">SLUTFÖRD: Om aktiviteten har slutförts eller inte.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-249">COMPLETED: If the task has been completed or not.</span></span> <span data-ttu-id="3f7e4-250">EN **BOOLESKT**.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-250">A **BOOLEAN**.</span></span>

### <a name="creating-the-schema-in-the-code"></a><span data-ttu-id="3f7e4-251">Skapa schemat i koden</span><span class="sxs-lookup"><span data-stu-id="3f7e4-251">Creating the schema in the code</span></span>
1. <span data-ttu-id="3f7e4-252">Från kommandoraden ändrar du katalog till den **azuread** mappen om du inte visas.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-252">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="3f7e4-253">Öppna din `server.js` filen i din favorit-redigeraren och Lägg sedan till följande information under konfigurationsposten:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-253">Open your `server.js` file in your favorite editor, and then add the following information below the configuration entry:</span></span>

    ```Javascript
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');

    // Here we create a schema to store our tasks and users. It's a fairly simple schema for now.
    var TaskSchema = new Schema({
        owner: String,
        task: String,
        completed: Boolean,
        date: Date
    });

    // Use the schema to register a model.
    mongoose.model('Task', TaskSchema);
    var Task = mongoose.model('Task');
    ```
<span data-ttu-id="3f7e4-254">Som du ser från koden skapar vi vårt schemat först.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-254">As you can tell from the code, we create our schema first.</span></span> <span data-ttu-id="3f7e4-255">Sedan skapar vi ett modellobjekt som vi använder för att lagra våra data i hela koden när vi definiera vår **vägar**.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-255">Then we create a model object that we use to store our data throughout the code when we define our **Routes**.</span></span>

## <a name="step-14-add-our-routes-for-our-task-rest-api-server"></a><span data-ttu-id="3f7e4-256">Steg 14: Lägga till våra vägar för våra REST API-aktivitetsserver</span><span class="sxs-lookup"><span data-stu-id="3f7e4-256">Step 14: Add our routes for our task REST API server</span></span>
<span data-ttu-id="3f7e4-257">Nu när vi har en databasmodell att arbeta med Lägg till de vägar som vi ska använda för vårt REST API-servern.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-257">Now that we have a database model to work with, let's add the routes we are going use for our REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="3f7e4-258">Om vägar i Restify</span><span class="sxs-lookup"><span data-stu-id="3f7e4-258">About routes in Restify</span></span>
<span data-ttu-id="3f7e4-259">I Restify fungerar vägar på samma sätt som i Express-stacken.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-259">Routes work in Restify the same way they do in the Express stack.</span></span> <span data-ttu-id="3f7e4-260">Du definierar vägar genom att använda den URI som du förväntar dig att klientprogram anropar.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-260">You define routes by using the URI that you expect the client applications to call.</span></span> <span data-ttu-id="3f7e4-261">Vanligtvis kan du definiera vägarna i en separat fil.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-261">Usually, you define your routes in a separate file.</span></span> <span data-ttu-id="3f7e4-262">För våra ändamål låter vi vårt vägar i server.js-fil.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-262">For our purposes, we put our routes in the server.js file.</span></span> <span data-ttu-id="3f7e4-263">Vi rekommenderar att du strukturerar dessa vägar till sina egna fil för produktion.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-263">We recommend that you factor these routes into their own file for production use.</span></span>

<span data-ttu-id="3f7e4-264">Ett typiskt mönster för en Restify-väg är följande:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-264">A typical pattern for a Restify route is as follows:</span></span>

```Javascript
function createObject(req, res, next) {

// Do work on object.

 _object.name = req.params.object; // passed value is in req.params under object

 ///...

return next(); // Keep the server going.
}

....

server.post('/service/:add/:object', createObject); // Calls createObject on routes that match this.

```


<span data-ttu-id="3f7e4-265">Det här är mönstret på den mest grundläggande nivån.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-265">This is the pattern at its most basic level.</span></span> <span data-ttu-id="3f7e4-266">Restify (och Express) ger mycket mer ingående funktioner, till exempel definiera programtyper och tillhandahåller komplex Routning över olika slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-266">Restify (and Express) provide much deeper functionality, such as defining application types and providing complex routing across different endpoints.</span></span> <span data-ttu-id="3f7e4-267">För våra ändamål hindrar VI vägarna enkla.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-267">For our purposes, we are keeping these routes simple.</span></span>

### <a name="add-default-routes-to-our-server"></a><span data-ttu-id="3f7e4-268">Lägga till standardvägar i vår server</span><span class="sxs-lookup"><span data-stu-id="3f7e4-268">Add default routes to our server</span></span>
<span data-ttu-id="3f7e4-269">Vi nu lägga till grundläggande CRUD-vägarna för skapa, hämta, uppdatera och ta bort.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-269">We now add the basic CRUD routes of Create, Retrieve, Update, and Delete.</span></span>

1. <span data-ttu-id="3f7e4-270">Från kommandoraden ändrar du katalog till den **azuread** mapp om du inte redan det:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-270">From the command line, change directories to the **azuread** folder if you're not already there:</span></span>

    `cd azuread`

2. <span data-ttu-id="3f7e4-271">Öppna den `server.js` filen i din favorit-redigeraren och Lägg sedan till följande information under de föregående databasposter du gjorde:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-271">Open the `server.js` file in your favorite editor, and then add the following information below the previous database entries that you made:</span></span>

```Javascript

/**
 *
 * APIs for our REST Task server.
 */

// Create a task.

function createTask(req, res, next) {

    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it, and save it to Mongodb.
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
            req.log.warn(err, 'createTask: unable to save');
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
                'removeTask: unable to delete %s',
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
            req.log.warn(err, 'get: unable to read %s', owner);
            next(err);
            return;
        }

        res.json(data);
    });

    return next();
}

/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

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
            log.warn(err, "There is no tasks in the database. Did you initialize the database as stated in the README?");
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

### <a name="add-error-handling-in-our-apis"></a><span data-ttu-id="3f7e4-272">Lägg till felhantering i våra API: er</span><span class="sxs-lookup"><span data-stu-id="3f7e4-272">Add error handling in our APIs</span></span>
```

///--- Errors for communicating something interesting back to the client.

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


## <a name="step-15-create-your-server"></a><span data-ttu-id="3f7e4-273">Steg 15: Skapa servern</span><span class="sxs-lookup"><span data-stu-id="3f7e4-273">Step 15: Create your server</span></span>
<span data-ttu-id="3f7e4-274">Vi har definierat vår databas och våra vägar på plats.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-274">We have defined our database and our routes are in place.</span></span> <span data-ttu-id="3f7e4-275">Det sista du behöver göra är att lägga till serverinstansen som hanterar våra anrop.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-275">The last thing to do is add the server instance that manages our calls.</span></span>

<span data-ttu-id="3f7e4-276">I Restify (och snabb) kan du göra mycket anpassning av en REST API-server, men igen vi ska använda den mest grundläggande installationen för våra ändamål.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-276">In Restify (and Express) you can do a lot of customization for a REST API server, but again we are going to use the most basic setup for our purposes.</span></span>

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

// Allow five requests per second by IP, and burst to 10.
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want.
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allow for JSON mapping to REST.
```

## <a name="step-16-add-the-routes-to-the-server-without-authentication-for-now"></a><span data-ttu-id="3f7e4-277">Steg 16: Lägg till vägar på servern (utan autentisering för tillfället)</span><span class="sxs-lookup"><span data-stu-id="3f7e4-277">Step 16: Add the routes to the server (without authentication for now)</span></span>
```Javascript
/// Now the real handlers. Here we just CRUD.
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'.
/* In the pasport.authenticate() method. We set 'session: false' because REST is stateless and
/* we don't need to maintain session state. You can experiment with removing API protection
/* by removing the passport.authenticate() method as follows:
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
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="step-17-run-the-server-before-adding-oauth-support"></a><span data-ttu-id="3f7e4-278">Steg 17: Kör servern (innan du lägger till stöd för OAuth)</span><span class="sxs-lookup"><span data-stu-id="3f7e4-278">Step 17: Run the server (before adding OAuth support)</span></span>
<span data-ttu-id="3f7e4-279">Testa servern innan vi lägger till autentisering.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-279">Test out your server before we add authentication.</span></span>

<span data-ttu-id="3f7e4-280">Det enklaste sättet att testa din server är med hjälp av curl på en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-280">The easiest way to test your server is by using curl in a command line.</span></span> <span data-ttu-id="3f7e4-281">Innan vi gör det, måste ett verktyg som gör att vi kan analysera utdata som JSON.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-281">Before we do that, we need a utility that allows us to parse output as JSON.</span></span>

1. <span data-ttu-id="3f7e4-282">Installera följande JSON-verktyget (i följande exempel används det här verktyget):</span><span class="sxs-lookup"><span data-stu-id="3f7e4-282">Install the following JSON tool (all the following examples use this tool):</span></span>

    `$npm install -g jsontool`

    <span data-ttu-id="3f7e4-283">Då installeras JSON-verktyget globalt.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-283">This installs the JSON tool globally.</span></span> <span data-ttu-id="3f7e4-284">Nu när vi har åstadkommas som vi spela upp på servern:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-284">Now that we’ve accomplished that, let’s play with the server:</span></span>

2. <span data-ttu-id="3f7e4-285">Kontrollera först att mongoDB-instansen körs:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-285">First, make sure that your mongoDB instance is running:</span></span>

    `$sudo mongod`

3. <span data-ttu-id="3f7e4-286">Sedan kan ändra till katalogen och starta curling:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-286">Then, change to the directory and start curling:</span></span>

    <span data-ttu-id="3f7e4-287">`$ cd azuread` `$ node server.js`</span><span class="sxs-lookup"><span data-stu-id="3f7e4-287">`$ cd azuread` `$ node server.js`</span></span>

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

4. <span data-ttu-id="3f7e4-288">Sedan vi kan lägga till en aktivitet för det här sättet:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-288">Then, we can add a task this way:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    <span data-ttu-id="3f7e4-289">Svaret bör vara:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-289">The response should be:</span></span>

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
    <span data-ttu-id="3f7e4-290">Och vi kan ange aktiviteter för Jens det här sättet:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-290">And we can list tasks for Brandon this way:</span></span>

        `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

<span data-ttu-id="3f7e4-291">Om allt detta fungerar redo vi att lägga till OAuth i REST API-servern.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-291">If all this works, we're ready to add OAuth to the REST API server.</span></span>

<span data-ttu-id="3f7e4-292">Du har en REST API-server med MongoDB!</span><span class="sxs-lookup"><span data-stu-id="3f7e4-292">You have a REST API server with MongoDB!</span></span>

## <a name="step-18-add-authentication-to-our-rest-api-server"></a><span data-ttu-id="3f7e4-293">Steg 18: Lägg till autentisering i vårt REST API-server</span><span class="sxs-lookup"><span data-stu-id="3f7e4-293">Step 18: Add authentication to our REST API server</span></span>
<span data-ttu-id="3f7e4-294">Nu när vi har en aktiv REST-API kan vi börjar att det är användbart med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-294">Now that we have a running REST API, let's start making it useful with Azure AD.</span></span>

<span data-ttu-id="3f7e4-295">Från kommandoraden ändrar du katalog till den **azuread** mappen om du inte visas.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-295">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a><span data-ttu-id="3f7e4-296">Använd den OIDC-ägarstrategi som ingår i passport-azure-ad</span><span class="sxs-lookup"><span data-stu-id="3f7e4-296">Use the OIDCBearerStrategy that is included with passport-azure-ad</span></span>
<span data-ttu-id="3f7e4-297">Hittills har vi skapat en typisk REST TODO-server utan någon typ av auktorisering.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-297">So far we have built a typical REST TODO server without any kind of authorization.</span></span> <span data-ttu-id="3f7e4-298">Detta är där vi börja sätta ihop som.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-298">This is where we start putting that together.</span></span>

1. <span data-ttu-id="3f7e4-299">Vi måste först ange att vi vill använda Passport.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-299">First, we need to indicate that we want to use Passport.</span></span> <span data-ttu-id="3f7e4-300">Placera den här rättigheten efter din andra serverkonfiguration:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-300">Put this right after your other server configuration:</span></span>

    ```Javascript
            // Let's start using Passport.js.

            server.use(passport.initialize()); // Starts passport.
            server.use(passport.session()); // Provides session support.
    ```
    > [!TIP]
    > <span data-ttu-id="3f7e4-301">När du skriver API: er, rekommenderar vi att du alltid länka dina data till något unikt från token som användaren inte kan förfalska.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-301">When you write APIs, we recommend that you always link the data to something unique from the token that the user can’t spoof.</span></span> <span data-ttu-id="3f7e4-302">När den här servern lagrar TODO-objekt, lagrar dem baserat på objekt-ID för användaren i token (anropas via token.oid), som vi har gjort i fältet ”ägare”.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-302">When this server stores TODO items, it stores them based on the object ID of the user in the token (called through token.oid), which we put in the “owner” field.</span></span> <span data-ttu-id="3f7e4-303">Detta säkerställer att bara den användaren kan komma åt sina TODOs.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-303">This ensures that only that user can access their TODOs.</span></span> <span data-ttu-id="3f7e4-304">Det finns inga exponering API ”ägare” en extern användare kan begära TODOs av andra, även om de autentiseras.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-304">There is no exposure in the API of “owner,” so an external user can request the TODOs of others even if they are authenticated.</span></span>                    

2. <span data-ttu-id="3f7e4-305">Nästa ska vi använda den ägarstrategi som medföljer `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-305">Next let’s use the bearer strategy that comes with `passport-azure-ad`.</span></span> <span data-ttu-id="3f7e4-306">Titta på koden för tillfället och förklarar vi resten inom kort.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-306">Look at the code for now and we'll explain the rest shortly.</span></span> <span data-ttu-id="3f7e4-307">Placera det efter vad du klistrade in ovan:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-307">Put this after what you pasted above:</span></span>

```Javascript
    /**
    /*
    /* Calling the OIDCBearerStrategy and managing users.
    /*
    /* Passport pattern provides the need to manage users and info tokens
    /* with a FindorCreate() method that must be provided by the implementor.
    /* Here we just auto-register any user and implement a FindById().
    /* You'll want to do something smarter.
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
            log.info('verifying the user');
            log.info(token, 'was the token retreived');
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

<span data-ttu-id="3f7e4-308">Passport använder ett liknande mönster för alla dess strategier (Twitter, Facebook och så vidare) som alla strategiskrivare följer.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-308">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on) that all strategy writers adhere to.</span></span> <span data-ttu-id="3f7e4-309">Tittar på strategin ser du vi skickar en funktion som har en token och klart som parametrar.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-309">Looking at the strategy, you see we pass it a function that has a token and a done as the parameters.</span></span> <span data-ttu-id="3f7e4-310">Strategin kommer tillbaka till oss när den har sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-310">The strategy comes back to us after it does its work.</span></span> <span data-ttu-id="3f7e4-311">När det har vi lagra användaren och token så att vi inte behöver fråga efter den igen.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-311">After it does, we store the user and stash the token so we won’t need to ask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f7e4-312">Föregående kod tar alla användare som inträffar att autentisera till vår server.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-312">The previous code takes any user that happens to authenticate to our server.</span></span> <span data-ttu-id="3f7e4-313">Detta kallas för automatisk registrering.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-313">This is known as auto-registration.</span></span> <span data-ttu-id="3f7e4-314">Produktionsservrar bör gå du rekommenderar vi att du inte släpper vem som helst utan att behöva dem igenom en registreringsprocess som du väljer.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-314">In production servers, you we recommend that you don't let anyone in without first having them go through a registration process that you decide on.</span></span> <span data-ttu-id="3f7e4-315">Detta är oftast det mönster du ser i konsumentappar, så att du kan registrera med Facebook men sedan uppmanad att fylla i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-315">This is usually the pattern you see in consumer apps, which allow you to register with Facebook but then ask you to fill out additional information.</span></span> <span data-ttu-id="3f7e4-316">Om det inte var ett kommandoradsprogram kunnat vi extrahera den e-postadressen från tokenobjektet som returneras och sedan bett användaren att fylla i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-316">If this wasn’t a command-line program, we could have extracted the email from the token object that is returned and then asked the user to fill out additional information.</span></span> <span data-ttu-id="3f7e4-317">Eftersom det här är en testserver lägger vi helt enkelt till dem i den minnesinterna databasen.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-317">Because this is a test server, we simply add them to the in-memory database.</span></span>
>
>

### <a name="protect-some-endpoints"></a><span data-ttu-id="3f7e4-318">Skydda vissa slutpunkter</span><span class="sxs-lookup"><span data-stu-id="3f7e4-318">Protect some endpoints</span></span>
<span data-ttu-id="3f7e4-319">Du skyddar slutpunkter genom att ange den `passport.authenticate()` anropa med det protokoll som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-319">You protect endpoints by specifying the `passport.authenticate()` call with the protocol that you want to use.</span></span>

<span data-ttu-id="3f7e4-320">Se vår serverkoden gör något mer intressant, vi redigera vägen.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-320">To make our server code do something more interesting, let’s edit the route.</span></span>

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

## <a name="step-19-run-your-server-application-again-and-ensure-it-rejects-you"></a><span data-ttu-id="3f7e4-321">Steg 19: Kör serverprogrammet igen och se till att det avvisar dig</span><span class="sxs-lookup"><span data-stu-id="3f7e4-321">Step 19: Run your server application again and ensure it rejects you</span></span>
<span data-ttu-id="3f7e4-322">Nu ska vi använda `curl` igen för att se om vi nu har OAuth2-skydd mot vår slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-322">Let's use `curl` again to see if we now have OAuth2 protection against our endpoints.</span></span> <span data-ttu-id="3f7e4-323">Vi gör det här testet innan du kan köra klient-SDK: er mot den här slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-323">We do this test before running any of our client SDKs against this endpoint.</span></span> <span data-ttu-id="3f7e4-324">Rubriker som returneras borde räcka för att berätta för oss om vi ned sökvägen.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-324">The headers that are returned should be enough to tell us if we're going down the right path.</span></span>

1. <span data-ttu-id="3f7e4-325">Kontrollera först att mongoDB-instansen körs:</span><span class="sxs-lookup"><span data-stu-id="3f7e4-325">First, make sure that your mongoDB instance is running:</span></span>

    `$sudo mongod`

2. <span data-ttu-id="3f7e4-326">Sedan kan ändra till katalogen och starta curling.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-326">Then, change to the directory and start curling.</span></span>

      <span data-ttu-id="3f7e4-327">`$ cd azuread` `$ node server.js`</span><span class="sxs-lookup"><span data-stu-id="3f7e4-327">`$ cd azuread` `$ node server.js`</span></span>

3. <span data-ttu-id="3f7e4-328">Försök med ett grundläggande INLÄGG.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-328">Try a basic POST.</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

<span data-ttu-id="3f7e4-329">Ett 401 är det svar du letar efter här.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-329">A 401 is the response you are looking for here.</span></span> <span data-ttu-id="3f7e4-330">Svaret anger att Passport-lagret försöker omdirigera till auktoriserade slutpunkten, vilket är exakt vad du vill.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-330">This response indicates that the Passport layer is trying to redirect to the authorized endpoint, which is exactly what you want.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f7e4-331">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3f7e4-331">Next steps</span></span>
<span data-ttu-id="3f7e4-332">Du har gjort så mycket du kan med den här servern utan att använda en OAuth2-kompatibel klient.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-332">You've gone as far as you can with this server without using an OAuth2 compatible client.</span></span> <span data-ttu-id="3f7e4-333">Du behöver gå igenom ett ytterligare genomgången.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-333">You will need to go through an additional walkthrough.</span></span>

<span data-ttu-id="3f7e4-334">Nu har du lärt dig hur du implementerar en REST-API med Restify och OAuth2.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-334">You've now learned how to implement a REST API by using Restify and OAuth2.</span></span> <span data-ttu-id="3f7e4-335">Du kan också ha mer än tillräckligt med kod för att hålla utveckla tjänsten och lär dig hur du bygger på det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-335">You also have more than enough code to keep developing your service and learning how to build on this example.</span></span>

<span data-ttu-id="3f7e4-336">Om du är intresserad av i nästa steg i din ADAL resa, kan vissa klienter som stöds ADAL rekommenderar vi att du fortsätta att arbeta med.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-336">If you are interested in the next steps in your ADAL journey, here are some supported ADAL clients we recommend that you  keep working with.</span></span>

<span data-ttu-id="3f7e4-337">Klona ned till datorn utvecklare och konfigurera enligt beskrivningen i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="3f7e4-337">Clone down to your developer machine and configure as described in the walkthrough.</span></span>

[<span data-ttu-id="3f7e4-338">ADAL för iOS</span><span class="sxs-lookup"><span data-stu-id="3f7e4-338">ADAL for iOS</span></span>](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[<span data-ttu-id="3f7e4-339">ADAL för Android</span><span class="sxs-lookup"><span data-stu-id="3f7e4-339">ADAL for Android</span></span>](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
