---
title: Skydda ett Azure Active Directory v2.0 webb-API med Node.js | Microsoft Docs
description: "Lär dig mer om att skapa en Node.js-webb-API som accepterar token från en personligt Microsoft-konto och från arbetet eller skolan konton."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 0b572fc1-2aaf-4cb6-82de-63010fb1941d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 94e945a52b9df7c495de1611baa08083357670c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="secure-a-web-api-by-using-nodejs"></a><span data-ttu-id="e5215-103">Säkra ett webb-API med Node.js</span><span class="sxs-lookup"><span data-stu-id="e5215-103">Secure a web API by using Node.js</span></span>
> [!NOTE]
> <span data-ttu-id="e5215-104">Inte alla Azure Active Directory-scenarier och funktioner fungerar med v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="e5215-104">Not all Azure Active Directory scenarios and features work with the v2.0 endpoint.</span></span> <span data-ttu-id="e5215-105">Läs mer om för att avgöra om du ska använda v2.0-slutpunkten eller v1.0 slutpunkten [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="e5215-105">To determine whether you should use the v2.0 endpoint or the v1.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

<span data-ttu-id="e5215-106">När du använder Azure Active Directory (AD Azure) v2.0-slutpunkten kan du använda [OAuth 2.0](active-directory-v2-protocols.md) åtkomst-tokens för att skydda ditt webb-API.</span><span class="sxs-lookup"><span data-stu-id="e5215-106">When you use the Azure Active Directory (Azure AD) v2.0 endpoint, you can use [OAuth 2.0](active-directory-v2-protocols.md) access tokens to protect your web API.</span></span> <span data-ttu-id="e5215-107">Åtkomst-token, användare som har både en personliga Microsoft-konto och arbete eller skolkonton kan på ett säkert sätt komma åt ditt webb-API med OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="e5215-107">With OAuth 2.0 access tokens, users who have both a personal Microsoft account and work or school accounts can securely access your web API.</span></span>

<span data-ttu-id="e5215-108">*Passport* är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="e5215-108">*Passport* is authentication middleware for Node.js.</span></span> <span data-ttu-id="e5215-109">Flexibel och modulära, Passport kan släppas diskret in i alla Express-baserade eller restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="e5215-109">Flexible and modular, Passport can be unobtrusively dropped into any Express-based or restify web application.</span></span> <span data-ttu-id="e5215-110">I Passport, en omfattande uppsättning strategier stöder autentisering med ett användarnamn och lösenord, Facebook, Twitter eller andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="e5215-110">In Passport, a comprehensive set of strategies support authentication by using a username and password, Facebook, Twitter, or other options.</span></span> <span data-ttu-id="e5215-111">Vi har utvecklat en strategi för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e5215-111">We have developed a strategy for Azure AD.</span></span> <span data-ttu-id="e5215-112">I den här artikeln vi hur du kan installera modulen och Lägg sedan till Azure AD `passport-azure-ad` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="e5215-112">In this article, we show you how to install the module, and then add the Azure AD `passport-azure-ad` plug-in.</span></span>

## <a name="download"></a><span data-ttu-id="e5215-113">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="e5215-113">Download</span></span>
<span data-ttu-id="e5215-114">Koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).</span><span class="sxs-lookup"><span data-stu-id="e5215-114">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).</span></span> <span data-ttu-id="e5215-115">Om du vill följa kursen, kan du [ladda ned appens stomme som en .zip-fil](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), eller klona stommen:</span><span class="sxs-lookup"><span data-stu-id="e5215-115">To follow the tutorial, you can [download the app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

<span data-ttu-id="e5215-116">Du kan också få det färdiga programmet i slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e5215-116">You also can get the completed application at the end of this tutorial.</span></span>

## <a name="1-register-an-app"></a><span data-ttu-id="e5215-117">1: registrera en app</span><span class="sxs-lookup"><span data-stu-id="e5215-117">1: Register an app</span></span>
<span data-ttu-id="e5215-118">Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa [dessa detaljerade steg](active-directory-v2-app-registration.md) att registrera en app.</span><span class="sxs-lookup"><span data-stu-id="e5215-118">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow [these detailed steps](active-directory-v2-app-registration.md) to register an app.</span></span> <span data-ttu-id="e5215-119">Se till att du:</span><span class="sxs-lookup"><span data-stu-id="e5215-119">Make sure you:</span></span>

* <span data-ttu-id="e5215-120">Kopiera den **program-Id** tilldelats din app.</span><span class="sxs-lookup"><span data-stu-id="e5215-120">Copy the **Application Id** assigned to your app.</span></span> <span data-ttu-id="e5215-121">Du behöver det för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e5215-121">You'll need it for this tutorial.</span></span>
* <span data-ttu-id="e5215-122">Lägg till den **Mobile** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="e5215-122">Add the **Mobile** platform for your app.</span></span>
* <span data-ttu-id="e5215-123">Kopiera den **omdirigerings-URI** från portalen.</span><span class="sxs-lookup"><span data-stu-id="e5215-123">Copy the **Redirect URI** from the portal.</span></span> <span data-ttu-id="e5215-124">Du måste använda URI standardvärdet `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="e5215-124">You must use the default URI value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="2-install-nodejs"></a><span data-ttu-id="e5215-125">2: Installera Node.js</span><span class="sxs-lookup"><span data-stu-id="e5215-125">2: Install Node.js</span></span>
<span data-ttu-id="e5215-126">Använd den här självstudiekursen, måste du [installera Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="e5215-126">To use the sample for this tutorial, you must [install Node.js](http://nodejs.org).</span></span>

## <a name="3-install-mongodb"></a><span data-ttu-id="e5215-127">3: Installera MongoDB</span><span class="sxs-lookup"><span data-stu-id="e5215-127">3: Install MongoDB</span></span>
<span data-ttu-id="e5215-128">Om du vill kunna använda det här exemplet måste du [installera MongoDB](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="e5215-128">To successfully use this sample, you must [install MongoDB](http://www.mongodb.org).</span></span> <span data-ttu-id="e5215-129">I det här exemplet använder du MongoDB för att göra dina REST-API beständiga över serverinstanser.</span><span class="sxs-lookup"><span data-stu-id="e5215-129">In this sample, you use MongoDB to make your REST API persistent across server instances.</span></span>

> [!NOTE]
> <span data-ttu-id="e5215-130">I den här artikeln förutsätter vi att du använder standardslutpunkterna för installation och server för MongoDB: mongodb://localhost.</span><span class="sxs-lookup"><span data-stu-id="e5215-130">In this article, we assume that you use the default installation and server endpoints for MongoDB: mongodb://localhost.</span></span>
> 
> 

## <a name="4-install-the-restify-modules-in-your-web-api"></a><span data-ttu-id="e5215-131">4: Installera restify-modulerna i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="e5215-131">4: Install the restify modules in your web API</span></span>
<span data-ttu-id="e5215-132">Vi använder Resitfy för att skapa vårt REST-API.</span><span class="sxs-lookup"><span data-stu-id="e5215-132">We use Resitfy to build our REST API.</span></span> <span data-ttu-id="e5215-133">Restify är ett minimalt och flexibelt Node.js programramverk som härleds från Express.</span><span class="sxs-lookup"><span data-stu-id="e5215-133">Restify is a minimal and flexible Node.js application framework that's derived from Express.</span></span> <span data-ttu-id="e5215-134">Restify har en kraftfull uppsättning funktioner som du kan använda för att skapa REST API: er utöver Connect.</span><span class="sxs-lookup"><span data-stu-id="e5215-134">Restify has a robust set of features that you can use to build REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="e5215-135">Installera restify</span><span class="sxs-lookup"><span data-stu-id="e5215-135">Install restify</span></span>
1.  <span data-ttu-id="e5215-136">I Kommandotolken, ändra katalogen till **azuread**:</span><span class="sxs-lookup"><span data-stu-id="e5215-136">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

    <span data-ttu-id="e5215-137">Om den **azuread** katalogen inte finns, skapar du den:</span><span class="sxs-lookup"><span data-stu-id="e5215-137">If the **azuread** directory does not exist, create it:</span></span>

    `mkdir azuread`

2.  <span data-ttu-id="e5215-138">Installera restify:</span><span class="sxs-lookup"><span data-stu-id="e5215-138">Install restify:</span></span>

    `npm install restify`

    <span data-ttu-id="e5215-139">Kommandots utdata ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="e5215-139">The output of this command should look like this:</span></span>

    ```
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
    └── bunyan@0.22.0(mv@0.0.5)
    ```

#### <a name="did-you-get-an-error"></a><span data-ttu-id="e5215-140">Fick du ett felmeddelande?</span><span class="sxs-lookup"><span data-stu-id="e5215-140">Did you get an error?</span></span>
<span data-ttu-id="e5215-141">I vissa operativsystem när du använder den `npm` kommandot kan du se meddelandet: `Error: EPERM, chmod '/usr/local/bin/..'`.</span><span class="sxs-lookup"><span data-stu-id="e5215-141">On some operating systems, when you use the `npm` command, you might see this message: `Error: EPERM, chmod '/usr/local/bin/..'`.</span></span> <span data-ttu-id="e5215-142">Felet följs av en begäran om att du försöker köra kontot som administratör.</span><span class="sxs-lookup"><span data-stu-id="e5215-142">The error is followed by a request that you try running the account as an administrator.</span></span> <span data-ttu-id="e5215-143">Om detta inträffar kan du använda kommandot `sudo` att köra `npm` på en högre Privilegienivå.</span><span class="sxs-lookup"><span data-stu-id="e5215-143">If this occurs, use the command `sudo` to run `npm` at a higher privilege level.</span></span>

#### <a name="did-you-get-an-error-related-to-dtrace"></a><span data-ttu-id="e5215-144">Fick du ett fel som rör DTrace?</span><span class="sxs-lookup"><span data-stu-id="e5215-144">Did you get an error related to DTrace?</span></span>
<span data-ttu-id="e5215-145">När du installerar restify kan det hända att det här meddelandet:</span><span class="sxs-lookup"><span data-stu-id="e5215-145">When you install restify, you might see this message:</span></span>

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: two
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

<span data-ttu-id="e5215-146">Restify har en kraftfull mekanism för att spåra REST-anrop med DTrace.</span><span class="sxs-lookup"><span data-stu-id="e5215-146">Restify has a powerful mechanism to trace REST calls by using DTrace.</span></span> <span data-ttu-id="e5215-147">DTrace är inte tillgänglig på många operativsystem.</span><span class="sxs-lookup"><span data-stu-id="e5215-147">However, DTrace is not available on many operating systems.</span></span> <span data-ttu-id="e5215-148">Du kan ignorera det här felmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="e5215-148">You can safely ignore this error message.</span></span>


## <a name="5-install-passportjs-in-your-web-api"></a><span data-ttu-id="e5215-149">5: Installera Passport.js i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="e5215-149">5: Install Passport.js in your web API</span></span>
1.  <span data-ttu-id="e5215-150">I Kommandotolken, ändra katalogen till **azuread**.</span><span class="sxs-lookup"><span data-stu-id="e5215-150">At the command prompt, change the directory to **azuread**.</span></span>

2.  <span data-ttu-id="e5215-151">Installera Passport.js:</span><span class="sxs-lookup"><span data-stu-id="e5215-151">Install Passport.js:</span></span>

    `npm install passport`

    <span data-ttu-id="e5215-152">Resultatet av kommandot ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="e5215-152">The output of the command should look like this:</span></span>

    ```
     passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3
    ```

## <a name="6-add-passport-azure-ad-to-your-web-api"></a><span data-ttu-id="e5215-153">6: Lägg till passport-azure-ad i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="e5215-153">6: Add passport-azure-ad to your web API</span></span>
<span data-ttu-id="e5215-154">Lägg sedan till OAuth-strategin genom att använda passport-azuread.</span><span class="sxs-lookup"><span data-stu-id="e5215-154">Next, add the OAuth strategy, by using passport-azuread.</span></span> <span data-ttu-id="e5215-155">`passport-azuread`är en uppsättning strategier som ansluter Azure AD med Passport.</span><span class="sxs-lookup"><span data-stu-id="e5215-155">`passport-azuread` is a suite of strategies that connect Azure AD with Passport.</span></span> <span data-ttu-id="e5215-156">Vi använder den här strategin för ägar-token i REST API-exemplet.</span><span class="sxs-lookup"><span data-stu-id="e5215-156">We use this strategy for bearer tokens in this REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="e5215-157">Även om OAuth 2.0 erbjuder ett ramverk där alla kända token-typer kan utfärdas, används ofta för vissa typer av token.</span><span class="sxs-lookup"><span data-stu-id="e5215-157">Although OAuth 2.0 provides a framework in which any known token type can be issued, certain token types are commonly used.</span></span> <span data-ttu-id="e5215-158">Ägar-token används ofta för att skydda slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="e5215-158">Bearer tokens are commonly used to protect endpoints.</span></span> <span data-ttu-id="e5215-159">Ägar-token är den mest typ token i OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="e5215-159">Bearer tokens are the most widely issued type of token in OAuth 2.0.</span></span> <span data-ttu-id="e5215-160">Många OAuth 2.0 olika implementeringar utgår ifrån att ägar-token är den enda typ av token som utfärdas.</span><span class="sxs-lookup"><span data-stu-id="e5215-160">Many OAuth 2.0 implementations assume that bearer tokens are the only type of token issued.</span></span>
> 
> 

1.  <span data-ttu-id="e5215-161">I Kommandotolken, ändra katalogen till **azuread**.</span><span class="sxs-lookup"><span data-stu-id="e5215-161">At a command prompt, change the directory to **azuread**.</span></span>

    `cd azuread`

2.  <span data-ttu-id="e5215-162">Installera Passport.js `passport-azure-ad` modulen:</span><span class="sxs-lookup"><span data-stu-id="e5215-162">Install the Passport.js `passport-azure-ad` module:</span></span>

    `npm install passport-azure-ad`

    <span data-ttu-id="e5215-163">Resultatet av kommandot ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="e5215-163">The output of the command should look like this:</span></span>

    ```
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
    ```

## <a name="7-add-mongodb-modules-to-your-web-api"></a><span data-ttu-id="e5215-164">7: lägga till MongoDB-moduler i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="e5215-164">7: Add MongoDB modules to your web API</span></span>
<span data-ttu-id="e5215-165">I det här exemplet använder vi MongoDB som våra datalagret.</span><span class="sxs-lookup"><span data-stu-id="e5215-165">In this sample, we use MongoDB as our data store.</span></span> 

1.  <span data-ttu-id="e5215-166">Installera Mongoose ett vanligt plugin-program att hantera modeller och scheman:</span><span class="sxs-lookup"><span data-stu-id="e5215-166">Install Mongoose, a widely used plug-in, to manage models and schemas:</span></span> 

    `npm install mongoose`

2.  <span data-ttu-id="e5215-167">Installera databasdrivrutinen för MongoDB, som också kallas MongoDB:</span><span class="sxs-lookup"><span data-stu-id="e5215-167">Install the database driver for MongoDB, which is also called MongoDB:</span></span>

    `npm install mongodb`

## <a name="8-install-additional-modules"></a><span data-ttu-id="e5215-168">8: installera ytterligare moduler</span><span class="sxs-lookup"><span data-stu-id="e5215-168">8: Install additional modules</span></span>
<span data-ttu-id="e5215-169">Installera de resterande modulerna som krävs.</span><span class="sxs-lookup"><span data-stu-id="e5215-169">Install the remaining required modules.</span></span>

1.  <span data-ttu-id="e5215-170">I Kommandotolken, ändra katalogen till **azuread**:</span><span class="sxs-lookup"><span data-stu-id="e5215-170">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="e5215-171">Ange följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="e5215-171">Enter the following commands.</span></span> <span data-ttu-id="e5215-172">Kommandona installera följande moduler i katalogen node_modules:</span><span class="sxs-lookup"><span data-stu-id="e5215-172">The commands install the following modules in your node_modules directory:</span></span>

    *   `npm install crypto`
    *   `npm install assert-plus`
    *   `npm install posix-getopt`
    *   `npm install util`
    *   `npm install path`
    *   `npm install connect`
    *   `npm install xml-crypto`
    *   `npm install xml2js`
    *   `npm install xmldom`
    *   `npm install async`
    *   `npm install request`
    *   `npm install underscore`
    *   `npm install grunt-contrib-jshint@0.1.1`
    *   `npm install grunt-contrib-nodeunit@0.1.2`
    *   `npm install grunt-contrib-watch@0.2.0`
    *   `npm install grunt@0.4.1`
    *   `npm install xtend@2.0.3`
    *   `npm install bunyan`
    *   `npm update`

## <a name="9-create-a-serverjs-file-for-your-dependencies"></a><span data-ttu-id="e5215-173">9: skapa en Server.js-fil för dina beroenden</span><span class="sxs-lookup"><span data-stu-id="e5215-173">9: Create a Server.js file for your dependencies</span></span>
<span data-ttu-id="e5215-174">En Server.js-fil innehåller flesta av funktionerna för web API-servern.</span><span class="sxs-lookup"><span data-stu-id="e5215-174">A Server.js file holds the majority of the functionality for your web API server.</span></span> <span data-ttu-id="e5215-175">Lägg till de flesta av din kod till den här filen.</span><span class="sxs-lookup"><span data-stu-id="e5215-175">Add most of your code to this file.</span></span> <span data-ttu-id="e5215-176">Produktion ska refactor du funktionerna i mindre filer som för separata vägar och styrenheter.</span><span class="sxs-lookup"><span data-stu-id="e5215-176">For production purposes, you can refactor the functionality into smaller files, like for separate routes and controllers.</span></span> <span data-ttu-id="e5215-177">I den här artikeln använder vi Server.js för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="e5215-177">In this article, we use Server.js for this purpose.</span></span>

1.  <span data-ttu-id="e5215-178">I Kommandotolken, ändra katalogen till **azuread**:</span><span class="sxs-lookup"><span data-stu-id="e5215-178">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="e5215-179">Med hjälp av ett redigeringsprogram, skapa en Server.js-fil.</span><span class="sxs-lookup"><span data-stu-id="e5215-179">Using an editor of your choice, create a Server.js file.</span></span> <span data-ttu-id="e5215-180">Lägg till följande information i filen:</span><span class="sxs-lookup"><span data-stu-id="e5215-180">Add the following information to the file:</span></span>

    ```Javascript
    'use strict';
    /**
    * Module dependencies.
    */
    var util = require('util');
    var assert = require('assert-plus');
    var mongoose = require('mongoose/');
    var bunyan = require('bunyan');
    var restify = require('restify');
    var config = require('./config');
    var passport = require('passport');
    var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
    ```

3.  <span data-ttu-id="e5215-181">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="e5215-181">Save the file.</span></span> <span data-ttu-id="e5215-182">Du kommer tillbaka till den inom kort.</span><span class="sxs-lookup"><span data-stu-id="e5215-182">You will return to it shortly.</span></span>

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a><span data-ttu-id="e5215-183">10: skapa en konfigurationsfil för att lagra dina Azure AD-inställningar</span><span class="sxs-lookup"><span data-stu-id="e5215-183">10: Create a config file to store your Azure AD settings</span></span>
<span data-ttu-id="e5215-184">Den här kodfilen skickar konfigurationsparametrarna från din Azure AD-portalen till Passport.js.</span><span class="sxs-lookup"><span data-stu-id="e5215-184">This code file passes the configuration parameters from your Azure AD portal to Passport.js.</span></span> <span data-ttu-id="e5215-185">Du har skapat konfigurationsvärdena när du har lagt till webb-API i portalen i början av artikeln.</span><span class="sxs-lookup"><span data-stu-id="e5215-185">You created these configuration values when you added the web API to the portal at the beginning of the article.</span></span> <span data-ttu-id="e5215-186">När du har kopierat koden förklarar vi vad du ska ange i värdena för dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="e5215-186">After you copy the code, we'll explain what to put in the values of these parameters.</span></span>

1.  <span data-ttu-id="e5215-187">I Kommandotolken, ändra katalogen till **azuread**:</span><span class="sxs-lookup"><span data-stu-id="e5215-187">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="e5215-188">Skapa en Config.js-fil i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="e5215-188">In an editor, create a Config.js file.</span></span> <span data-ttu-id="e5215-189">Lägg till följande information:</span><span class="sxs-lookup"><span data-stu-id="e5215-189">Add the following information:</span></span>

    ```Javascript
    // Don't commit this file to your public repos. This config is for first-run.
    exports.creds = {
    mongoose_auth_local: 'mongodb://localhost/tasklist', // Your Mongo auth URI goes here.
    issuer: 'https://sts.windows.net/**<your application id>**/',
    audience: '<your redirect URI>',
    identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For Microsoft, you should never need to change this.
    };

    ```



### <a name="required-values"></a><span data-ttu-id="e5215-190">Värden som krävs</span><span class="sxs-lookup"><span data-stu-id="e5215-190">Required values</span></span>

*   <span data-ttu-id="e5215-191">**IdentityMetadata**: det är där `passport-azure-ad` letar efter konfigurationsdata för identitetsprovider (IDP) och nycklarna att validera JSON Web token (JWTs).</span><span class="sxs-lookup"><span data-stu-id="e5215-191">**IdentityMetadata**: This is where `passport-azure-ad` looks for your configuration data for the identity provider (IDP) and the keys to validate the JSON Web Tokens (JWTs).</span></span> <span data-ttu-id="e5215-192">Om du använder Azure AD kan vill du förmodligen inte ändra den här.</span><span class="sxs-lookup"><span data-stu-id="e5215-192">If you are using Azure AD, you probably don't want to change this.</span></span>

*   <span data-ttu-id="e5215-193">**målgruppen**: din omdirigerings-URI från portalen.</span><span class="sxs-lookup"><span data-stu-id="e5215-193">**audience**: Your redirect URI from the portal.</span></span>

> [!NOTE]
> <span data-ttu-id="e5215-194">Distribuera dina nycklar med återkommande intervall.</span><span class="sxs-lookup"><span data-stu-id="e5215-194">Roll your keys at frequent intervals.</span></span> <span data-ttu-id="e5215-195">Var noga med att du alltid hämta från URL: en ”openid_keys” och att appen har åtkomst till Internet.</span><span class="sxs-lookup"><span data-stu-id="e5215-195">Be sure that you always pull from the "openid_keys" URL, and that the app can access the Internet.</span></span>
> 
> 

## <a name="11-add-the-configuration-to-your-serverjs-file"></a><span data-ttu-id="e5215-196">11: lägga till konfigurationen i filen Server.js</span><span class="sxs-lookup"><span data-stu-id="e5215-196">11: Add the configuration to your Server.js file</span></span>
<span data-ttu-id="e5215-197">Programmet måste läsa värdena från den config-fil som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="e5215-197">Your application needs to read the values from the config file you just created.</span></span> <span data-ttu-id="e5215-198">Lägg till .config-filen som en nödvändig resurs i ditt program.</span><span class="sxs-lookup"><span data-stu-id="e5215-198">Add the .config file as a required resource in your application.</span></span> <span data-ttu-id="e5215-199">Ange de globala variablerna till dem som finns i Config.js.</span><span class="sxs-lookup"><span data-stu-id="e5215-199">Set the global variables to those that are in Config.js.</span></span>

1.  <span data-ttu-id="e5215-200">I Kommandotolken, ändra katalogen till **azuread**:</span><span class="sxs-lookup"><span data-stu-id="e5215-200">At the command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="e5215-201">Öppna Server.js i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="e5215-201">In an editor, open Server.js.</span></span> <span data-ttu-id="e5215-202">Lägg till följande information:</span><span class="sxs-lookup"><span data-stu-id="e5215-202">Add the following information:</span></span>

    ```Javascript
    var config = require('./config');
    ```

3.  <span data-ttu-id="e5215-203">Lägg till ett nytt avsnitt i Server.js:</span><span class="sxs-lookup"><span data-stu-id="e5215-203">Add a new section to Server.js:</span></span>

    ```Javascript
    // Pass these options in to the ODICBearerStrategy.
    var options = {
    // The URL of the metadata document for your app. Put the keys for token validation from the URL found in the jwks_uri tag in the metadata.
    identityMetadata: config.creds.identityMetadata,
    issuer: config.creds.issuer,
    audience: config.creds.audience
    };
    // Array to hold signed-in users and the current signed-in user (owner).
    var users = [];
    var owner = null;
    // Your logger
    var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
    });
    ```

## <a name="12-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="e5215-204">12: lägga till informationen om MongoDB-modellen och schemat genom att använda Mongoose</span><span class="sxs-lookup"><span data-stu-id="e5215-204">12: Add the MongoDB model and schema information by using Mongoose</span></span>
<span data-ttu-id="e5215-205">Anslut sedan dessa tre filer i en REST API-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e5215-205">Next, connect these three files in a REST API service.</span></span>

<span data-ttu-id="e5215-206">Vi använder MongoDB för att lagra våra uppgifter i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e5215-206">In this article, we use MongoDB to store our tasks.</span></span> <span data-ttu-id="e5215-207">Diskuterar vi detta i *steg 4*.</span><span class="sxs-lookup"><span data-stu-id="e5215-207">We discuss this in *step 4*.</span></span>

<span data-ttu-id="e5215-208">I den Config.js-fil som du skapade i steg 11 databasen kallas *tasklist*.</span><span class="sxs-lookup"><span data-stu-id="e5215-208">In the Config.js file you created in step 11, your database is called *tasklist*.</span></span> <span data-ttu-id="e5215-209">Som var det du angav i slutet av din mongoose_auth_local anslutnings-URL.</span><span class="sxs-lookup"><span data-stu-id="e5215-209">That was what you put at the end of your mongoose_auth_local connection URL.</span></span> <span data-ttu-id="e5215-210">Du behöver inte skapa den här databasen i förväg i MongoDB.</span><span class="sxs-lookup"><span data-stu-id="e5215-210">You don't need to create this database beforehand in MongoDB.</span></span> <span data-ttu-id="e5215-211">Databasen skapas första gången du kör serverprogrammet (förutsatt att databasen inte redan finns).</span><span class="sxs-lookup"><span data-stu-id="e5215-211">The database is created on the first run of your server application (assuming the database does not already exist).</span></span>

<span data-ttu-id="e5215-212">Du har redan nämnt servern vilken MongoDB-databas som ska användas.</span><span class="sxs-lookup"><span data-stu-id="e5215-212">You've told the server what MongoDB database to use.</span></span> <span data-ttu-id="e5215-213">Därefter måste du skriva ytterligare kod för att skapa modellen och schemat för din server uppgifter.</span><span class="sxs-lookup"><span data-stu-id="e5215-213">Next, you need to write some additional code to create the model and schema for your server's tasks.</span></span>

### <a name="the-model"></a><span data-ttu-id="e5215-214">Modellen</span><span class="sxs-lookup"><span data-stu-id="e5215-214">The model</span></span>
<span data-ttu-id="e5215-215">Schemamodellen är väldigt enkla.</span><span class="sxs-lookup"><span data-stu-id="e5215-215">The schema model is very basic.</span></span> <span data-ttu-id="e5215-216">Du kan expandera den om du behöver.</span><span class="sxs-lookup"><span data-stu-id="e5215-216">You can expand it if you need to.</span></span> 

<span data-ttu-id="e5215-217">Schemamodellen har dessa värden:</span><span class="sxs-lookup"><span data-stu-id="e5215-217">The schema model has these values:</span></span>

*   <span data-ttu-id="e5215-218">**NAMNET**.</span><span class="sxs-lookup"><span data-stu-id="e5215-218">**NAME**.</span></span> <span data-ttu-id="e5215-219">Den person som tilldelats aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="e5215-219">The person assigned to the task.</span></span> <span data-ttu-id="e5215-220">Det här är en **sträng** värde.</span><span class="sxs-lookup"><span data-stu-id="e5215-220">This is a **string** value.</span></span>
*   <span data-ttu-id="e5215-221">**UPPGIFTEN**.</span><span class="sxs-lookup"><span data-stu-id="e5215-221">**TASK**.</span></span> <span data-ttu-id="e5215-222">Namnet på aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="e5215-222">The name of the task.</span></span> <span data-ttu-id="e5215-223">Det här är en **sträng** värde.</span><span class="sxs-lookup"><span data-stu-id="e5215-223">This is a **string** value.</span></span>
*   <span data-ttu-id="e5215-224">**DATUM**.</span><span class="sxs-lookup"><span data-stu-id="e5215-224">**DATE**.</span></span> <span data-ttu-id="e5215-225">Det datum då uppgiften förfaller.</span><span class="sxs-lookup"><span data-stu-id="e5215-225">The date that the task is due.</span></span> <span data-ttu-id="e5215-226">Det här är en **datetime** värde.</span><span class="sxs-lookup"><span data-stu-id="e5215-226">This is a **datetime** value.</span></span>
*   <span data-ttu-id="e5215-227">**SLUTFÖRA**.</span><span class="sxs-lookup"><span data-stu-id="e5215-227">**COMPLETED**.</span></span> <span data-ttu-id="e5215-228">Om aktiviteten är slutförd.</span><span class="sxs-lookup"><span data-stu-id="e5215-228">Whether the task is completed.</span></span> <span data-ttu-id="e5215-229">Det här är en **booleskt** värde.</span><span class="sxs-lookup"><span data-stu-id="e5215-229">This is a **Boolean** value.</span></span>

### <a name="create-the-schema-in-the-code"></a><span data-ttu-id="e5215-230">Skapa schemat i koden</span><span class="sxs-lookup"><span data-stu-id="e5215-230">Create the schema in the code</span></span>
1.  <span data-ttu-id="e5215-231">I Kommandotolken, ändra katalogen till **azuread**:</span><span class="sxs-lookup"><span data-stu-id="e5215-231">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="e5215-232">Öppna Server.js i redigeringsprogram.</span><span class="sxs-lookup"><span data-stu-id="e5215-232">In your editor, open Server.js.</span></span> <span data-ttu-id="e5215-233">Lägg till följande information under konfigurationsposten:</span><span class="sxs-lookup"><span data-stu-id="e5215-233">Below the configuration entry, add the following information:</span></span>

    ```Javascript
    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');
    ```

<span data-ttu-id="e5215-234">Den här koden ansluter till MongoDB-servern.</span><span class="sxs-lookup"><span data-stu-id="e5215-234">This code connects to the MongoDB server.</span></span> <span data-ttu-id="e5215-235">Den returnerar även ett schemaobjekt.</span><span class="sxs-lookup"><span data-stu-id="e5215-235">It also returns a schema object.</span></span>

#### <a name="using-the-schema-create-your-model-in-the-code"></a><span data-ttu-id="e5215-236">Använd schemat för att skapa modellen i koden</span><span class="sxs-lookup"><span data-stu-id="e5215-236">Using the schema, create your model in the code</span></span>
<span data-ttu-id="e5215-237">Lägg till följande kod under den föregående kod:</span><span class="sxs-lookup"><span data-stu-id="e5215-237">Below the preceding code, add the following code:</span></span>

```Javascript
// Create a basic schema to store your tasks and users.
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

