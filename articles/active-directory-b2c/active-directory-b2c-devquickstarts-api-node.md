---
title: "Azure AD B2C: Säkra ett webb-API med Node.js | Microsoft Docs"
description: "Hur toobuild en Node.js webb-API accepterar som token från en B2C-klient"
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: fc2b9af8-fbda-44e0-962a-8b963449106a
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: xerners
ms.openlocfilehash: 47f5bae025a9ba2f486e36acef36aa37cfb43543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a><span data-ttu-id="f8cbe-103">Azure AD B2C: Säkra ett webb-API med Node.js</span><span class="sxs-lookup"><span data-stu-id="f8cbe-103">Azure AD B2C: Secure a web API by using Node.js</span></span>
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

<span data-ttu-id="f8cbe-104">Med Azure Active Directory (Active AD) B2C kan du skydda ett webb-API med hjälp av OAuth 2.0-åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-104">With Azure Active Directory (Azure AD) B2C, you can secure a web API by using OAuth 2.0 access tokens.</span></span> <span data-ttu-id="f8cbe-105">Dessa token kan dina klientappar som använder Azure AD B2C tooauthenticate toohello API.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-105">These tokens allow your client apps that use Azure AD B2C tooauthenticate toohello API.</span></span> <span data-ttu-id="f8cbe-106">Den här artikeln visar hur toocreate ett ”uppgiftslistan”-API som gör att användare tooadd och listan uppgifter.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-106">This article shows you how toocreate a "to-do list" API that allows users tooadd and list tasks.</span></span> <span data-ttu-id="f8cbe-107">hello web API skyddas med Azure AD B2C och tillåter endast autentiserade användare toomanage sina att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-107">hello web API is secured using Azure AD B2C and only allows authenticated users toomanage their to-do list.</span></span>

> [!NOTE]
> <span data-ttu-id="f8cbe-108">Det här exemplet har skrivits toobe anslutna tooby vår [iOS B2C-exempelprogram](active-directory-b2c-devquickstarts-ios.md).</span><span class="sxs-lookup"><span data-stu-id="f8cbe-108">This sample was written toobe connected tooby using our [iOS B2C sample application](active-directory-b2c-devquickstarts-ios.md).</span></span> <span data-ttu-id="f8cbe-109">Gör hello aktuella genomgången först och följ sedan det exemplet.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-109">Do hello current walk-through first, and then follow along with that sample.</span></span>
>
>

<span data-ttu-id="f8cbe-110">**Passport** är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-110">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="f8cbe-111">Modulbaserade Passport är flexibelt och kan diskret installeras i alla Express-baserade webbappar eller Restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-111">Flexible and modular, Passport can be unobtrusively installed in any Express-based or Restify web application.</span></span> <span data-ttu-id="f8cbe-112">En omfattande uppsättning strategier stöder autentisering med användarnamn och lösenord, Facebook, Twitter och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-112">A comprehensive set of strategies supports authentication by using a user name and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="f8cbe-113">Vi har utvecklat en strategi för Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f8cbe-113">We have developed a strategy for Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="f8cbe-114">Du installerar den här modulen och lägger sedan till hello Azure AD `passport-azure-ad` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-114">You install this module and then add hello Azure AD `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="f8cbe-115">toodo det här exemplet måste du:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-115">toodo this sample, you need to:</span></span>

