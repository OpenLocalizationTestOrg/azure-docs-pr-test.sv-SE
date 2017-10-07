---
title: aaaSecure Azure Active Directory v2.0 webb-API med Node.js | Microsoft Docs
description: "Lär dig hur toobuild en Node.js webb-API som accepterar token från en personligt Microsoft-konto och från arbets-eller skolkonton."
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
ms.openlocfilehash: 219e324cca11e107186b7e5f995589b9260af8a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-web-api-by-using-nodejs"></a><span data-ttu-id="8f8d9-103">Säkra ett webb-API med Node.js</span><span class="sxs-lookup"><span data-stu-id="8f8d9-103">Secure a web API by using Node.js</span></span>
> [!NOTE]
> <span data-ttu-id="8f8d9-104">Inte alla Azure Active Directory-scenarier och funktioner fungerar med hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-104">Not all Azure Active Directory scenarios and features work with hello v2.0 endpoint.</span></span> <span data-ttu-id="8f8d9-105">toodetermine om du ska använda hello v2.0-slutpunkten eller hello v1.0 slutpunkt och Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="8f8d9-105">toodetermine whether you should use hello v2.0 endpoint or hello v1.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

<span data-ttu-id="8f8d9-106">När du använder hello Azure Active Directory (AD Azure) v2.0-slutpunkten kan du använda [OAuth 2.0](active-directory-v2-protocols.md) access token tooprotect ditt webb-API.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-106">When you use hello Azure Active Directory (Azure AD) v2.0 endpoint, you can use [OAuth 2.0](active-directory-v2-protocols.md) access tokens tooprotect your web API.</span></span> <span data-ttu-id="8f8d9-107">Åtkomst-token, användare som har både en personliga Microsoft-konto och arbete eller skolkonton kan på ett säkert sätt komma åt ditt webb-API med OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-107">With OAuth 2.0 access tokens, users who have both a personal Microsoft account and work or school accounts can securely access your web API.</span></span>

<span data-ttu-id="8f8d9-108">*Passport* är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-108">*Passport* is authentication middleware for Node.js.</span></span> <span data-ttu-id="8f8d9-109">Flexibel och modulära, Passport kan släppas diskret in i alla Express-baserade eller restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-109">Flexible and modular, Passport can be unobtrusively dropped into any Express-based or restify web application.</span></span> <span data-ttu-id="8f8d9-110">I Passport, en omfattande uppsättning strategier stöder autentisering med ett användarnamn och lösenord, Facebook, Twitter eller andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-110">In Passport, a comprehensive set of strategies support authentication by using a username and password, Facebook, Twitter, or other options.</span></span> <span data-ttu-id="8f8d9-111">Vi har utvecklat en strategi för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-111">We have developed a strategy for Azure AD.</span></span> <span data-ttu-id="8f8d9-112">I den här artikeln visar vi du hur tooinstall hello modulen och lägger sedan till hello Azure AD `passport-azure-ad` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-112">In this article, we show you how tooinstall hello module, and then add hello Azure AD `passport-azure-ad` plug-in.</span></span>

## <a name="download"></a><span data-ttu-id="8f8d9-113">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="8f8d9-113">Download</span></span>
<span data-ttu-id="8f8d9-114">hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).</span><span class="sxs-lookup"><span data-stu-id="8f8d9-114">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).</span></span> <span data-ttu-id="8f8d9-115">toofollow hello självstudier, kan du [hämta hello appens stomme som en .zip-fil](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), eller klona hello stommen:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-115">toofollow hello tutorial, you can [download hello app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

<span data-ttu-id="8f8d9-116">Du kan också få hello slutförts programmet hello slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-116">You also can get hello completed application at hello end of this tutorial.</span></span>

## <a name="1-register-an-app"></a><span data-ttu-id="8f8d9-117">1: registrera en app</span><span class="sxs-lookup"><span data-stu-id="8f8d9-117">1: Register an app</span></span>
<span data-ttu-id="8f8d9-118">Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa [dessa detaljerade steg](active-directory-v2-app-registration.md) tooregister en app.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-118">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow [these detailed steps](active-directory-v2-app-registration.md) tooregister an app.</span></span> <span data-ttu-id="8f8d9-119">Se till att du:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-119">Make sure you:</span></span>