<span data-ttu-id="e5215-238">Som du ser från koden skapar du först ditt schema.</span><span class="sxs-lookup"><span data-stu-id="e5215-238">As you can tell from the code, first you create your schema.</span></span> <span data-ttu-id="e5215-239">Därefter skapar du ett modellobjekt.</span><span class="sxs-lookup"><span data-stu-id="e5215-239">Next, you create a model object.</span></span> <span data-ttu-id="e5215-240">Du använder model-objektet för att lagra data i hela koden när du definierar din **vägar**.</span><span class="sxs-lookup"><span data-stu-id="e5215-240">You use the model object to store your data throughout the code when you define your **routes**.</span></span>

## <a name="13-add-your-routes-for-your-task-rest-api-server"></a><span data-ttu-id="e5215-241">13: lägga till din vägar för aktiviteten REST API-servern</span><span class="sxs-lookup"><span data-stu-id="e5215-241">13: Add your routes for your task REST API server</span></span>
<span data-ttu-id="e5215-242">Lägg till de vägar du ska använda för REST API-servern nu när du har en databasmodell att arbeta med.</span><span class="sxs-lookup"><span data-stu-id="e5215-242">Now that you have a database model to work with, add the routes you'll use for your REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="e5215-243">Om vägar i restify</span><span class="sxs-lookup"><span data-stu-id="e5215-243">About routes in restify</span></span>
<span data-ttu-id="e5215-244">Vägar i restify fungerar på samma sätt de göra när du använder Express-stacken.</span><span class="sxs-lookup"><span data-stu-id="e5215-244">Routes in restify work exactly the same way they do when you use the Express stack.</span></span> <span data-ttu-id="e5215-245">Du definierar vägar genom att använda den URI som du förväntar dig att klientprogram anropar.</span><span class="sxs-lookup"><span data-stu-id="e5215-245">You define routes by using the URI that you expect the client applications to call.</span></span> <span data-ttu-id="e5215-246">Vanligtvis kan du definiera vägarna i en separat fil.</span><span class="sxs-lookup"><span data-stu-id="e5215-246">Usually, you define your routes in a separate file.</span></span> <span data-ttu-id="e5215-247">I den här självstudiekursen lägger vi vårt vägar i Server.js.</span><span class="sxs-lookup"><span data-stu-id="e5215-247">In this tutorial, we put our routes in Server.js.</span></span> <span data-ttu-id="e5215-248">För produktion rekommenderar vi att du strukturerar vägar till sina egna fil.</span><span class="sxs-lookup"><span data-stu-id="e5215-248">For production use, we recommend that you factor routes into their own file.</span></span>