1. <span data-ttu-id="f8cbe-116">Registrera ett program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-116">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="f8cbe-117">Konfigurera ditt program toouse Passport's `azure-ad-passport` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-117">Set up your application toouse Passport's `azure-ad-passport` plug-in.</span></span>
3. <span data-ttu-id="f8cbe-118">Konfigurera en klient programmet toocall hello ”uppgiftslistan” webb-API.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-118">Configure a client application toocall hello "to-do list" web API.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="f8cbe-119">Skaffa en Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="f8cbe-119">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="f8cbe-120">Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-120">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="f8cbe-121">En katalog är en behållare för alla användare, appar, grupper och mer.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-121">A directory is a container for all users, apps, groups, and more.</span></span>  <span data-ttu-id="f8cbe-122">Om du inte redan har en [skapar du en B2C-katalog](active-directory-b2c-get-started.md) innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-122">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="f8cbe-123">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="f8cbe-123">Create an application</span></span>
<span data-ttu-id="f8cbe-124">Sedan måste toocreate en app i B2C-katalogen som ger Azure AD information toosecurely måste kommunicera med din app.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-124">Next, you need toocreate an app in your B2C directory that gives Azure AD some information that it needs toosecurely communicate with your app.</span></span> <span data-ttu-id="f8cbe-125">I det här fallet både hello-klientappen och webb-API som representeras av en enda **program-ID**eftersom de bildar en logisk app.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-125">In this case, both hello client app and web API are represented by a single **Application ID**, because they comprise one logical app.</span></span> <span data-ttu-id="f8cbe-126">toocreate en app, Följ [instruktionerna](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="f8cbe-126">toocreate an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="f8cbe-127">Se till att:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-127">Be sure to:</span></span>

* <span data-ttu-id="f8cbe-128">Inkludera en **webbapp/webb api** i hello program</span><span class="sxs-lookup"><span data-stu-id="f8cbe-128">Include a **web app/web api** in hello application</span></span>
* <span data-ttu-id="f8cbe-129">Ange `http://localhost/TodoListService` som en **Reply URL**.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-129">Enter `http://localhost/TodoListService` as a **Reply URL**.</span></span> <span data-ttu-id="f8cbe-130">Det är hello standard-URL för det här kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-130">It is hello default URL for this code sample.</span></span>
* <span data-ttu-id="f8cbe-131">Skapa en **programhemlighet** för programmet och kopiera den.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-131">Create an **Application secret** for your application and copy it.</span></span> <span data-ttu-id="f8cbe-132">Du behöver dessa data senare.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-132">You need this data later.</span></span> <span data-ttu-id="f8cbe-133">Observera att det här värdet måste toobe [ESC-XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) innan du använder den.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-133">Note that this value needs toobe [XML escaped](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) before you use it.</span></span>
* <span data-ttu-id="f8cbe-134">Kopiera hello **program-ID** som är tilldelade tooyour app.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-134">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="f8cbe-135">Du behöver dessa data senare.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-135">You need this data later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="f8cbe-136">Skapa principer</span><span class="sxs-lookup"><span data-stu-id="f8cbe-136">Create your policies</span></span>
<span data-ttu-id="f8cbe-137">I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="f8cbe-137">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="f8cbe-138">Det här programmet innehåller två identitetsupplevelser: registrera sig och logga in.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-138">This app contains two identity experiences: sign up and sign in.</span></span> <span data-ttu-id="f8cbe-139">Du behöver toocreate en princip av varje typ, enligt beskrivningen i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="f8cbe-139">You need toocreate one policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span>  <span data-ttu-id="f8cbe-140">Tänk på följande när du skapar dina tre principer:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-140">When you create your three policies, be sure to:</span></span>

* <span data-ttu-id="f8cbe-141">Välj hello **visningsnamn** och andra registreringsattribut i registreringsprincipen.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-141">Choose hello **Display name** and other sign-up attributes in your sign-up policy.</span></span>
* <span data-ttu-id="f8cbe-142">Välj hello **visningsnamn** och **objekt-ID** programanspråken i varje princip.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-142">Choose hello **Display name** and **Object ID** application claims in every policy.</span></span>  <span data-ttu-id="f8cbe-143">Du kan också välja andra anspråk.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-143">You can choose other claims as well.</span></span>
* <span data-ttu-id="f8cbe-144">Kopiera hello **namn** på varje princip när du har skapat den.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-144">Copy down hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="f8cbe-145">Det bör ha hello prefixet `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-145">It should have hello prefix `b2c_1_`.</span></span>  <span data-ttu-id="f8cbe-146">Du behöver dem senare.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-146">You need those policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="f8cbe-147">När du har skapat dina tre principer kan du är klar toobuild din app.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-147">After you have created your three policies, you're ready toobuild your app.</span></span>

<span data-ttu-id="f8cbe-148">toolearn om hur principer fungerar i Azure AD B2C, börja med hello [.NET komma igång med webbappen kursen](active-directory-b2c-devquickstarts-web-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="f8cbe-148">toolearn about how policies work in Azure AD B2C, start with hello [.NET web app getting started tutorial](active-directory-b2c-devquickstarts-web-dotnet.md).</span></span>

## <a name="download-hello-code"></a><span data-ttu-id="f8cbe-149">Hämta hello koden</span><span class="sxs-lookup"><span data-stu-id="f8cbe-149">Download hello code</span></span>
<span data-ttu-id="f8cbe-150">Hej koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="f8cbe-150">hello code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS).</span></span> <span data-ttu-id="f8cbe-151">toobuild hello exempel som du går du [ladda ned stommen av ett projekt som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="f8cbe-151">toobuild hello sample as you go, you can [download a skeleton project as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip).</span></span> <span data-ttu-id="f8cbe-152">Du kan också klona stommen hello:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-152">You can also clone hello skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

<span data-ttu-id="f8cbe-153">hello slutförts appen finns också [som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) eller på hello `complete` grenen av hello samma databas.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-153">hello completed app is also [available as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) or on hello `complete` branch of hello same repository.</span></span>

## <a name="download-nodejs-for-your-platform"></a><span data-ttu-id="f8cbe-154">Ladda ned Node.js för plattformen</span><span class="sxs-lookup"><span data-stu-id="f8cbe-154">Download Node.js for your platform</span></span>
<span data-ttu-id="f8cbe-155">toosuccessfully använda det här exemplet måste du en fungerande installation av Node.js.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-155">toosuccessfully use this sample, you need a working installation of Node.js.</span></span>

<span data-ttu-id="f8cbe-156">Installera Node.js från [nodejs.org](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="f8cbe-156">Install Node.js from [nodejs.org](http://nodejs.org).</span></span>

## <a name="install-mongodb-for-your-platform"></a><span data-ttu-id="f8cbe-157">Installera MongoDB för din plattform</span><span class="sxs-lookup"><span data-stu-id="f8cbe-157">Install MongoDB for your platform</span></span>
<span data-ttu-id="f8cbe-158">toosuccessfully använda det här exemplet måste du en fungerande installation av MongoDB.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-158">toosuccessfully use this sample, you need a working installation of MongoDB.</span></span> <span data-ttu-id="f8cbe-159">Vi använder MongoDB toomake REST-API beständiga över serverinstanser.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-159">We use MongoDB toomake your REST API persistent across server instances.</span></span>

<span data-ttu-id="f8cbe-160">Installera MongoDB från [mongodb.org](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="f8cbe-160">Install MongoDB from [mongodb.org](http://www.mongodb.org).</span></span>

> [!NOTE]
> <span data-ttu-id="f8cbe-161">Den här genomgången förutsätter att du använder hello installation och server standardslutpunkterna för MongoDB, som när detta skrivs hello är `mongodb://localhost`.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-161">This walk-through assumes that you use hello default installation and server endpoints for MongoDB, which at hello time of this writing is `mongodb://localhost`.</span></span>
>
>

## <a name="install-hello-restify-modules-in-your-web-api"></a><span data-ttu-id="f8cbe-162">Installera hello Restify-modulerna i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="f8cbe-162">Install hello Restify modules in your web API</span></span>
<span data-ttu-id="f8cbe-163">Vi använder Restify toobuild REST-API.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-163">We use Restify toobuild your REST API.</span></span> <span data-ttu-id="f8cbe-164">Restify är ett minimalt och flexibelt Node.js-program härlett från Express.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-164">Restify is a minimal and flexible Node.js application framework derived from Express.</span></span> <span data-ttu-id="f8cbe-165">Det har kraftfulla och stabila funktioner för att skapa REST API:er utöver Connect.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-165">It has a robust set of features for building REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="f8cbe-166">Installera Restify</span><span class="sxs-lookup"><span data-stu-id="f8cbe-166">Install Restify</span></span>
<span data-ttu-id="f8cbe-167">Från kommandoraden hello ändra katalogen för`azuread`.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-167">From hello command line, change your directory too`azuread`.</span></span> <span data-ttu-id="f8cbe-168">Om hello `azuread` katalogen inte finns, skapar du den.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-168">If hello `azuread` directory doesn't exist, create it.</span></span>

<span data-ttu-id="f8cbe-169">`cd azuread` eller `mkdir azuread;`</span><span class="sxs-lookup"><span data-stu-id="f8cbe-169">`cd azuread` or `mkdir azuread;`</span></span>

<span data-ttu-id="f8cbe-170">Ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-170">Enter hello following command:</span></span>

`npm install restify`

<span data-ttu-id="f8cbe-171">Restify installeras med det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-171">This command installs Restify.</span></span>

#### <a name="did-you-get-an-error"></a><span data-ttu-id="f8cbe-172">Fick du ett felmeddelande?</span><span class="sxs-lookup"><span data-stu-id="f8cbe-172">Did you get an error?</span></span>
<span data-ttu-id="f8cbe-173">I vissa operativsystem, när du använder `npm`, felmeddelande hello `Error: EPERM, chmod '/usr/local/bin/..'` och en förfrågan om att du kör hello kontot som administratör.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-173">In some operating systems, when you use `npm`, you may receive hello error `Error: EPERM, chmod '/usr/local/bin/..'` and a request that you run hello account as an administrator.</span></span> <span data-ttu-id="f8cbe-174">Om det här problemet inträffar kan använda hello `sudo` kommandot toorun `npm` på en högre Privilegienivå.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-174">If this problem occurs, use hello `sudo` command toorun `npm` at a higher privilege level.</span></span>

#### <a name="did-you-get-a-dtrace-error"></a><span data-ttu-id="f8cbe-175">Fick du ett felmeddelande om DTrace?</span><span class="sxs-lookup"><span data-stu-id="f8cbe-175">Did you get a DTrace error?</span></span>
<span data-ttu-id="f8cbe-176">När du installerar Restify kan du stöta på något som ser ut som den här texten:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-176">You may see something like this text when you install Restify:</span></span>

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

<span data-ttu-id="f8cbe-177">Restify erbjuder en kraftfull mekanism för att spåra REST-anrop med DTrace.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-177">Restify provides a powerful mechanism for tracing REST calls by using DTrace.</span></span> <span data-ttu-id="f8cbe-178">Men många operativsystem har inte DTrace tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-178">However, many operating systems do not have DTrace available.</span></span> <span data-ttu-id="f8cbe-179">Du kan ignorera dessa fel utan att oroa dig.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-179">You can safely ignore these errors.</span></span>

<span data-ttu-id="f8cbe-180">hello hello kommandots utdata ska se ut liknande toothis text:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-180">hello output of hello command should appear similar toothis text:</span></span>

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

## <a name="install-passport-in-your-web-api"></a><span data-ttu-id="f8cbe-181">Installera Passport i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="f8cbe-181">Install Passport in your web API</span></span>
<span data-ttu-id="f8cbe-182">Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-182">From hello command line, change your directory too`azuread`, if it's not already there.</span></span>

<span data-ttu-id="f8cbe-183">Installera Passport med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-183">Install Passport using hello following command:</span></span>

`npm install passport`

<span data-ttu-id="f8cbe-184">hello hello kommandots utdata ska vara liknande toothis text:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-184">hello output of hello command should be similar toothis text:</span></span>

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-tooyour-web-api"></a><span data-ttu-id="f8cbe-185">Lägg till passport-azuread tooyour webb-API</span><span class="sxs-lookup"><span data-stu-id="f8cbe-185">Add passport-azuread tooyour web API</span></span>
<span data-ttu-id="f8cbe-186">Lägg till hello OAuth-strategin genom att använda `passport-azuread`, en uppsättning strategier som ansluter Azure AD med Passport.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-186">Next, add hello OAuth strategy by using `passport-azuread`, a suite of strategies that connect Azure AD with Passport.</span></span> <span data-ttu-id="f8cbe-187">Använd den här strategin för ägar-token i hello REST API-exemplet.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-187">Use this strategy for bearer tokens in hello REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="f8cbe-188">Även om OAuth2 erbjuder ett ramverk där alla kända token-typer kan utfärdas, är det bara vissa token-typer som används ofta.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-188">Although OAuth2 provides a framework in which any known token type can be issued, only certain token types have gained widespread use.</span></span> <span data-ttu-id="f8cbe-189">hello-token som skyddar slutpunkter är ägar-token.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-189">hello tokens for protecting endpoints are bearer tokens.</span></span> <span data-ttu-id="f8cbe-190">Dessa typer av token är hello utfärdats oftast i OAuth2.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-190">These types of tokens are hello most widely issued in OAuth2.</span></span> <span data-ttu-id="f8cbe-191">Många olika implementeringar förutsätter att ägar-token hello enda typ av token som utfärdas.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-191">Many implementations assume that bearer tokens are hello only type of token issued.</span></span>
>
>

<span data-ttu-id="f8cbe-192">Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-192">From hello command line, change your directory too`azuread`, if it's not already there.</span></span>

<span data-ttu-id="f8cbe-193">Installera hello Passport `passport-azure-ad` modul med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-193">Install hello Passport `passport-azure-ad` module using hello following command:</span></span>

`npm install passport-azure-ad`

<span data-ttu-id="f8cbe-194">hello hello kommandots utdata ska vara liknande toothis text:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-194">hello output of hello command should be similar toothis text:</span></span>

``
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
``

## <a name="add-mongodb-modules-tooyour-web-api"></a><span data-ttu-id="f8cbe-195">Lägg till MongoDB-moduler tooyour webb-API</span><span class="sxs-lookup"><span data-stu-id="f8cbe-195">Add MongoDB modules tooyour web API</span></span>
<span data-ttu-id="f8cbe-196">Det här exemplet använder MongoDB som datalager.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-196">This sample uses MongoDB as your data store.</span></span> <span data-ttu-id="f8cbe-197">Installera därför Mongoose, ett vanligt plugin-program för att hantera modeller och scheman.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-197">For that install Mongoose, a widely used plug-in for managing models and schemas.</span></span>

* `npm install mongoose`

## <a name="install-additional-modules"></a><span data-ttu-id="f8cbe-198">Installera ytterligare moduler</span><span class="sxs-lookup"><span data-stu-id="f8cbe-198">Install additional modules</span></span>
<span data-ttu-id="f8cbe-199">Installera hello resterande moduler som krävs.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-199">Next, install hello remaining required modules.</span></span>

<span data-ttu-id="f8cbe-200">Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-200">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="f8cbe-201">Installera hello moduler i din `node_modules` directory:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-201">Install hello modules in your `node_modules` directory:</span></span>

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`

## <a name="create-a-serverjs-file-with-your-dependencies"></a><span data-ttu-id="f8cbe-202">Skapa en server.js-fil med dina beroenden</span><span class="sxs-lookup"><span data-stu-id="f8cbe-202">Create a server.js file with your dependencies</span></span>
<span data-ttu-id="f8cbe-203">Hej `server.js` filen innehåller hello merparten av hello funktioner för webb-API-server.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-203">hello `server.js` file provides hello majority of hello functionality for your Web API server.</span></span>

<span data-ttu-id="f8cbe-204">Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-204">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="f8cbe-205">Skapa en `server.js`-fil i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-205">Create a `server.js` file in an editor.</span></span> <span data-ttu-id="f8cbe-206">Lägg till hello följande information:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-206">Add hello following information:</span></span>

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

<span data-ttu-id="f8cbe-207">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-207">Save hello file.</span></span> <span data-ttu-id="f8cbe-208">Tooit tillbaka senare.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-208">You return tooit later.</span></span>

## <a name="create-a-configjs-file-toostore-your-azure-ad-settings"></a><span data-ttu-id="f8cbe-209">Skapa en config.js-fil toostore Azure AD-inställningar</span><span class="sxs-lookup"><span data-stu-id="f8cbe-209">Create a config.js file toostore your Azure AD settings</span></span>
<span data-ttu-id="f8cbe-210">Den här kodfilen skickar hello konfigurationsparametrar från din Azure AD-portalen toohello `Passport.js` fil.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-210">This code file passes hello configuration parameters from your Azure AD Portal toohello `Passport.js` file.</span></span> <span data-ttu-id="f8cbe-211">Du har skapat konfigurationsvärdena när du lade till hello webbportalen API toohello hello första delen av hello hanteringspaketen.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-211">You created these configuration values when you added hello web API toohello portal in hello first part of hello walk-through.</span></span> <span data-ttu-id="f8cbe-212">Vi förklarar vilka tooput i hello värdena för dessa parametrar när du har kopierat hello kod.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-212">We explain what tooput in hello values of these parameters after you copy hello code.</span></span>

<span data-ttu-id="f8cbe-213">Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-213">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="f8cbe-214">Skapa en `config.js`-fil i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-214">Create a `config.js` file in an editor.</span></span> <span data-ttu-id="f8cbe-215">Lägg till hello följande information:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-215">Add hello following information:</span></span>

```Javascript
// Don't commit this file tooyour public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in hello portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // hello Client ID of hello application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add hello B2C tenant name in hello <tenant name> area
tenantName:'<tenant name>',
policyName:'b2c_1_<sign in policy name>' // This is hello policy you'll want toovalidate against in B2C. Usually this is your Sign-in policy (as users sign in toothis API)
passReqToCallback: false // This is a node.js construct that lets you pass hello req all hello way back tooany upstream caller. We turn this off as there is no upstream caller.
};

```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a><span data-ttu-id="f8cbe-216">Värden som krävs</span><span class="sxs-lookup"><span data-stu-id="f8cbe-216">Required values</span></span>
<span data-ttu-id="f8cbe-217">`clientID`: hello klient-ID för Web API-program.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-217">`clientID`: hello client ID of your Web API application.</span></span>

<span data-ttu-id="f8cbe-218">`IdentityMetadata`: Det är där `passport-azure-ad` letar efter konfigurationsdata för hello identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-218">`IdentityMetadata`: This is where `passport-azure-ad` looks for your configuration data for hello identity provider.</span></span> <span data-ttu-id="f8cbe-219">Det verkar också för hello nycklar toovalidate hello JSON web token.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-219">It also looks for hello keys toovalidate hello JSON web tokens.</span></span>

<span data-ttu-id="f8cbe-220">`audience`: hello URI (uniform resource identifier) från hello-portalen som identifierar din anropande programmet.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-220">`audience`: hello uniform resource identifier (URI) from hello portal that identifies your calling application.</span></span>

<span data-ttu-id="f8cbe-221">`tenantName`: Ditt klientnamn (till exempel **contoso.onmicrosoft.com**).</span><span class="sxs-lookup"><span data-stu-id="f8cbe-221">`tenantName`: Your tenant name (for example, **contoso.onmicrosoft.com**).</span></span>

<span data-ttu-id="f8cbe-222">`policyName`: hello principen som du vill toovalidate hello token som kommer tooyour server.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-222">`policyName`: hello policy that you want toovalidate hello tokens coming in tooyour server.</span></span> <span data-ttu-id="f8cbe-223">Den här principen ska vara hello samma princip som du använder på hello klientprogrammet för inloggning.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-223">This policy should be hello same policy that you use on hello client application for sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="f8cbe-224">För tillfället hello Använd samma principer för både klient och server-installationen.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-224">For now, use hello same policies across both client and server setup.</span></span> <span data-ttu-id="f8cbe-225">Om du redan har slutfört en genomgång och skapat dessa principer behöver du inte toodo så igen.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-225">If you have already completed a walk-through and created these policies, you don't need toodo so again.</span></span> <span data-ttu-id="f8cbe-226">Eftersom du har slutfört genomgången hello bör inte måste tooset in nya principer för klientgenomgångarna på hello platsen.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-226">Because you completed hello walk-through, you shouldn't need tooset up new policies for client walk-throughs on hello site.</span></span>
>
>

## <a name="add-configuration-tooyour-serverjs-file"></a><span data-ttu-id="f8cbe-227">Lägg till konfigurationen tooyour server.js-fil</span><span class="sxs-lookup"><span data-stu-id="f8cbe-227">Add configuration tooyour server.js file</span></span>
<span data-ttu-id="f8cbe-228">tooread hello värden från hello `config.js` fil du skapade, lägga till hello `.config` filen som en nödvändig resurs i ditt program och ange sedan hello globala variabler toothose i hello `config.js` dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-228">tooread hello values from hello `config.js` file you created, add hello `.config` file as a required resource in your application, and then set hello global variables toothose in hello `config.js` document.</span></span>

<span data-ttu-id="f8cbe-229">Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-229">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="f8cbe-230">Öppna hello `server.js` fil i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-230">Open hello `server.js` file in an editor.</span></span> <span data-ttu-id="f8cbe-231">Lägg till hello följande information:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-231">Add hello following information:</span></span>

```Javascript
var config = require('./config');
```
<span data-ttu-id="f8cbe-232">Lägg till ett nytt avsnitt för`server.js` som innehåller hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-232">Add a new section too`server.js` that includes hello following code:</span></span>

```Javascript
// We pass these options in toohello ODICBearerStrategy.

var options = {
    // hello URL of hello metadata document for your app. We put hello keys for token validation from hello URL found in hello jwks_uri tag of hello in hello metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

<span data-ttu-id="f8cbe-233">Nu ska vi lägga till platshållare för hello användare som vi får från våra anropa program.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-233">Next, let's add some placeholders for hello users we receive from our calling applications.</span></span>

```Javascript
// array toohold logged in users and hello current logged in user (owner)
var users = [];
var owner = null;
```

<span data-ttu-id="f8cbe-234">Vi fortsätter med att skapa våra transaktionsloggfiler också.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-234">Let's go ahead and create our logger too.</span></span>

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="f8cbe-235">Lägga till hello MongoDB-modellen och schemat genom att använda Mongoose</span><span class="sxs-lookup"><span data-stu-id="f8cbe-235">Add hello MongoDB model and schema information by using Mongoose</span></span>
<span data-ttu-id="f8cbe-236">hello betalar förberett tidigare när du för ihop dessa tre filer i en REST API-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-236">hello earlier preparation pays off as you bring these three files together in a REST API service.</span></span>

<span data-ttu-id="f8cbe-237">För den här genomgången Använd MongoDB toostore dina aktiviteter, enligt tidigare diskussion.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-237">For this walk-through, use MongoDB toostore your tasks, as discussed earlier.</span></span>

<span data-ttu-id="f8cbe-238">I hello `config.js` fil, kallade du databasen **tasklist**.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-238">In hello `config.js` file, you called your database **tasklist**.</span></span> <span data-ttu-id="f8cbe-239">Det här namnet har också vad du placerar hello slutet av hello `mongoose_auth_local` anslutnings-URL.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-239">That name was also what you put at hello end of hello `mongoose_auth_local` connection URL.</span></span> <span data-ttu-id="f8cbe-240">Du behöver inte toocreate databasen i förväg i MongoDB.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-240">You don't need toocreate this database beforehand in MongoDB.</span></span> <span data-ttu-id="f8cbe-241">Hello databasen skapas åt dig på hello första gången du kör serverprogrammet.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-241">It creates hello database for you on hello first run of your server application.</span></span>

<span data-ttu-id="f8cbe-242">När du har talat hello servern vilken MongoDB databasen toouse, behöver du toowrite vissa ytterligare kod toocreate hello modellen och schemat för serveraktiviteterna.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-242">After you tell hello server which MongoDB database toouse, you need toowrite some additional code toocreate hello model and schema for your server tasks.</span></span>

### <a name="expand-hello-model"></a><span data-ttu-id="f8cbe-243">Expandera hello modellen</span><span class="sxs-lookup"><span data-stu-id="f8cbe-243">Expand hello model</span></span>
<span data-ttu-id="f8cbe-244">Den här schemamodellen är enkel.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-244">This schema model is simple.</span></span> <span data-ttu-id="f8cbe-245">Du kan expandera den efter behov.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-245">You can expand it as required.</span></span>

<span data-ttu-id="f8cbe-246">`owner`: Den som är tilldelad toohello aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-246">`owner`: Who is assigned toohello task.</span></span> <span data-ttu-id="f8cbe-247">Det här objektet är en **sträng**.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-247">This object is a **string**.</span></span>  

<span data-ttu-id="f8cbe-248">`Text`: själva hello-aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-248">`Text`: hello task itself.</span></span> <span data-ttu-id="f8cbe-249">Det här objektet är en **sträng**.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-249">This object is a **string**.</span></span>

<span data-ttu-id="f8cbe-250">`date`: hello datum hello uppgiften förfaller.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-250">`date`: hello date that hello task is due.</span></span> <span data-ttu-id="f8cbe-251">Det här objektet är en **datetime**.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-251">This object is a **datetime**.</span></span>

<span data-ttu-id="f8cbe-252">`completed`: Om hello uppgiften har slutförts.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-252">`completed`: If hello task is complete.</span></span> <span data-ttu-id="f8cbe-253">Det här objektet är en **boolesk**.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-253">This object is a **Boolean**.</span></span>

### <a name="create-hello-schema-in-hello-code"></a><span data-ttu-id="f8cbe-254">Skapa hello schema i hello kod</span><span class="sxs-lookup"><span data-stu-id="f8cbe-254">Create hello schema in hello code</span></span>
<span data-ttu-id="f8cbe-255">Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-255">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="f8cbe-256">Öppna hello `server.js` fil i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-256">Open hello `server.js` file in an editor.</span></span> <span data-ttu-id="f8cbe-257">Lägg till följande information under hello konfigurationspost hello:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-257">Add hello following information below hello configuration entry:</span></span>

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect tooMongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema toostore our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use hello schema tooregister a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
<span data-ttu-id="f8cbe-258">Du först skapa hello schemat och skapas ett modellobjekt som du använder toostore data i hela hello code när du definierar din **vägar**.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-258">You first create hello schema, and then you create a model object that you use toostore your data throughout hello code when you define your **routes**.</span></span>

## <a name="add-routes-for-your-rest-api-task-server"></a><span data-ttu-id="f8cbe-259">Lägga till vägar för din REST API-aktivitetsserver</span><span class="sxs-lookup"><span data-stu-id="f8cbe-259">Add routes for your REST API task server</span></span>
<span data-ttu-id="f8cbe-260">Nu när du har en databas modellen toowork med lägger du till hello vägar som du använder för REST API-servern.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-260">Now that you have a database model toowork with, add hello routes you use for your REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="f8cbe-261">Om vägar i Restify</span><span class="sxs-lookup"><span data-stu-id="f8cbe-261">About routes in Restify</span></span>
<span data-ttu-id="f8cbe-262">I Restify fungerar vägar i hello samma sätt som när de använder hello Express-stacken.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-262">Routes work in Restify in hello same way that they work when they use hello Express stack.</span></span> <span data-ttu-id="f8cbe-263">Du definierar vägar genom att använda hello URI som du förväntar dig hello klienten program toocall.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-263">You define routes by using hello URI that you expect hello client applications toocall.</span></span>

<span data-ttu-id="f8cbe-264">Ett typiskt mönster för en Restify-väg är:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-264">A typical pattern for a Restify route is:</span></span>

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep hello server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

<span data-ttu-id="f8cbe-265">Restify och Express kan ge mycket mer ingående funktioner, till exempel att definiera programtyper och göra komplex routning över olika slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-265">Restify and Express can provide much deeper functionality, such as defining application types and doing complex routing across different endpoints.</span></span> <span data-ttu-id="f8cbe-266">För hello i den här kursen är behåller vi vägarna enkla.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-266">For hello purposes of this tutorial, we keep these routes simple.</span></span>

#### <a name="add-default-routes-tooyour-server"></a><span data-ttu-id="f8cbe-267">Lägg till standard vägar tooyour server</span><span class="sxs-lookup"><span data-stu-id="f8cbe-267">Add default routes tooyour server</span></span>
<span data-ttu-id="f8cbe-268">Du nu lägga till hello grundläggande CRUD-vägarna för **skapa** och **lista** för vårt REST-API.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-268">You now add hello basic CRUD routes of **create** and **list** for our REST API.</span></span> <span data-ttu-id="f8cbe-269">Andra vägar kan hittas i hello `complete` grenen av hello exempel.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-269">Other routes can be found in hello `complete` branch of hello sample.</span></span>

<span data-ttu-id="f8cbe-270">Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-270">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="f8cbe-271">Öppna hello `server.js` fil i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-271">Open hello `server.js` file in an editor.</span></span> <span data-ttu-id="f8cbe-272">Nedan hello databasposter gjorde du senare lägga till hello följande information:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-272">Below hello database entries you made above add hello following information:</span></span>

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you tooset default headers
    // This headers comply with CORS and allow us toomongodbServer our response tooany origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it tooMongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
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
```

```Javascript
/// Simple returns hello list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you tooset default headers
    // This headers comply with CORS and allow us toomongodbServer our response tooany origin

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
            log.warn(err, "There is no tasks in hello database. Add one!");
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


#### <a name="add-error-handling-for-hello-routes"></a><span data-ttu-id="f8cbe-273">Lägg till felhantering för vägarna hello</span><span class="sxs-lookup"><span data-stu-id="f8cbe-273">Add error handling for hello routes</span></span>
<span data-ttu-id="f8cbe-274">Lägga till vissa felhantering så att du kan kommunicera problem tillbaka toohello klienten så att den kan förstå.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-274">Add some error handling so that you can communicate any problems you encounter back toohello client in a way that it can understand.</span></span>

<span data-ttu-id="f8cbe-275">Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-275">Add hello following code:</span></span>

```Javascript
///--- Errors for communicating something interesting back toohello client
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


## <a name="create-your-server"></a><span data-ttu-id="f8cbe-276">Skapa servern</span><span class="sxs-lookup"><span data-stu-id="f8cbe-276">Create your server</span></span>
<span data-ttu-id="f8cbe-277">Du har nu definierat databasen och placerat ut vägarna.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-277">You have now defined your database and put your routes in place.</span></span> <span data-ttu-id="f8cbe-278">hello sista för du toodo är tooadd hello serverinstansen som hanterar dina anrop.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-278">hello last thing for you toodo is tooadd hello server instance that manages your calls.</span></span>

<span data-ttu-id="f8cbe-279">Restify och Express ger anpassa för en REST API-server, men vi använder här hello mest grundläggande installationen.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-279">Restify and Express provide deep customization for a REST API server, but we use hello most basic setup here.</span></span>

```Javascript

/**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst too10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use hello common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping tooREST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-hello-routes-toohello-server-without-authentication"></a><span data-ttu-id="f8cbe-280">Lägg till hello vägar toohello server (utan autentisering)</span><span class="sxs-lookup"><span data-stu-id="f8cbe-280">Add hello routes toohello server (without authentication)</span></span>
```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser too%s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json tooget some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

```

## <a name="add-authentication-tooyour-rest-api-server"></a><span data-ttu-id="f8cbe-281">Lägg till autentisering tooyour REST API-server</span><span class="sxs-lookup"><span data-stu-id="f8cbe-281">Add authentication tooyour REST API server</span></span>
<span data-ttu-id="f8cbe-282">Nu när du har en aktiv REST API-server kan du använda den med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-282">Now that you have a running REST API server, you can make it useful against Azure AD.</span></span>

<span data-ttu-id="f8cbe-283">Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-283">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a><span data-ttu-id="f8cbe-284">Använd hello oidc-Ägarstrategi som ingår i passport-azure-ad</span><span class="sxs-lookup"><span data-stu-id="f8cbe-284">Use hello OIDCBearerStrategy that is included with passport-azure-ad</span></span>
> [!TIP]
> <span data-ttu-id="f8cbe-285">När du skriver att API: er, bör du alltid länka hello data toosomething unika från hello-token som hello användare inte kan förfalska.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-285">When you write APIs, you should always link hello data toosomething unique from hello token that hello user can’t spoof.</span></span> <span data-ttu-id="f8cbe-286">När hello-servern lagrar ToDo-objekt, sker det baserat på hello **oid** för hello användare i hello token (anropas via token.oid), som ska ingå i hello ”ägare”.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-286">When hello server stores ToDo items, it does so based on hello **oid** of hello user in hello token (called through token.oid), which goes in hello “owner” field.</span></span> <span data-ttu-id="f8cbe-287">Det här värdet garanterar att bara denna användaren kan komma åt sina egna ToDo-objekt.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-287">This value ensures that only that user can access their own ToDo items.</span></span> <span data-ttu-id="f8cbe-288">Exponeras aldrig i hello API för ”ägare”, så en extern användare kan begära andras ToDo-objekt, även om de autentiseras.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-288">There is no exposure in hello API of “owner,” so an external user can request others’ ToDo items even if they are authenticated.</span></span>
>
>

<span data-ttu-id="f8cbe-289">Använd sedan hello ägarstrategi som medföljer `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-289">Next, use hello bearer strategy that comes with `passport-azure-ad`.</span></span>

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying hello user');
        log.info(token, 'was hello token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

<span data-ttu-id="f8cbe-290">Passport använder hello samma mönster för alla sina strategier.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-290">Passport uses hello same pattern for all its strategies.</span></span> <span data-ttu-id="f8cbe-291">Du skickar en `function()` som har `token` och `done` som parametrar.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-291">You pass it a `function()` that has `token` and `done` as parameters.</span></span> <span data-ttu-id="f8cbe-292">hello strategin kommer tillbaka tooyou när den har utfört allt sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-292">hello strategy comes back tooyou after it has done all of its work.</span></span> <span data-ttu-id="f8cbe-293">Du bör och sedan lagra hello användaren och spara hello token så att du inte behöver tooask för den igen.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-293">You should then store hello user and save hello token so that you don’t need tooask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f8cbe-294">hello koden ovan tar alla användare som tooauthenticate tooyour server.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-294">hello code above takes any user who happens tooauthenticate tooyour server.</span></span> <span data-ttu-id="f8cbe-295">Denna process kallas autoregistrering.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-295">This process is known as autoregistration.</span></span> <span data-ttu-id="f8cbe-296">I produktionsservrar Låt inte i alla användare åtkomst hello API utan att behöva dem gå igenom registreringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-296">In production servers, don't let in any users access hello API without first having them go through a registration process.</span></span> <span data-ttu-id="f8cbe-297">Den här processen är vanligtvis hello mönster du ser i konsumentappar som gör att du tooregister med hjälp av Facebook men sedan ber du toofill i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-297">This process is usually hello pattern you see in consumer apps that allow you tooregister by using Facebook but then ask you toofill out additional information.</span></span> <span data-ttu-id="f8cbe-298">Om programmet inte var ett kommandoradsprogram, kunnat vi extrahera hello e-post från token hello-objekt som returneras och sedan bett användarna toofill i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-298">If this program wasn’t a command-line program, we could have extracted hello email from hello token object that is returned and then asked users toofill out additional information.</span></span> <span data-ttu-id="f8cbe-299">Eftersom detta är ett exempel kan vi lägga till dem tooan minnesinterna databasen.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-299">Because this is a sample, we add them tooan in-memory database.</span></span>
>
>

## <a name="run-your-server-application-tooverify-that-it-rejects-you"></a><span data-ttu-id="f8cbe-300">Kör tooverify din server-program avvisar att det dig</span><span class="sxs-lookup"><span data-stu-id="f8cbe-300">Run your server application tooverify that it rejects you</span></span>
<span data-ttu-id="f8cbe-301">Du kan använda `curl` toosee om du nu har OAuth2-skydd mot dina slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-301">You can use `curl` toosee if you now have OAuth2 protection against your endpoints.</span></span> <span data-ttu-id="f8cbe-302">hello rubriker som returneras bör vara tillräckligt med tootell att du inte på hello rätt sökväg.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-302">hello headers returned should be enough tootell you that you are on hello right path.</span></span>

<span data-ttu-id="f8cbe-303">Kontrollera att MongoDB-instansen körs:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-303">Make sure that your MongoDB instance is running:</span></span>

    $sudo mongodb

<span data-ttu-id="f8cbe-304">Ändra toohello directory och kör hello server:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-304">Change toohello directory and run hello server:</span></span>

    $ cd azuread
    $ node server.js

<span data-ttu-id="f8cbe-305">I ett nytt terminalfönster kör `curl`</span><span class="sxs-lookup"><span data-stu-id="f8cbe-305">In a new terminal window, run `curl`</span></span>

<span data-ttu-id="f8cbe-306">Prova med ett grundläggande INLÄGG:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-306">Try a basic POST:</span></span>

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

<span data-ttu-id="f8cbe-307">Ett 401-fel är hello-svar som du vill ha.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-307">A 401 error is hello response you want.</span></span> <span data-ttu-id="f8cbe-308">Anger det att hello Passport-lagret försöker tooredirect toohello auktorisera slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-308">It indicates that hello Passport layer is trying tooredirect toohello authorize endpoint.</span></span>

## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a><span data-ttu-id="f8cbe-309">Du har nu en REST API-tjänst som använder OAuth2</span><span class="sxs-lookup"><span data-stu-id="f8cbe-309">You now have a REST API service that uses OAuth2</span></span>
<span data-ttu-id="f8cbe-310">Du har implementerat en REST API med Restify och OAuth!</span><span class="sxs-lookup"><span data-stu-id="f8cbe-310">You have implemented a REST API by using Restify and OAuth!</span></span> <span data-ttu-id="f8cbe-311">Nu har du tillräckligt med kod så att du kan fortsätta toodevelop din tjänst och bygga på det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-311">You now have sufficient code so that you can continue toodevelop your service and build on this example.</span></span> <span data-ttu-id="f8cbe-312">Du har gjort så mycket du kan med den här servern utan att använda en OAuth2-kompatibel klient.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-312">You have gone as far as you can with this server without using an OAuth2-compatible client.</span></span> <span data-ttu-id="f8cbe-313">För att nästa steg att använda ytterligare en genomgång som våra [ansluta tooa webb-API med hjälp av iOS med B2C](active-directory-b2c-devquickstarts-ios.md) genomgången.</span><span class="sxs-lookup"><span data-stu-id="f8cbe-313">For that next step use an additional walk-through like our [Connect tooa web API by using iOS with B2C](active-directory-b2c-devquickstarts-ios.md) walkthrough.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8cbe-314">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f8cbe-314">Next steps</span></span>
<span data-ttu-id="f8cbe-315">Du kan nu flytta toomore avancerade ämnen, exempelvis:</span><span class="sxs-lookup"><span data-stu-id="f8cbe-315">You can now move toomore advanced topics, such as:</span></span>

[<span data-ttu-id="f8cbe-316">Ansluta tooa webb-API med hjälp av iOS med B2C</span><span class="sxs-lookup"><span data-stu-id="f8cbe-316">Connect tooa web API by using iOS with B2C</span></span>](active-directory-b2c-devquickstarts-ios.md)