* <span data-ttu-id="8f8d9-120">Kopiera hello **program-Id** tilldelade tooyour app.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-120">Copy hello **Application Id** assigned tooyour app.</span></span> <span data-ttu-id="8f8d9-121">Du behöver det för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-121">You'll need it for this tutorial.</span></span>
* <span data-ttu-id="8f8d9-122">Lägg till hello **Mobile** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-122">Add hello **Mobile** platform for your app.</span></span>
* <span data-ttu-id="8f8d9-123">Kopiera hello **omdirigerings-URI** från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-123">Copy hello **Redirect URI** from hello portal.</span></span> <span data-ttu-id="8f8d9-124">Du måste använda hello URI standardvärdet `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-124">You must use hello default URI value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="2-install-nodejs"></a><span data-ttu-id="8f8d9-125">2: Installera Node.js</span><span class="sxs-lookup"><span data-stu-id="8f8d9-125">2: Install Node.js</span></span>
<span data-ttu-id="8f8d9-126">toouse hello exemplet för den här självstudiekursen måste du [installera Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="8f8d9-126">toouse hello sample for this tutorial, you must [install Node.js](http://nodejs.org).</span></span>

## <a name="3-install-mongodb"></a><span data-ttu-id="8f8d9-127">3: Installera MongoDB</span><span class="sxs-lookup"><span data-stu-id="8f8d9-127">3: Install MongoDB</span></span>
<span data-ttu-id="8f8d9-128">toosuccessfully använda det här exemplet måste du [installera MongoDB](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="8f8d9-128">toosuccessfully use this sample, you must [install MongoDB](http://www.mongodb.org).</span></span> <span data-ttu-id="8f8d9-129">I det här exemplet använder du MongoDB toomake REST-API beständiga över serverinstanser.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-129">In this sample, you use MongoDB toomake your REST API persistent across server instances.</span></span>

> [!NOTE]
> <span data-ttu-id="8f8d9-130">I den här artikeln förutsätter vi att du använder hello installation och server standardslutpunkterna för MongoDB: mongodb://localhost.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-130">In this article, we assume that you use hello default installation and server endpoints for MongoDB: mongodb://localhost.</span></span>
> 
> 

## <a name="4-install-hello-restify-modules-in-your-web-api"></a><span data-ttu-id="8f8d9-131">4: Installera hello restify-modulerna i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="8f8d9-131">4: Install hello restify modules in your web API</span></span>
<span data-ttu-id="8f8d9-132">Vi använder Resitfy toobuild vårt REST-API.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-132">We use Resitfy toobuild our REST API.</span></span> <span data-ttu-id="8f8d9-133">Restify är ett minimalt och flexibelt Node.js programramverk som härleds från Express.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-133">Restify is a minimal and flexible Node.js application framework that's derived from Express.</span></span> <span data-ttu-id="8f8d9-134">Restify har en kraftfull uppsättning funktioner som du kan använda toobuild REST API: er utöver Connect.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-134">Restify has a robust set of features that you can use toobuild REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="8f8d9-135">Installera restify</span><span class="sxs-lookup"><span data-stu-id="8f8d9-135">Install restify</span></span>
1.  <span data-ttu-id="8f8d9-136">I Kommandotolken, ändra hello katalogen för**azuread**:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-136">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

    <span data-ttu-id="8f8d9-137">Om hello **azuread** katalogen inte finns, skapar du den:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-137">If hello **azuread** directory does not exist, create it:</span></span>

    `mkdir azuread`

2.  <span data-ttu-id="8f8d9-138">Installera restify:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-138">Install restify:</span></span>

    `npm install restify`

    <span data-ttu-id="8f8d9-139">hello utdata från kommandot ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-139">hello output of this command should look like this:</span></span>

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

#### <a name="did-you-get-an-error"></a><span data-ttu-id="8f8d9-140">Fick du ett felmeddelande?</span><span class="sxs-lookup"><span data-stu-id="8f8d9-140">Did you get an error?</span></span>
<span data-ttu-id="8f8d9-141">I vissa operativsystem när du använder hello `npm` kommandot kan du se meddelandet: `Error: EPERM, chmod '/usr/local/bin/..'`.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-141">On some operating systems, when you use hello `npm` command, you might see this message: `Error: EPERM, chmod '/usr/local/bin/..'`.</span></span> <span data-ttu-id="8f8d9-142">hello fel följs av en begäran om du försöker köra hello kontot som administratör.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-142">hello error is followed by a request that you try running hello account as an administrator.</span></span> <span data-ttu-id="8f8d9-143">Om detta inträffar kommandot hello `sudo` toorun `npm` på en högre Privilegienivå.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-143">If this occurs, use hello command `sudo` toorun `npm` at a higher privilege level.</span></span>

#### <a name="did-you-get-an-error-related-toodtrace"></a><span data-ttu-id="8f8d9-144">Fick du ett fel som rör tooDTrace?</span><span class="sxs-lookup"><span data-stu-id="8f8d9-144">Did you get an error related tooDTrace?</span></span>
<span data-ttu-id="8f8d9-145">När du installerar restify kan det hända att det här meddelandet:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-145">When you install restify, you might see this message:</span></span>

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

<span data-ttu-id="8f8d9-146">Restify har en kraftfull mekanism tootrace REST-anrop med DTrace.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-146">Restify has a powerful mechanism tootrace REST calls by using DTrace.</span></span> <span data-ttu-id="8f8d9-147">DTrace är inte tillgänglig på många operativsystem.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-147">However, DTrace is not available on many operating systems.</span></span> <span data-ttu-id="8f8d9-148">Du kan ignorera det här felmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-148">You can safely ignore this error message.</span></span>


## <a name="5-install-passportjs-in-your-web-api"></a><span data-ttu-id="8f8d9-149">5: Installera Passport.js i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="8f8d9-149">5: Install Passport.js in your web API</span></span>
1.  <span data-ttu-id="8f8d9-150">Kommandotolken hello ändra hello katalogen för**azuread**.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-150">At hello command prompt, change hello directory too**azuread**.</span></span>

2.  <span data-ttu-id="8f8d9-151">Installera Passport.js:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-151">Install Passport.js:</span></span>

    `npm install passport`

    <span data-ttu-id="8f8d9-152">hello hello kommandots utdata ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-152">hello output of hello command should look like this:</span></span>

    ```
     passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3
    ```

## <a name="6-add-passport-azure-ad-tooyour-web-api"></a><span data-ttu-id="8f8d9-153">6: Lägg till passport-azure-ad tooyour webb-API</span><span class="sxs-lookup"><span data-stu-id="8f8d9-153">6: Add passport-azure-ad tooyour web API</span></span>
<span data-ttu-id="8f8d9-154">Lägg sedan till hello OAuth-strategin genom att använda passport-azuread.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-154">Next, add hello OAuth strategy, by using passport-azuread.</span></span> <span data-ttu-id="8f8d9-155">`passport-azuread`är en uppsättning strategier som ansluter Azure AD med Passport.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-155">`passport-azuread` is a suite of strategies that connect Azure AD with Passport.</span></span> <span data-ttu-id="8f8d9-156">Vi använder den här strategin för ägar-token i REST API-exemplet.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-156">We use this strategy for bearer tokens in this REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="8f8d9-157">Även om OAuth 2.0 erbjuder ett ramverk där alla kända token-typer kan utfärdas, används ofta för vissa typer av token.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-157">Although OAuth 2.0 provides a framework in which any known token type can be issued, certain token types are commonly used.</span></span> <span data-ttu-id="8f8d9-158">Ägar-token är vanliga tooprotect slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-158">Bearer tokens are commonly used tooprotect endpoints.</span></span> <span data-ttu-id="8f8d9-159">Ägar-token är hello mest utfärdat typ av token i OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-159">Bearer tokens are hello most widely issued type of token in OAuth 2.0.</span></span> <span data-ttu-id="8f8d9-160">Många olika implementeringar OAuth 2.0 förutsätter att ägar-token hello enda typ av token som utfärdas.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-160">Many OAuth 2.0 implementations assume that bearer tokens are hello only type of token issued.</span></span>
> 
> 

1.  <span data-ttu-id="8f8d9-161">I Kommandotolken, ändra hello katalogen för**azuread**.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-161">At a command prompt, change hello directory too**azuread**.</span></span>

    `cd azuread`

2.  <span data-ttu-id="8f8d9-162">Installera hello Passport.js `passport-azure-ad` modulen:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-162">Install hello Passport.js `passport-azure-ad` module:</span></span>

    `npm install passport-azure-ad`

    <span data-ttu-id="8f8d9-163">hello hello kommandots utdata ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-163">hello output of hello command should look like this:</span></span>

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

## <a name="7-add-mongodb-modules-tooyour-web-api"></a><span data-ttu-id="8f8d9-164">7: lägga till MongoDB-moduler tooyour webb-API</span><span class="sxs-lookup"><span data-stu-id="8f8d9-164">7: Add MongoDB modules tooyour web API</span></span>
<span data-ttu-id="8f8d9-165">I det här exemplet använder vi MongoDB som våra datalagret.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-165">In this sample, we use MongoDB as our data store.</span></span> 

1.  <span data-ttu-id="8f8d9-166">Installera Mongoose ett vanligt plugin-programmet toomanage modeller och scheman:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-166">Install Mongoose, a widely used plug-in, toomanage models and schemas:</span></span> 

    `npm install mongoose`

2.  <span data-ttu-id="8f8d9-167">Installera hello databasdrivrutinen för MongoDB, som också kallas MongoDB:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-167">Install hello database driver for MongoDB, which is also called MongoDB:</span></span>

    `npm install mongodb`

## <a name="8-install-additional-modules"></a><span data-ttu-id="8f8d9-168">8: installera ytterligare moduler</span><span class="sxs-lookup"><span data-stu-id="8f8d9-168">8: Install additional modules</span></span>
<span data-ttu-id="8f8d9-169">Installera hello resterande moduler som krävs.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-169">Install hello remaining required modules.</span></span>

1.  <span data-ttu-id="8f8d9-170">I Kommandotolken, ändra hello katalogen för**azuread**:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-170">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="8f8d9-171">Ange hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-171">Enter hello following commands.</span></span> <span data-ttu-id="8f8d9-172">hello kommandon installera hello följande moduler i katalogen node_modules:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-172">hello commands install hello following modules in your node_modules directory:</span></span>

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

## <a name="9-create-a-serverjs-file-for-your-dependencies"></a><span data-ttu-id="8f8d9-173">9: skapa en Server.js-fil för dina beroenden</span><span class="sxs-lookup"><span data-stu-id="8f8d9-173">9: Create a Server.js file for your dependencies</span></span>
<span data-ttu-id="8f8d9-174">En Server.js-fil innehåller hello merparten av hello funktioner för web API-servern.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-174">A Server.js file holds hello majority of hello functionality for your web API server.</span></span> <span data-ttu-id="8f8d9-175">Lägg till de flesta av din kod toothis-fil.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-175">Add most of your code toothis file.</span></span> <span data-ttu-id="8f8d9-176">Produktion ska refactor du hello funktionerna i mindre filer som för separata vägar och styrenheter.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-176">For production purposes, you can refactor hello functionality into smaller files, like for separate routes and controllers.</span></span> <span data-ttu-id="8f8d9-177">I den här artikeln använder vi Server.js för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-177">In this article, we use Server.js for this purpose.</span></span>

1.  <span data-ttu-id="8f8d9-178">I Kommandotolken, ändra hello katalogen för**azuread**:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-178">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="8f8d9-179">Med hjälp av ett redigeringsprogram, skapa en Server.js-fil.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-179">Using an editor of your choice, create a Server.js file.</span></span> <span data-ttu-id="8f8d9-180">Lägg till följande information toohello hello:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-180">Add hello following information toohello file:</span></span>

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

3.  <span data-ttu-id="8f8d9-181">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-181">Save hello file.</span></span> <span data-ttu-id="8f8d9-182">Du kommer tillbaka tooit inom kort.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-182">You will return tooit shortly.</span></span>

## <a name="10-create-a-config-file-toostore-your-azure-ad-settings"></a><span data-ttu-id="8f8d9-183">10: skapa en config-fil toostore Azure AD-inställningar</span><span class="sxs-lookup"><span data-stu-id="8f8d9-183">10: Create a config file toostore your Azure AD settings</span></span>
<span data-ttu-id="8f8d9-184">Den här kodfilen skickar hello konfigurationsparametrar från tooPassport.js din Azure AD-portalen.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-184">This code file passes hello configuration parameters from your Azure AD portal tooPassport.js.</span></span> <span data-ttu-id="8f8d9-185">Du har skapat konfigurationsvärdena när du lade till hello webbportalen API toohello hello början av hello artikel.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-185">You created these configuration values when you added hello web API toohello portal at hello beginning of hello article.</span></span> <span data-ttu-id="8f8d9-186">När du har kopierat hello koden förklarar vi vad tooput i hello värdena för dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-186">After you copy hello code, we'll explain what tooput in hello values of these parameters.</span></span>

1.  <span data-ttu-id="8f8d9-187">I Kommandotolken, ändra hello katalogen för**azuread**:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-187">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="8f8d9-188">Skapa en Config.js-fil i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-188">In an editor, create a Config.js file.</span></span> <span data-ttu-id="8f8d9-189">Lägg till hello följande information:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-189">Add hello following information:</span></span>

    ```Javascript
    // Don't commit this file tooyour public repos. This config is for first-run.
    exports.creds = {
    mongoose_auth_local: 'mongodb://localhost/tasklist', // Your Mongo auth URI goes here.
    issuer: 'https://sts.windows.net/**<your application id>**/',
    audience: '<your redirect URI>',
    identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For Microsoft, you should never need toochange this.
    };

    ```



### <a name="required-values"></a><span data-ttu-id="8f8d9-190">Värden som krävs</span><span class="sxs-lookup"><span data-stu-id="8f8d9-190">Required values</span></span>

*   <span data-ttu-id="8f8d9-191">**IdentityMetadata**: det är där `passport-azure-ad` letar efter konfigurationsdata för hello identitetsprovider (IDP) och hello nycklar toovalidate hello JSON Web token (JWTs).</span><span class="sxs-lookup"><span data-stu-id="8f8d9-191">**IdentityMetadata**: This is where `passport-azure-ad` looks for your configuration data for hello identity provider (IDP) and hello keys toovalidate hello JSON Web Tokens (JWTs).</span></span> <span data-ttu-id="8f8d9-192">Om du använder Azure AD, vill du förmodligen inte toochange detta.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-192">If you are using Azure AD, you probably don't want toochange this.</span></span>

*   <span data-ttu-id="8f8d9-193">**målgruppen**: din omdirigerings-URI från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-193">**audience**: Your redirect URI from hello portal.</span></span>

> [!NOTE]
> <span data-ttu-id="8f8d9-194">Distribuera dina nycklar med återkommande intervall.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-194">Roll your keys at frequent intervals.</span></span> <span data-ttu-id="8f8d9-195">Var noga med att du alltid hämtar från hello ”openid_keys” URL och hello appen kan komma åt hello Internet.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-195">Be sure that you always pull from hello "openid_keys" URL, and that hello app can access hello Internet.</span></span>
> 
> 

## <a name="11-add-hello-configuration-tooyour-serverjs-file"></a><span data-ttu-id="8f8d9-196">11: Lägg till hello configuration tooyour Server.js-fil</span><span class="sxs-lookup"><span data-stu-id="8f8d9-196">11: Add hello configuration tooyour Server.js file</span></span>
<span data-ttu-id="8f8d9-197">Programmet måste tooread hello värden från hello config-fil som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-197">Your application needs tooread hello values from hello config file you just created.</span></span> <span data-ttu-id="8f8d9-198">Lägg till hello .config-filen som en nödvändig resurs i ditt program.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-198">Add hello .config file as a required resource in your application.</span></span> <span data-ttu-id="8f8d9-199">Ange hello globala variabler toothose som finns i Config.js.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-199">Set hello global variables toothose that are in Config.js.</span></span>

1.  <span data-ttu-id="8f8d9-200">Kommandotolken hello ändra hello katalogen för**azuread**:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-200">At hello command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="8f8d9-201">Öppna Server.js i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-201">In an editor, open Server.js.</span></span> <span data-ttu-id="8f8d9-202">Lägg till hello följande information:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-202">Add hello following information:</span></span>

    ```Javascript
    var config = require('./config');
    ```

3.  <span data-ttu-id="8f8d9-203">Lägg till ett nytt avsnitt tooServer.js:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-203">Add a new section tooServer.js:</span></span>

    ```Javascript
    // Pass these options in toohello ODICBearerStrategy.
    var options = {
    // hello URL of hello metadata document for your app. Put hello keys for token validation from hello URL found in hello jwks_uri tag in hello metadata.
    identityMetadata: config.creds.identityMetadata,
    issuer: config.creds.issuer,
    audience: config.creds.audience
    };
    // Array toohold signed-in users and hello current signed-in user (owner).
    var users = [];
    var owner = null;
    // Your logger
    var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
    });
    ```

## <a name="12-add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="8f8d9-204">12: lägga till hello MongoDB-modellen och schemat genom att använda Mongoose</span><span class="sxs-lookup"><span data-stu-id="8f8d9-204">12: Add hello MongoDB model and schema information by using Mongoose</span></span>
<span data-ttu-id="8f8d9-205">Anslut sedan dessa tre filer i en REST API-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-205">Next, connect these three files in a REST API service.</span></span>

<span data-ttu-id="8f8d9-206">I den här artikeln använder vi MongoDB toostore våra uppgifter.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-206">In this article, we use MongoDB toostore our tasks.</span></span> <span data-ttu-id="8f8d9-207">Diskuterar vi detta i *steg 4*.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-207">We discuss this in *step 4*.</span></span>

<span data-ttu-id="8f8d9-208">I hello Config.js fil som du skapade i steg 11 databasen kallas *tasklist*.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-208">In hello Config.js file you created in step 11, your database is called *tasklist*.</span></span> <span data-ttu-id="8f8d9-209">Som var det du angav hello slutet av din mongoose_auth_local anslutnings-URL.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-209">That was what you put at hello end of your mongoose_auth_local connection URL.</span></span> <span data-ttu-id="8f8d9-210">Du behöver inte toocreate databasen i förväg i MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-210">You don't need toocreate this database beforehand in MongoDB.</span></span> <span data-ttu-id="8f8d9-211">hello-databas skapas på hello först kör serverprogrammet (förutsatt att hello databas inte redan finns).</span><span class="sxs-lookup"><span data-stu-id="8f8d9-211">hello database is created on hello first run of your server application (assuming hello database does not already exist).</span></span>

<span data-ttu-id="8f8d9-212">Du har redan nämnt hello server toouse vilken MongoDB-databas.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-212">You've told hello server what MongoDB database toouse.</span></span> <span data-ttu-id="8f8d9-213">Därefter behöver du toowrite vissa ytterligare kod toocreate hello modellen och schemat för din server-uppgifter.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-213">Next, you need toowrite some additional code toocreate hello model and schema for your server's tasks.</span></span>

### <a name="hello-model"></a><span data-ttu-id="8f8d9-214">hello-modellen</span><span class="sxs-lookup"><span data-stu-id="8f8d9-214">hello model</span></span>
<span data-ttu-id="8f8d9-215">Hej schemamodellen är väldigt enkla.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-215">hello schema model is very basic.</span></span> <span data-ttu-id="8f8d9-216">Du kan expandera den om du behöver.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-216">You can expand it if you need to.</span></span> 

<span data-ttu-id="8f8d9-217">hello har-schemat dessa värden:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-217">hello schema model has these values:</span></span>

*   <span data-ttu-id="8f8d9-218">**NAMNET**.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-218">**NAME**.</span></span> <span data-ttu-id="8f8d9-219">hello person tilldelade toohello uppgift.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-219">hello person assigned toohello task.</span></span> <span data-ttu-id="8f8d9-220">Det här är en **sträng** värde.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-220">This is a **string** value.</span></span>
*   <span data-ttu-id="8f8d9-221">**UPPGIFTEN**.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-221">**TASK**.</span></span> <span data-ttu-id="8f8d9-222">hello namnet på hello aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-222">hello name of hello task.</span></span> <span data-ttu-id="8f8d9-223">Det här är en **sträng** värde.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-223">This is a **string** value.</span></span>
*   <span data-ttu-id="8f8d9-224">**DATUM**.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-224">**DATE**.</span></span> <span data-ttu-id="8f8d9-225">hello datum hello uppgiften förfaller.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-225">hello date that hello task is due.</span></span> <span data-ttu-id="8f8d9-226">Det här är en **datetime** värde.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-226">This is a **datetime** value.</span></span>
*   <span data-ttu-id="8f8d9-227">**SLUTFÖRA**.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-227">**COMPLETED**.</span></span> <span data-ttu-id="8f8d9-228">Om hello-åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-228">Whether hello task is completed.</span></span> <span data-ttu-id="8f8d9-229">Det här är en **booleskt** värde.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-229">This is a **Boolean** value.</span></span>

### <a name="create-hello-schema-in-hello-code"></a><span data-ttu-id="8f8d9-230">Skapa hello schema i hello kod</span><span class="sxs-lookup"><span data-stu-id="8f8d9-230">Create hello schema in hello code</span></span>
1.  <span data-ttu-id="8f8d9-231">I Kommandotolken, ändra hello katalogen för**azuread**:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-231">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="8f8d9-232">Öppna Server.js i redigeringsprogram.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-232">In your editor, open Server.js.</span></span> <span data-ttu-id="8f8d9-233">Lägg till hello följande information under hello konfigurationspost:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-233">Below hello configuration entry, add hello following information:</span></span>

    ```Javascript
    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    // Connect tooMongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');
    ```

<span data-ttu-id="8f8d9-234">Den här koden ansluter toohello MongoDB-servern.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-234">This code connects toohello MongoDB server.</span></span> <span data-ttu-id="8f8d9-235">Den returnerar även ett schemaobjekt.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-235">It also returns a schema object.</span></span>

#### <a name="using-hello-schema-create-your-model-in-hello-code"></a><span data-ttu-id="8f8d9-236">Med hello schemat kan skapa din modell i hello kod</span><span class="sxs-lookup"><span data-stu-id="8f8d9-236">Using hello schema, create your model in hello code</span></span>
<span data-ttu-id="8f8d9-237">Lägg till följande kod hello under hello föregående kod:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-237">Below hello preceding code, add hello following code:</span></span>

```Javascript
// Create a basic schema toostore your tasks and users.
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

<span data-ttu-id="8f8d9-238">Som du ser från hello kod först skapar du schemat.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-238">As you can tell from hello code, first you create your schema.</span></span> <span data-ttu-id="8f8d9-239">Därefter skapar du ett modellobjekt.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-239">Next, you create a model object.</span></span> <span data-ttu-id="8f8d9-240">Du använder hello modellen objektet toostore data i hela hello code när du definierar din **vägar**.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-240">You use hello model object toostore your data throughout hello code when you define your **routes**.</span></span>

## <a name="13-add-your-routes-for-your-task-rest-api-server"></a><span data-ttu-id="8f8d9-241">13: lägga till din vägar för aktiviteten REST API-servern</span><span class="sxs-lookup"><span data-stu-id="8f8d9-241">13: Add your routes for your task REST API server</span></span>
<span data-ttu-id="8f8d9-242">Nu när du har en databas modellen toowork med lägger du till hello vägar du ska använda för REST API-servern.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-242">Now that you have a database model toowork with, add hello routes you'll use for your REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="8f8d9-243">Om vägar i restify</span><span class="sxs-lookup"><span data-stu-id="8f8d9-243">About routes in restify</span></span>
<span data-ttu-id="8f8d9-244">Vägar i restify fungerar exakt hello samma sätt som när du använder hello Express-stacken.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-244">Routes in restify work exactly hello same way they do when you use hello Express stack.</span></span> <span data-ttu-id="8f8d9-245">Du definierar vägar genom att använda hello URI som du förväntar dig hello klienten program toocall.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-245">You define routes by using hello URI that you expect hello client applications toocall.</span></span> <span data-ttu-id="8f8d9-246">Vanligtvis kan du definiera vägarna i en separat fil.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-246">Usually, you define your routes in a separate file.</span></span> <span data-ttu-id="8f8d9-247">I den här självstudiekursen lägger vi vårt vägar i Server.js.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-247">In this tutorial, we put our routes in Server.js.</span></span> <span data-ttu-id="8f8d9-248">För produktion rekommenderar vi att du strukturerar vägar till sina egna fil.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-248">For production use, we recommend that you factor routes into their own file.</span></span>

<span data-ttu-id="8f8d9-249">Ett typiskt mönster för en restify-väg är:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-249">A typical pattern for a restify route is:</span></span>

```Javascript
function createObject(req, res, next) {
// Do work on object.
_object.name = req.params.object; // Passed value is in req.params under object.
///...
return next(); // Keep hello server going.
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```


<span data-ttu-id="8f8d9-250">Detta är hello mönster på hello mest grundläggande nivån.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-250">This is hello pattern at hello most basic level.</span></span> <span data-ttu-id="8f8d9-251">Restify (och Express) ger mycket mer ingående funktioner, t.ex. hello möjlighet toodefine programtyper och komplex Routning över olika slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-251">Restify (and Express) provide much deeper functionality, like hello ability toodefine application types, and complex routing across different endpoints.</span></span>

#### <a name="add-default-routes-tooyour-server"></a><span data-ttu-id="8f8d9-252">Lägg till standard vägar tooyour server</span><span class="sxs-lookup"><span data-stu-id="8f8d9-252">Add default routes tooyour server</span></span>
<span data-ttu-id="8f8d9-253">Lägg till hello grundläggande CRUD-vägarna: **skapa**, **hämta**, **uppdatera**, och **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-253">Add hello basic CRUD routes: **create**, **retrieve**, **update**, and **delete**.</span></span>

1.  <span data-ttu-id="8f8d9-254">I Kommandotolken, ändra hello katalogen för**azuread**:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-254">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="8f8d9-255">Öppna Server.js i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-255">In an editor, open Server.js.</span></span> <span data-ttu-id="8f8d9-256">Nedan hello databasposter du gjorde tidigare, lägga till hello följande information:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-256">Below hello database entries you made earlier, add hello following information:</span></span>

    ```Javascript
    /**
    *
    * APIs for your REST task server
    */
    // Create a task.
    function createTask(req, res, next) {
    // Resitify currently has a bug that doesn't allow you tooset default headers.
    // These headers comply with CORS, and allow you toouse MongoDB Server as your response tooany origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    // Create a new task model, fill it, and save it tooMongoDB.
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
    /// Returns hello list of TODOs that were loaded.
    function listTasks(req, res, next) {
    // Resitify currently has a bug that doesn't allow you tooset default headers.
    // These headers comply with CORS, and allow us toouse MongoDB Server as our response tooany origin.
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
    log.warn(err, "There are no tasks in hello database. Add one!");
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

### <a name="add-error-handling-for-hello-routes"></a><span data-ttu-id="8f8d9-257">Lägg till felhantering för vägarna hello</span><span class="sxs-lookup"><span data-stu-id="8f8d9-257">Add error handling for hello routes</span></span>
<span data-ttu-id="8f8d9-258">Lägga till viss felhantering, så du kan kommunicera tillbaka toohello klienten om hello fel som inträffade.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-258">Add some error handling so you can communicate back toohello client about hello problem you encountered.</span></span>

<span data-ttu-id="8f8d9-259">Lägg till följande kod under hello-kod som du redan har skrivit hello:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-259">Add hello following code below hello code, which you've already written:</span></span>

```Javascript
///--- Errors for communicating something more information back toohello client.
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


## <a name="14-create-your-server"></a><span data-ttu-id="8f8d9-260">14: skapa servern</span><span class="sxs-lookup"><span data-stu-id="8f8d9-260">14: Create your server</span></span>
<span data-ttu-id="8f8d9-261">Hej senaste sak toodo är tooadd server-instansen.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-261">hello last thing toodo is tooadd your server instance.</span></span> <span data-ttu-id="8f8d9-262">Hej server-instans hanterar dina anrop.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-262">hello server instance manages your calls.</span></span>

<span data-ttu-id="8f8d9-263">Restify (och Express) har djupgående anpassning som du kan använda med en REST API-server.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-263">Restify (and Express) have deep customization that you can use with a REST API server.</span></span> <span data-ttu-id="8f8d9-264">I den här kursen använder vi hello mest grundläggande installationen.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-264">In this tutorial, we use hello most basic setup.</span></span>

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
// Allow 5 requests/second by IP address, and burst too10.
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
## <a name="15-add-hello-routes-without-authentication-for-now"></a><span data-ttu-id="8f8d9-265">15: lägga till vägar hello (utan autentisering för tillfället)</span><span class="sxs-lookup"><span data-stu-id="8f8d9-265">15: Add hello routes (without authentication, for now)</span></span>
```Javascript
/// Use CRUD tooadd hello real handlers.
/**
/*
/* Each of these handlers is protected by your Open ID Connect Bearer strategy. Invoke 'oidc-bearer'
/* in hello pasport.authenticate() method. Because REST is stateless, set 'session: false'. You 
/* don't need toomaintain session state. You can experiment with removing API protection.
/* toodo this, remove hello passport.authenticate() method:
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
consoleMessage += '\n Open your browser too%s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json tooget some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## <a name="16-run-hello-server"></a><span data-ttu-id="8f8d9-266">16: kör hello-server</span><span class="sxs-lookup"><span data-stu-id="8f8d9-266">16: Run hello server</span></span>
<span data-ttu-id="8f8d9-267">Det är en bra idé tootest servern innan du lägger till autentisering.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-267">It's a good idea tootest your server before you add authentication.</span></span>

<span data-ttu-id="8f8d9-268">hello enklaste sättet tootest din server är att använda curl vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-268">hello easiest way tootest your server is by using curl at a command prompt.</span></span> <span data-ttu-id="8f8d9-269">toodo detta, behöver du ett enkelt verktyg som du kan använda tooparse utdata som JSON.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-269">toodo this, you need a simple utility that you can use tooparse output as JSON.</span></span> 

1.  <span data-ttu-id="8f8d9-270">Installera hello JSON-verktyget som vi använder i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-270">Install hello JSON tool that we use in hello following examples:</span></span>

    `$npm install -g jsontool`

    <span data-ttu-id="8f8d9-271">Detta installerar hello JSON-verktyget globalt.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-271">This installs hello JSON tool globally.</span></span>

2.  <span data-ttu-id="8f8d9-272">Kontrollera att MongoDB-instansen körs:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-272">Make sure that your MongoDB instance is running:</span></span>

    `$sudo mongod`

3.  <span data-ttu-id="8f8d9-273">Ändra hello katalogen för**azuread**, och kör sedan curl:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-273">Change hello directory too**azuread**, and then run curl:</span></span>

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

4.  <span data-ttu-id="8f8d9-274">tooadd en uppgift:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-274">tooadd a task:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

    <span data-ttu-id="8f8d9-275">hello svaret bör vara:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-275">hello response should be:</span></span>

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

5.  <span data-ttu-id="8f8d9-276">Lista över aktiviteter för Jens:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-276">List tasks for Brandon:</span></span>

    `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

<span data-ttu-id="8f8d9-277">Om dessa kommandon körs utan problem, är du redo tooadd OAuth toohello REST API-servern.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-277">If all these commands run without errors, you are ready tooadd OAuth toohello REST API server.</span></span>

<span data-ttu-id="8f8d9-278">*Du har nu en REST API-server med MongoDB!*</span><span class="sxs-lookup"><span data-stu-id="8f8d9-278">*You now have a REST API server with MongoDB!*</span></span>

## <a name="17-add-authentication-tooyour-rest-api-server"></a><span data-ttu-id="8f8d9-279">17: Lägg till autentisering tooyour REST API-server</span><span class="sxs-lookup"><span data-stu-id="8f8d9-279">17: Add authentication tooyour REST API server</span></span>
<span data-ttu-id="8f8d9-280">Nu när du har en aktiv REST-API, ställa in den toouse den med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-280">Now that you have a running REST API, set it up toouse it with Azure AD.</span></span>

<span data-ttu-id="8f8d9-281">I Kommandotolken, ändra hello katalogen för**azuread**:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-281">At a command prompt, change hello directory too**azuread**:</span></span>

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-thats-included-with-passport-azure-ad"></a><span data-ttu-id="8f8d9-282">Använd hello oidc-ägarstrategi som ingår i passport-azure-ad</span><span class="sxs-lookup"><span data-stu-id="8f8d9-282">Use hello oidcbearerstrategy that's included with passport-azure-ad</span></span>
<span data-ttu-id="8f8d9-283">Hittills har du skapat en typisk REST TODO-server utan någon typ av auktorisering.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-283">So far, you've built a typical REST TODO server without any kind of authorization.</span></span> <span data-ttu-id="8f8d9-284">Lägg till autentisering.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-284">Now, add authentication.</span></span>

<span data-ttu-id="8f8d9-285">Ange först som du vill toouse Passport.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-285">First,  indicate that you want toouse Passport.</span></span> <span data-ttu-id="8f8d9-286">Placera den här rättigheten efter tidigare serverkonfiguration:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-286">Put this right after your earlier server configuration:</span></span>

```Javascript
// Start using Passport.js.

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [!TIP]
> <span data-ttu-id="8f8d9-287">När du skriver API: er är det en bra idé tooalways länk hello data toosomething unika från hello-token som hello användare inte kan förfalska.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-287">When you write APIs, it's a good idea tooalways link hello data toosomething unique from hello token that hello user can’t spoof.</span></span> <span data-ttu-id="8f8d9-288">När den här servern lagrar TODO-objekt, lagrar dem baserat på hello användaren prenumerations-ID i hello token (anropas via token.sub).</span><span class="sxs-lookup"><span data-stu-id="8f8d9-288">When this server stores TODO items, it stores them based on hello user subscription ID in hello token (called through token.sub).</span></span> <span data-ttu-id="8f8d9-289">Du kan placera hello token.sub hello ”ägare” fältet.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-289">You put hello token.sub in hello “owner” field.</span></span> <span data-ttu-id="8f8d9-290">Detta säkerställer att bara den användaren kan komma åt hello användarens TODOs.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-290">This ensures that only that user can access hello user's TODOs.</span></span> <span data-ttu-id="8f8d9-291">Ingen annan kan komma åt hello TODOs som har angetts.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-291">No one else can access hello TODOs that were entered.</span></span> <span data-ttu-id="8f8d9-292">Det finns inga exponering i hello API för ”ägare”.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-292">There is no exposure in hello API for “owner.”</span></span> <span data-ttu-id="8f8d9-293">En extern användare kan begära TODOs för andra användare, även om de autentiseras.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-293">An external user can request other users' TODOs, even if they are authenticated.</span></span>
> 
> 

<span data-ttu-id="8f8d9-294">Använd sedan hello öppna ID Connect ägarstrategi som medföljer `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-294">Next, use hello Open ID Connect Bearer strategy that comes with `passport-azure-ad`.</span></span> <span data-ttu-id="8f8d9-295">Placera det efter vad du klistrade in ovan:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-295">Put this after what you pasted earlier:</span></span>

```Javascript
/**
/*
/* Calling hello OIDCBearerStrategy and managing users.
/*
/* Because of hello Passport pattern, you need toomanage users and info tokens
/* with a FindorCreate() method. hello method must be provided by hello implementor.
/* In hello following code, you autoregister any user and implement a FindById().
/* It's a good idea toodo something more advanced.
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
log.info('verifying hello user');
log.info(token, 'was hello token retrieved');
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

<span data-ttu-id="8f8d9-296">Passport använder ett liknande mönster för alla sina strategier (Twitter, Facebook och så vidare).</span><span class="sxs-lookup"><span data-stu-id="8f8d9-296">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on).</span></span> <span data-ttu-id="8f8d9-297">Alla strategigenererare följer toohello mönster.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-297">All strategy writers adhere toohello pattern.</span></span> <span data-ttu-id="8f8d9-298">Skicka hello strategi en `function()` som använder en token och `done` som parametrar.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-298">Pass hello strategy a `function()` that uses a token and `done` as parameters.</span></span> <span data-ttu-id="8f8d9-299">hello strategi returneras när den gör allt arbete.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-299">hello strategy is returned after it does all its work.</span></span> <span data-ttu-id="8f8d9-300">Lagra hello användar- och stash hello token så inte behöver du tooask för den igen.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-300">Store hello user and stash hello token so you don’t need tooask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8f8d9-301">hello tar föregående kod alla användare som kan autentisera tooyour server.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-301">hello preceding code takes any user that can authenticate tooyour server.</span></span> <span data-ttu-id="8f8d9-302">Detta kallas för automatisk registrering.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-302">This is known as auto-registration.</span></span> <span data-ttu-id="8f8d9-303">På en produktionsserver förmodligen du vill toolet vem som helst utan att behöva dem gå igenom den registreringsprocess som du väljer.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-303">On a production server, you wouldn’t want toolet anyone in without first having them go through a registration process that you choose.</span></span> <span data-ttu-id="8f8d9-304">Detta är vanligtvis hello mönster du ser i konsumentappar.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-304">This is usually hello pattern you see in consumer apps.</span></span> <span data-ttu-id="8f8d9-305">hello app kan tillåta dig tooregister med Facebook, men sedan du tillfrågas tooenter ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-305">hello app might allow you tooregister with Facebook, but then it asks you tooenter additional information.</span></span> <span data-ttu-id="8f8d9-306">Om du inte använde ett kommandoradsprogram för den här självstudiekursen, kan du extrahera hello e-post från token hello-objekt som returneras.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-306">If you weren't using a command-line program for this tutorial, you could extract hello email from hello token object that is returned.</span></span> <span data-ttu-id="8f8d9-307">Sedan kan du be hello tooenter ytterligare användarinformation.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-307">Then, you might ask hello user tooenter additional information.</span></span> <span data-ttu-id="8f8d9-308">Eftersom detta är en testserver kan du lägga till hello användaren direkt toohello minnesinterna databasen.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-308">Because this is a test server, you add hello user directly toohello in-memory database.</span></span>
> 
> 

### <a name="protect-endpoints"></a><span data-ttu-id="8f8d9-309">Skydda slutpunkter</span><span class="sxs-lookup"><span data-stu-id="8f8d9-309">Protect endpoints</span></span>
<span data-ttu-id="8f8d9-310">Skydda slutpunkter genom att ange hello **passport.authenticate()** anrop med hello-protokollet som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-310">Protect endpoints by specifying hello **passport.authenticate()** call with hello protocol that you want toouse.</span></span>

<span data-ttu-id="8f8d9-311">Du kan redigera din vägen i serverkoden för mer avancerade alternativ:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-311">You can edit your route in your server code for a more advanced option:</span></span>

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

## <a name="18-run-your-server-application-again"></a><span data-ttu-id="8f8d9-312">18: kör serverprogrammet igen</span><span class="sxs-lookup"><span data-stu-id="8f8d9-312">18: Run your server application again</span></span>
<span data-ttu-id="8f8d9-313">Använd curl igen toosee om du använder OAuth 2.0 skydd mot dina slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-313">Use curl again toosee if you have OAuth 2.0 protection against your endpoints.</span></span> <span data-ttu-id="8f8d9-314">Gör detta innan du kör någon av dina klient-SDK: er mot den här slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-314">Do this before you run any of your client SDKs against this endpoint.</span></span> <span data-ttu-id="8f8d9-315">hello rubriker som returneras bör berättar om dina autentisering fungerar.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-315">hello headers returned should tell you if your authentication is working correctly.</span></span>

1.  <span data-ttu-id="8f8d9-316">Kontrollera att MongoDB-isntance körs:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-316">Make sure that your MongoDB isntance is running:</span></span>

    `$sudo mongod`

2.  <span data-ttu-id="8f8d9-317">Ändra toohello **azuread** katalogen och Använd curl:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-317">Change toohello **azuread** directory, and then use curl:</span></span>

    `$ cd azuread`

    `$ node server.js`

3.  <span data-ttu-id="8f8d9-318">Prova med ett grundläggande INLÄGG:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-318">Try a basic POST:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

<span data-ttu-id="8f8d9-319">Ett 401 svaret anger att hello Passport-lagret försöker tooredirect toohello auktorisera slutpunkt, vilket är exakt vad du vill.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-319">A 401 response indicates that hello Passport layer is trying tooredirect toohello authorize endpoint, which is exactly what you want.</span></span>

<span data-ttu-id="8f8d9-320">*Du har nu en REST API-tjänst som använder OAuth 2.0!*</span><span class="sxs-lookup"><span data-stu-id="8f8d9-320">*You now have a REST API service that uses OAuth 2.0!*</span></span>

<span data-ttu-id="8f8d9-321">Du har gjort så mycket du kan med den här servern utan att använda en OAuth 2.0-kompatibel klient.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-321">You've gone as far as you can with this server without using an OAuth 2.0-compatible client.</span></span> <span data-ttu-id="8f8d9-322">För att behöver du tooreview en ytterligare vägledning.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-322">For that, you will need tooreview an additional tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f8d9-323">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f8d9-323">Next steps</span></span>
<span data-ttu-id="8f8d9-324">För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) har angetts som [en .zip-fil](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="8f8d9-324">For reference, hello completed sample (without your configuration values) is provided as [a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip).</span></span> <span data-ttu-id="8f8d9-325">Du kan också klona det från GitHub:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-325">You also can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

<span data-ttu-id="8f8d9-326">Du kan nu gå vidare toomore avancerade alternativ.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-326">Now, you can move on toomore advanced topics.</span></span> <span data-ttu-id="8f8d9-327">Du kanske vill tootry [skydda en Node.js-webbapp med hello v2.0-slutpunkten](active-directory-v2-devquickstarts-node-web.md).</span><span class="sxs-lookup"><span data-stu-id="8f8d9-327">You might want tootry [Secure a Node.js web app using hello v2.0 endpoint](active-directory-v2-devquickstarts-node-web.md).</span></span>

<span data-ttu-id="8f8d9-328">Här följer några ytterligare resurser:</span><span class="sxs-lookup"><span data-stu-id="8f8d9-328">Here are some additional resources:</span></span>

* [<span data-ttu-id="8f8d9-329">Utvecklarhandbok för Azure AD v2.0</span><span class="sxs-lookup"><span data-stu-id="8f8d9-329">Azure AD v2.0 developer guide</span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="8f8d9-330">Stacken spill ”azure-active-directory” tagg</span><span class="sxs-lookup"><span data-stu-id="8f8d9-330">Stack Overflow "azure-active-directory" tag</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a><span data-ttu-id="8f8d9-331">Hämta säkerhetsuppdateringar för våra produkter</span><span class="sxs-lookup"><span data-stu-id="8f8d9-331">Get security updates for our products</span></span>
<span data-ttu-id="8f8d9-332">Vi rekommenderar att du toosign in toobe meddelas när säkerhetsincidenter.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-332">We encourage you toosign up toobe notified when security incidents occur.</span></span> <span data-ttu-id="8f8d9-333">På hello [Microsoft tekniska säkerhetsmeddelanden](https://technet.microsoft.com/security/dd252948) sidan, prenumerera tooSecurity rekommendationerna aviseringar.</span><span class="sxs-lookup"><span data-stu-id="8f8d9-333">On hello [Microsoft Technical Security Notifications](https://technet.microsoft.com/security/dd252948) page, subscribe tooSecurity Advisories Alerts.</span></span>