<span data-ttu-id="e5215-249">Ett typiskt mönster för en restify-väg är:</span><span class="sxs-lookup"><span data-stu-id="e5215-249">A typical pattern for a restify route is:</span></span>

```Javascript
function createObject(req, res, next) {
// Do work on object.
_object.name = req.params.object; // Passed value is in req.params under object.
///...
return next(); // Keep the server going.
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```


<span data-ttu-id="e5215-250">Det här är mönstret på den mest grundläggande nivån.</span><span class="sxs-lookup"><span data-stu-id="e5215-250">This is the pattern at the most basic level.</span></span> <span data-ttu-id="e5215-251">Restify (och Express) ger mycket mer ingående funktioner, t.ex. möjligheten att definiera programtyper och komplex Routning över olika slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="e5215-251">Restify (and Express) provide much deeper functionality, like the ability to define application types, and complex routing across different endpoints.</span></span>

#### <a name="add-default-routes-to-your-server"></a><span data-ttu-id="e5215-252">Lägga till standardvägar i servern</span><span class="sxs-lookup"><span data-stu-id="e5215-252">Add default routes to your server</span></span>
<span data-ttu-id="e5215-253">Lägg till grundläggande CRUD-vägarna: **skapa**, **hämta**, **uppdatera**, och **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e5215-253">Add the basic CRUD routes: **create**, **retrieve**, **update**, and **delete**.</span></span>

1.  <span data-ttu-id="e5215-254">I Kommandotolken, ändra katalogen till **azuread**:</span><span class="sxs-lookup"><span data-stu-id="e5215-254">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="e5215-255">Öppna Server.js i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="e5215-255">In an editor, open Server.js.</span></span> <span data-ttu-id="e5215-256">Nedan de databasposter du gjorde tidigare, lägger du till följande information:</span><span class="sxs-lookup"><span data-stu-id="e5215-256">Below the database entries you made earlier, add the following information:</span></span>

    ```Javascript
    /**
    *
    * APIs for your REST task server
    */
    // Create a task.
    function createTask(req, res, next) {
    // Resitify currently has a bug that doesn't allow you to set default headers.
    // These headers comply with CORS, and allow you to use MongoDB Server as your response to any origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    // Create a new task model, fill it, and save it to MongoDB.
    var _task = new Task();
    if (!req.params.task) {
    req.log.warn({
    params: p
    }, 'createTodo: missing task');
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
    /// Returns the list of TODOs that were loaded.
    function listTasks(req, res, next) {
    // Resitify currently has a bug that doesn't allow you to set default headers.
    // These headers comply with CORS, and allow us to use MongoDB Server as our response to any origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    log.info("listTasks was called for: ", owner);
    Task.find({
    owner: owner
    }).limit(20).sort('date').exec(function(err, data) {
    if (err)
    return next(err);
    if (data.length > 0) {
    log.info(data);
    }
    if (!data.length) {
    log.warn(err, "There are no tasks in the database. Add one!");
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

### <a name="add-error-handling-for-the-routes"></a><span data-ttu-id="e5215-257">Lägg till felhantering för vägarna</span><span class="sxs-lookup"><span data-stu-id="e5215-257">Add error handling for the routes</span></span>
<span data-ttu-id="e5215-258">Lägga till viss felhantering, så du kan kommunicera tillbaka till klienten om det fel som inträffade.</span><span class="sxs-lookup"><span data-stu-id="e5215-258">Add some error handling so you can communicate back to the client about the problem you encountered.</span></span>

<span data-ttu-id="e5215-259">Lägg till följande kod under den kod som du redan har skrivit:</span><span class="sxs-lookup"><span data-stu-id="e5215-259">Add the following code below the code, which you've already written:</span></span>

```Javascript
///--- Errors for communicating something more information back to the client.
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


## <a name="14-create-your-server"></a><span data-ttu-id="e5215-260">14: skapa servern</span><span class="sxs-lookup"><span data-stu-id="e5215-260">14: Create your server</span></span>
<span data-ttu-id="e5215-261">Det sista du behöver göra är att lägga till din server-instans.</span><span class="sxs-lookup"><span data-stu-id="e5215-261">The last thing to do is to add your server instance.</span></span> <span data-ttu-id="e5215-262">Serverinstansen som hanterar dina anrop.</span><span class="sxs-lookup"><span data-stu-id="e5215-262">The server instance manages your calls.</span></span>

<span data-ttu-id="e5215-263">Restify (och Express) har djupgående anpassning som du kan använda med en REST API-server.</span><span class="sxs-lookup"><span data-stu-id="e5215-263">Restify (and Express) have deep customization that you can use with a REST API server.</span></span> <span data-ttu-id="e5215-264">I den här kursen använder vi den mest grundläggande installationen.</span><span class="sxs-lookup"><span data-stu-id="e5215-264">In this tutorial, we use the most basic setup.</span></span>

```Javascript
/**
* Your server
*/
var server = restify.createServer({
name: "Microsoft Azure Active Directory TODO Server",
version: "2.0.1"
});
// Ensure that you don't drop data on uploads.
server.pre(restify.pre.pause());
// Clean up imprecise paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());
// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());
// Set a per-request Bunyan logger (with requestid filled in).
server.use(restify.requestLogger());
// Allow 5 requests/second by IP address, and burst to 10.
server.use(restify.throttle({
burst: 10,
rate: 5,
ip: true,
}));
// Use common commands, such as:
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
mapParams: true
}));
```
## <a name="15-add-the-routes-without-authentication-for-now"></a><span data-ttu-id="e5215-265">15: lägga till vägar (utan autentisering för tillfället)</span><span class="sxs-lookup"><span data-stu-id="e5215-265">15: Add the routes (without authentication, for now)</span></span>
```Javascript
/// Use CRUD to add the real handlers.
/**
/*
/* Each of these handlers is protected by your Open ID Connect Bearer strategy. Invoke 'oidc-bearer'
/* in the pasport.authenticate() method. Because REST is stateless, set 'session: false'. You 
/* don't need to maintain session state. You can experiment with removing API protection.
/* To do this, remove the passport.authenticate() method:
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
// Register a default '/' handler
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
## <a name="16-run-the-server"></a><span data-ttu-id="e5215-266">16: köra server</span><span class="sxs-lookup"><span data-stu-id="e5215-266">16: Run the server</span></span>
<span data-ttu-id="e5215-267">Det är en bra idé att testa servern innan du lägger till autentisering.</span><span class="sxs-lookup"><span data-stu-id="e5215-267">It's a good idea to test your server before you add authentication.</span></span>

<span data-ttu-id="e5215-268">Det enklaste sättet att testa din server är med hjälp av curl vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="e5215-268">The easiest way to test your server is by using curl at a command prompt.</span></span> <span data-ttu-id="e5215-269">Om du vill göra detta behöver du ett enkelt verktyg som du kan använda för att analysera utdata som JSON.</span><span class="sxs-lookup"><span data-stu-id="e5215-269">To do this, you need a simple utility that you can use to parse output as JSON.</span></span> 

1.  <span data-ttu-id="e5215-270">Installera JSON-verktyget som vi använder i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e5215-270">Install the JSON tool that we use in the following examples:</span></span>

    `$npm install -g jsontool`

    <span data-ttu-id="e5215-271">Då installeras JSON-verktyget globalt.</span><span class="sxs-lookup"><span data-stu-id="e5215-271">This installs the JSON tool globally.</span></span>

2.  <span data-ttu-id="e5215-272">Kontrollera att MongoDB-instansen körs:</span><span class="sxs-lookup"><span data-stu-id="e5215-272">Make sure that your MongoDB instance is running:</span></span>

    `$sudo mongod`

3.  <span data-ttu-id="e5215-273">Ändra katalogen till **azuread**, och kör sedan curl:</span><span class="sxs-lookup"><span data-stu-id="e5215-273">Change the directory to **azuread**, and then run curl:</span></span>

    `$ cd azuread`
    `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 2.0OK
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

4.  <span data-ttu-id="e5215-274">Lägg till en aktivitet:</span><span class="sxs-lookup"><span data-stu-id="e5215-274">To add a task:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

    <span data-ttu-id="e5215-275">Svaret bör vara:</span><span class="sxs-lookup"><span data-stu-id="e5215-275">The response should be:</span></span>

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

5.  <span data-ttu-id="e5215-276">Lista över aktiviteter för Jens:</span><span class="sxs-lookup"><span data-stu-id="e5215-276">List tasks for Brandon:</span></span>

    `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

<span data-ttu-id="e5215-277">Om dessa kommandon körs utan problem, är du redo att lägga till OAuth i REST API-servern.</span><span class="sxs-lookup"><span data-stu-id="e5215-277">If all these commands run without errors, you are ready to add OAuth to the REST API server.</span></span>

<span data-ttu-id="e5215-278">*Du har nu en REST API-server med MongoDB!*</span><span class="sxs-lookup"><span data-stu-id="e5215-278">*You now have a REST API server with MongoDB!*</span></span>

## <a name="17-add-authentication-to-your-rest-api-server"></a><span data-ttu-id="e5215-279">17: Lägg till autentisering i REST API-servern</span><span class="sxs-lookup"><span data-stu-id="e5215-279">17: Add authentication to your REST API server</span></span>
<span data-ttu-id="e5215-280">Nu när du har en aktiv REST-API, ställa in den för att använda den med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e5215-280">Now that you have a running REST API, set it up to use it with Azure AD.</span></span>

<span data-ttu-id="e5215-281">I Kommandotolken, ändra katalogen till **azuread**:</span><span class="sxs-lookup"><span data-stu-id="e5215-281">At a command prompt, change the directory to **azuread**:</span></span>

`cd azuread`

### <a name="use-the-oidcbearerstrategy-thats-included-with-passport-azure-ad"></a><span data-ttu-id="e5215-282">Använd den oidc-ägarstrategi som ingår i passport-azure-ad</span><span class="sxs-lookup"><span data-stu-id="e5215-282">Use the oidcbearerstrategy that's included with passport-azure-ad</span></span>
<span data-ttu-id="e5215-283">Hittills har du skapat en typisk REST TODO-server utan någon typ av auktorisering.</span><span class="sxs-lookup"><span data-stu-id="e5215-283">So far, you've built a typical REST TODO server without any kind of authorization.</span></span> <span data-ttu-id="e5215-284">Lägg till autentisering.</span><span class="sxs-lookup"><span data-stu-id="e5215-284">Now, add authentication.</span></span>

<span data-ttu-id="e5215-285">Ange först att du vill använda Passport.</span><span class="sxs-lookup"><span data-stu-id="e5215-285">First,  indicate that you want to use Passport.</span></span> <span data-ttu-id="e5215-286">Placera den här rättigheten efter tidigare serverkonfiguration:</span><span class="sxs-lookup"><span data-stu-id="e5215-286">Put this right after your earlier server configuration:</span></span>

```Javascript
// Start using Passport.js.

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [!TIP]
> <span data-ttu-id="e5215-287">När du skriver API: er är en bra idé att alltid länka dina data till något unikt från token som användaren inte kan förfalska.</span><span class="sxs-lookup"><span data-stu-id="e5215-287">When you write APIs, it's a good idea to always link the data to something unique from the token that the user can’t spoof.</span></span> <span data-ttu-id="e5215-288">När den här servern lagrar TODO-objekt, lagrar dem baserat på prenumerations-ID för användaren i token (anropas via token.sub).</span><span class="sxs-lookup"><span data-stu-id="e5215-288">When this server stores TODO items, it stores them based on the user subscription ID in the token (called through token.sub).</span></span> <span data-ttu-id="e5215-289">Du kan placera token.sub i fältet ”ägare”.</span><span class="sxs-lookup"><span data-stu-id="e5215-289">You put the token.sub in the “owner” field.</span></span> <span data-ttu-id="e5215-290">Detta säkerställer att endast denna användare har åtkomst till användarens TODOs.</span><span class="sxs-lookup"><span data-stu-id="e5215-290">This ensures that only that user can access the user's TODOs.</span></span> <span data-ttu-id="e5215-291">Ingen annan kan komma åt TODOs som har angetts.</span><span class="sxs-lookup"><span data-stu-id="e5215-291">No one else can access the TODOs that were entered.</span></span> <span data-ttu-id="e5215-292">Det finns inga exponering i API: et för ”ägare”.</span><span class="sxs-lookup"><span data-stu-id="e5215-292">There is no exposure in the API for “owner.”</span></span> <span data-ttu-id="e5215-293">En extern användare kan begära TODOs för andra användare, även om de autentiseras.</span><span class="sxs-lookup"><span data-stu-id="e5215-293">An external user can request other users' TODOs, even if they are authenticated.</span></span>
> 
> 

<span data-ttu-id="e5215-294">Använd sedan den öppna ID Connect ägarstrategi som medföljer `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="e5215-294">Next, use the Open ID Connect Bearer strategy that comes with `passport-azure-ad`.</span></span> <span data-ttu-id="e5215-295">Placera det efter vad du klistrade in ovan:</span><span class="sxs-lookup"><span data-stu-id="e5215-295">Put this after what you pasted earlier:</span></span>

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users.
/*
/* Because of the Passport pattern, you need to manage users and info tokens
/* with a FindorCreate() method. The method must be provided by the implementor.
/* In the following code, you autoregister any user and implement a FindById().
/* It's a good idea to do something more advanced.
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
var oidcStrategy = new OIDCBearerStrategy(options,
function(token, done) {
log.info('verifying the user');
log.info(token, 'was the token retrieved');
findById(token.sub, function(err, user) {
if (err) {
return done(err);
}
if (!user) {
// "Auto-registration"
log.info('User was added automatically, because they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

<span data-ttu-id="e5215-296">Passport använder ett liknande mönster för alla sina strategier (Twitter, Facebook och så vidare).</span><span class="sxs-lookup"><span data-stu-id="e5215-296">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on).</span></span> <span data-ttu-id="e5215-297">Alla strategigenererare följer mönstret.</span><span class="sxs-lookup"><span data-stu-id="e5215-297">All strategy writers adhere to the pattern.</span></span> <span data-ttu-id="e5215-298">Skicka en strategi för en `function()` som använder en token och `done` som parametrar.</span><span class="sxs-lookup"><span data-stu-id="e5215-298">Pass the strategy a `function()` that uses a token and `done` as parameters.</span></span> <span data-ttu-id="e5215-299">Strategin som returneras när den gör allt arbete.</span><span class="sxs-lookup"><span data-stu-id="e5215-299">The strategy is returned after it does all its work.</span></span> <span data-ttu-id="e5215-300">Lagra användaren och token så att du inte behöver fråga efter den igen.</span><span class="sxs-lookup"><span data-stu-id="e5215-300">Store the user and stash the token so you don’t need to ask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e5215-301">Föregående kod tar alla användare som kan autentisera till servern.</span><span class="sxs-lookup"><span data-stu-id="e5215-301">The preceding code takes any user that can authenticate to your server.</span></span> <span data-ttu-id="e5215-302">Detta kallas för automatisk registrering.</span><span class="sxs-lookup"><span data-stu-id="e5215-302">This is known as auto-registration.</span></span> <span data-ttu-id="e5215-303">På en produktionsserver skulle du vill låta alla i utan att behöva dem gå igenom den registreringsprocess som du väljer.</span><span class="sxs-lookup"><span data-stu-id="e5215-303">On a production server, you wouldn’t want to let anyone in without first having them go through a registration process that you choose.</span></span> <span data-ttu-id="e5215-304">Detta är oftast det mönster du ser i konsumentappar.</span><span class="sxs-lookup"><span data-stu-id="e5215-304">This is usually the pattern you see in consumer apps.</span></span> <span data-ttu-id="e5215-305">Appen kan tillåta dig att registrera med Facebook, men sedan du ombedd att ange ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="e5215-305">The app might allow you to register with Facebook, but then it asks you to enter additional information.</span></span> <span data-ttu-id="e5215-306">Om du inte använde ett kommandoradsprogram för den här självstudiekursen, kan du extrahera den e-postadressen från tokenobjektet som returneras.</span><span class="sxs-lookup"><span data-stu-id="e5215-306">If you weren't using a command-line program for this tutorial, you could extract the email from the token object that is returned.</span></span> <span data-ttu-id="e5215-307">Sedan kan du be användaren att ange ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="e5215-307">Then, you might ask the user to enter additional information.</span></span> <span data-ttu-id="e5215-308">Eftersom detta är en testserver kan du lägga till användaren direkt till den minnesintern databasen.</span><span class="sxs-lookup"><span data-stu-id="e5215-308">Because this is a test server, you add the user directly to the in-memory database.</span></span>
> 
> 

### <a name="protect-endpoints"></a><span data-ttu-id="e5215-309">Skydda slutpunkter</span><span class="sxs-lookup"><span data-stu-id="e5215-309">Protect endpoints</span></span>
<span data-ttu-id="e5215-310">Skydda slutpunkter genom att ange den **passport.authenticate()** anropa med det protokoll som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="e5215-310">Protect endpoints by specifying the **passport.authenticate()** call with the protocol that you want to use.</span></span>

<span data-ttu-id="e5215-311">Du kan redigera din vägen i serverkoden för mer avancerade alternativ:</span><span class="sxs-lookup"><span data-stu-id="e5215-311">You can edit your route in your server code for a more advanced option:</span></span>

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again"></a><span data-ttu-id="e5215-312">18: kör serverprogrammet igen</span><span class="sxs-lookup"><span data-stu-id="e5215-312">18: Run your server application again</span></span>
<span data-ttu-id="e5215-313">Använd curl igen för att se om du har OAuth 2.0 skydd mot dina slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="e5215-313">Use curl again to see if you have OAuth 2.0 protection against your endpoints.</span></span> <span data-ttu-id="e5215-314">Gör detta innan du kör någon av dina klient-SDK: er mot den här slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="e5215-314">Do this before you run any of your client SDKs against this endpoint.</span></span> <span data-ttu-id="e5215-315">Rubriker som returneras bör du informera om autentiseringen fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="e5215-315">The headers returned should tell you if your authentication is working correctly.</span></span>

1.  <span data-ttu-id="e5215-316">Kontrollera att MongoDB-isntance körs:</span><span class="sxs-lookup"><span data-stu-id="e5215-316">Make sure that your MongoDB isntance is running:</span></span>

    `$sudo mongod`

2.  <span data-ttu-id="e5215-317">Ändra till den **azuread** katalogen och Använd curl:</span><span class="sxs-lookup"><span data-stu-id="e5215-317">Change to the **azuread** directory, and then use curl:</span></span>

    `$ cd azuread`

    `$ node server.js`

3.  <span data-ttu-id="e5215-318">Prova med ett grundläggande INLÄGG:</span><span class="sxs-lookup"><span data-stu-id="e5215-318">Try a basic POST:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

<span data-ttu-id="e5215-319">Ett 401-svar som anger att Passport-lagret försöker omdirigera till slutpunkten för auktorisering, vilket är exakt vad du vill.</span><span class="sxs-lookup"><span data-stu-id="e5215-319">A 401 response indicates that the Passport layer is trying to redirect to the authorize endpoint, which is exactly what you want.</span></span>

<span data-ttu-id="e5215-320">*Du har nu en REST API-tjänst som använder OAuth 2.0!*</span><span class="sxs-lookup"><span data-stu-id="e5215-320">*You now have a REST API service that uses OAuth 2.0!*</span></span>

<span data-ttu-id="e5215-321">Du har gjort så mycket du kan med den här servern utan att använda en OAuth 2.0-kompatibel klient.</span><span class="sxs-lookup"><span data-stu-id="e5215-321">You've gone as far as you can with this server without using an OAuth 2.0-compatible client.</span></span> <span data-ttu-id="e5215-322">För att behöver du granska en ytterligare vägledning.</span><span class="sxs-lookup"><span data-stu-id="e5215-322">For that, you will need to review an additional tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5215-323">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5215-323">Next steps</span></span>
<span data-ttu-id="e5215-324">Referens tillhandahålls det slutförda exemplet (utan dina konfigurationsvärden) som [en .zip-fil](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="e5215-324">For reference, the completed sample (without your configuration values) is provided as [a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip).</span></span> <span data-ttu-id="e5215-325">Du kan också klona det från GitHub:</span><span class="sxs-lookup"><span data-stu-id="e5215-325">You also can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

<span data-ttu-id="e5215-326">Du kan nu gå vidare till mer avancerade avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e5215-326">Now, you can move on to more advanced topics.</span></span> <span data-ttu-id="e5215-327">Du kanske vill prova [skydda en Node.js-webbapp med v2.0-slutpunkten](active-directory-v2-devquickstarts-node-web.md).</span><span class="sxs-lookup"><span data-stu-id="e5215-327">You might want to try [Secure a Node.js web app using the v2.0 endpoint](active-directory-v2-devquickstarts-node-web.md).</span></span>

<span data-ttu-id="e5215-328">Här följer några ytterligare resurser:</span><span class="sxs-lookup"><span data-stu-id="e5215-328">Here are some additional resources:</span></span>

* [<span data-ttu-id="e5215-329">Utvecklarhandbok för Azure AD v2.0</span><span class="sxs-lookup"><span data-stu-id="e5215-329">Azure AD v2.0 developer guide</span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="e5215-330">Stacken spill ”azure-active-directory” tagg</span><span class="sxs-lookup"><span data-stu-id="e5215-330">Stack Overflow "azure-active-directory" tag</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a><span data-ttu-id="e5215-331">Hämta säkerhetsuppdateringar för våra produkter</span><span class="sxs-lookup"><span data-stu-id="e5215-331">Get security updates for our products</span></span>
<span data-ttu-id="e5215-332">Vi rekommenderar att du loggar som ska meddelas när säkerhetsincidenter.</span><span class="sxs-lookup"><span data-stu-id="e5215-332">We encourage you to sign up to be notified when security incidents occur.</span></span> <span data-ttu-id="e5215-333">På den [Microsoft tekniska säkerhetsmeddelanden](https://technet.microsoft.com/security/dd252948) kan prenumerera på rekommendationerna säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="e5215-333">On the [Microsoft Technical Security Notifications](https://technet.microsoft.com/security/dd252948) page, subscribe to Security Advisories Alerts.</span></span>

