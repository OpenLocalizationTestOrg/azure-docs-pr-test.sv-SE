---
title: "Azure AD B2C: Säkra ett webb-API med Node.js | Microsoft Docs"
description: "Hur du skapar ett webb-API för Node.js som accepterar token från en B2C-klient"
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
ms.openlocfilehash: 6480be75c314ede1b786e959a79c0385dd2edea8
ms.sourcegitcommit: 73f159cdbc122ffe42f3e1f7a3de05f77b6a4725
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/28/2017
---
# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a><span data-ttu-id="eca96-103">Azure AD B2C: Säkra ett webb-API med Node.js</span><span class="sxs-lookup"><span data-stu-id="eca96-103">Azure AD B2C: Secure a web API by using Node.js</span></span>
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

<span data-ttu-id="eca96-104">Med Azure Active Directory (Active AD) B2C kan du skydda ett webb-API med hjälp av OAuth 2.0-åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="eca96-104">With Azure Active Directory (Azure AD) B2C, you can secure a web API by using OAuth 2.0 access tokens.</span></span> <span data-ttu-id="eca96-105">Med hjälp av dessa token kan dina klientappar som använder Azure AD B2C autentisera mot API:et.</span><span class="sxs-lookup"><span data-stu-id="eca96-105">These tokens allow your client apps that use Azure AD B2C to authenticate to the API.</span></span> <span data-ttu-id="eca96-106">Den här artikeln visar hur du skapar en "uppgiftslista" för API som tillåter användare att lägga till och göra en lista med aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="eca96-106">This article shows you how to create a "to-do list" API that allows users to add and list tasks.</span></span> <span data-ttu-id="eca96-107">Webb-API:t skyddas med hjälp av Azure AD B2C och tillåter endast autentiserade användare att hantera sina att göra-listor.</span><span class="sxs-lookup"><span data-stu-id="eca96-107">The web API is secured using Azure AD B2C and only allows authenticated users to manage their to-do list.</span></span>

> [!NOTE]
> <span data-ttu-id="eca96-108">Det är tänkt att du ska ansluta till det här exemplet genom att använda vårt [iOS B2C-exempelprogram](active-directory-b2c-devquickstarts-ios.md).</span><span class="sxs-lookup"><span data-stu-id="eca96-108">This sample was written to be connected to by using our [iOS B2C sample application](active-directory-b2c-devquickstarts-ios.md).</span></span> <span data-ttu-id="eca96-109">Gör den här genomgången först och följ sedan det exemplet.</span><span class="sxs-lookup"><span data-stu-id="eca96-109">Do the current walk-through first, and then follow along with that sample.</span></span>
>
>

<span data-ttu-id="eca96-110">**Passport** är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="eca96-110">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="eca96-111">Modulbaserade Passport är flexibelt och kan diskret installeras i alla Express-baserade webbappar eller Restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="eca96-111">Flexible and modular, Passport can be unobtrusively installed in any Express-based or Restify web application.</span></span> <span data-ttu-id="eca96-112">En omfattande uppsättning strategier stöder autentisering med användarnamn och lösenord, Facebook, Twitter och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="eca96-112">A comprehensive set of strategies supports authentication by using a user name and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="eca96-113">Vi har utvecklat en strategi för Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="eca96-113">We have developed a strategy for Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="eca96-114">Du installerar den här modulen och lägger sedan till Azure AD-plugin-programmet `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="eca96-114">You install this module and then add the Azure AD `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="eca96-115">Om du vill göra det här exemplet måste du:</span><span class="sxs-lookup"><span data-stu-id="eca96-115">To do this sample, you need to:</span></span>

1. <span data-ttu-id="eca96-116">Registrera ett program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eca96-116">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="eca96-117">Ställa in så att programmet använder plugin-programmet för Passport `azure-ad-passport`.</span><span class="sxs-lookup"><span data-stu-id="eca96-117">Set up your application to use Passport's `azure-ad-passport` plug-in.</span></span>
3. <span data-ttu-id="eca96-118">Konfigurera ett klientprogram att anropa ett webb-API för ”uppgiftslistan”.</span><span class="sxs-lookup"><span data-stu-id="eca96-118">Configure a client application to call the "to-do list" web API.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="eca96-119">Skaffa en Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="eca96-119">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="eca96-120">Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.</span><span class="sxs-lookup"><span data-stu-id="eca96-120">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="eca96-121">En katalog är en behållare för alla användare, appar, grupper och mer.</span><span class="sxs-lookup"><span data-stu-id="eca96-121">A directory is a container for all users, apps, groups, and more.</span></span>  <span data-ttu-id="eca96-122">Om du inte redan har en [skapar du en B2C-katalog](active-directory-b2c-get-started.md) innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="eca96-122">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="eca96-123">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="eca96-123">Create an application</span></span>
<span data-ttu-id="eca96-124">Därefter måste du skapa en app i B2C-katalogen som ger Azure AD information som krävs för säker kommunikation med din app.</span><span class="sxs-lookup"><span data-stu-id="eca96-124">Next, you need to create an app in your B2C directory that gives Azure AD some information that it needs to securely communicate with your app.</span></span> <span data-ttu-id="eca96-125">I det här fallet representeras både klientappen och webb-API av ett enda **Program-ID** eftersom de omfattar en logisk app.</span><span class="sxs-lookup"><span data-stu-id="eca96-125">In this case, both the client app and web API are represented by a single **Application ID**, because they comprise one logical app.</span></span> <span data-ttu-id="eca96-126">Du skapar en app genom att följa [dessa anvisningar](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="eca96-126">To create an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="eca96-127">Se till att:</span><span class="sxs-lookup"><span data-stu-id="eca96-127">Be sure to:</span></span>

* <span data-ttu-id="eca96-128">Inkludera en **webbapp/ett webb-api** i programmet</span><span class="sxs-lookup"><span data-stu-id="eca96-128">Include a **web app/web api** in the application</span></span>
* <span data-ttu-id="eca96-129">Ange `http://localhost/TodoListService` som en **Reply URL**.</span><span class="sxs-lookup"><span data-stu-id="eca96-129">Enter `http://localhost/TodoListService` as a **Reply URL**.</span></span> <span data-ttu-id="eca96-130">Den är standard-URL:en för det här kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="eca96-130">It is the default URL for this code sample.</span></span>
* <span data-ttu-id="eca96-131">Skapa en **programhemlighet** för programmet och kopiera den.</span><span class="sxs-lookup"><span data-stu-id="eca96-131">Create an **Application secret** for your application and copy it.</span></span> <span data-ttu-id="eca96-132">Du behöver dessa data senare.</span><span class="sxs-lookup"><span data-stu-id="eca96-132">You need this data later.</span></span> <span data-ttu-id="eca96-133">Tänk på att det här värdet måste ha [ett XM-escape-tecken](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) innan du använder det.</span><span class="sxs-lookup"><span data-stu-id="eca96-133">Note that this value needs to be [XML escaped](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) before you use it.</span></span>
* <span data-ttu-id="eca96-134">Kopiera **program-ID:t** som har tilldelats din app.</span><span class="sxs-lookup"><span data-stu-id="eca96-134">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="eca96-135">Du behöver dessa data senare.</span><span class="sxs-lookup"><span data-stu-id="eca96-135">You need this data later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="eca96-136">Skapa principer</span><span class="sxs-lookup"><span data-stu-id="eca96-136">Create your policies</span></span>
<span data-ttu-id="eca96-137">I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="eca96-137">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="eca96-138">Det här programmet innehåller två identitetsupplevelser: registrera sig och logga in.</span><span class="sxs-lookup"><span data-stu-id="eca96-138">This app contains two identity experiences: sign up and sign in.</span></span> <span data-ttu-id="eca96-139">Du måste skapa en princip av varje typ. Mer information finns i [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="eca96-139">You need to create one policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span>  <span data-ttu-id="eca96-140">Tänk på följande när du skapar dina tre principer:</span><span class="sxs-lookup"><span data-stu-id="eca96-140">When you create your three policies, be sure to:</span></span>

* <span data-ttu-id="eca96-141">Välj **visningsnamnet** och andra registreringsattribut i registreringsprincipen.</span><span class="sxs-lookup"><span data-stu-id="eca96-141">Choose the **Display name** and other sign-up attributes in your sign-up policy.</span></span>
* <span data-ttu-id="eca96-142">Välj det **visningsnamn** och **objekt-ID** som programmet gör anspråk på i varje princip.</span><span class="sxs-lookup"><span data-stu-id="eca96-142">Choose the **Display name** and **Object ID** application claims in every policy.</span></span>  <span data-ttu-id="eca96-143">Du kan också välja andra anspråk.</span><span class="sxs-lookup"><span data-stu-id="eca96-143">You can choose other claims as well.</span></span>
* <span data-ttu-id="eca96-144">Kopiera ned **namnet** för varje princip när du har skapat den.</span><span class="sxs-lookup"><span data-stu-id="eca96-144">Copy down the **Name** of each policy after you create it.</span></span> <span data-ttu-id="eca96-145">Det bör ha prefixet `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="eca96-145">It should have the prefix `b2c_1_`.</span></span>  <span data-ttu-id="eca96-146">Du behöver dem senare.</span><span class="sxs-lookup"><span data-stu-id="eca96-146">You need those policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="eca96-147">När du har skapat dina tre principer, är du redo att bygga din app.</span><span class="sxs-lookup"><span data-stu-id="eca96-147">After you have created your three policies, you're ready to build your app.</span></span>

<span data-ttu-id="eca96-148">Börja med [självstudierna för att komma igång med .NET-webbappar](active-directory-b2c-devquickstarts-web-dotnet.md) om du vill veta mer om hur principer fungerar i Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="eca96-148">To learn about how policies work in Azure AD B2C, start with the [.NET web app getting started tutorial](active-directory-b2c-devquickstarts-web-dotnet.md).</span></span>

## <a name="download-the-code"></a><span data-ttu-id="eca96-149">Ladda ned koden</span><span class="sxs-lookup"><span data-stu-id="eca96-149">Download the code</span></span>
<span data-ttu-id="eca96-150">Koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="eca96-150">The code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS).</span></span> <span data-ttu-id="eca96-151">Om du vill bygga exemplet allteftersom kan du [ladda ned stommen av ett projekt som en ZIP-fil](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="eca96-151">To build the sample as you go, you can [download a skeleton project as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip).</span></span> <span data-ttu-id="eca96-152">Du kan också klona stommen:</span><span class="sxs-lookup"><span data-stu-id="eca96-152">You can also clone the skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

<span data-ttu-id="eca96-153">Den färdiga appen finns också [som en ZIP-fil](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) eller i `complete`-grenen för samma centrallager.</span><span class="sxs-lookup"><span data-stu-id="eca96-153">The completed app is also [available as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) or on the `complete` branch of the same repository.</span></span>

## <a name="download-nodejs-for-your-platform"></a><span data-ttu-id="eca96-154">Ladda ned Node.js för plattformen</span><span class="sxs-lookup"><span data-stu-id="eca96-154">Download Node.js for your platform</span></span>
<span data-ttu-id="eca96-155">Du behöver en fungerande installation av Node.js. för att kunna använda det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="eca96-155">To successfully use this sample, you need a working installation of Node.js.</span></span>

<span data-ttu-id="eca96-156">Installera Node.js från [nodejs.org](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="eca96-156">Install Node.js from [nodejs.org](http://nodejs.org).</span></span>

## <a name="install-mongodb-for-your-platform"></a><span data-ttu-id="eca96-157">Installera MongoDB för din plattform</span><span class="sxs-lookup"><span data-stu-id="eca96-157">Install MongoDB for your platform</span></span>
<span data-ttu-id="eca96-158">Du behöver en fungerande installation av MongoDB för att kunna använda det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="eca96-158">To successfully use this sample, you need a working installation of MongoDB.</span></span> <span data-ttu-id="eca96-159">Vi använder MongoDB för att göra dina REST-API:er beständiga över serverinstanser.</span><span class="sxs-lookup"><span data-stu-id="eca96-159">We use MongoDB to make your REST API persistent across server instances.</span></span>

<span data-ttu-id="eca96-160">Installera MongoDB från [mongodb.org](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="eca96-160">Install MongoDB from [mongodb.org](http://www.mongodb.org).</span></span>

> [!NOTE]
> <span data-ttu-id="eca96-161">Den här genomgången förutsätter att du använder standardslutpunkterna för MongoDB, vilket (när detta skrivs) är `mongodb://localhost`.</span><span class="sxs-lookup"><span data-stu-id="eca96-161">This walk-through assumes that you use the default installation and server endpoints for MongoDB, which at the time of this writing is `mongodb://localhost`.</span></span>
>
>

## <a name="install-the-restify-modules-in-your-web-api"></a><span data-ttu-id="eca96-162">Installera Restify-modulerna i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="eca96-162">Install the Restify modules in your web API</span></span>
<span data-ttu-id="eca96-163">Vi använder Restify för att skapa REST-API.</span><span class="sxs-lookup"><span data-stu-id="eca96-163">We use Restify to build your REST API.</span></span> <span data-ttu-id="eca96-164">Restify är ett minimalt och flexibelt Node.js-program härlett från Express.</span><span class="sxs-lookup"><span data-stu-id="eca96-164">Restify is a minimal and flexible Node.js application framework derived from Express.</span></span> <span data-ttu-id="eca96-165">Det har kraftfulla och stabila funktioner för att skapa REST API:er utöver Connect.</span><span class="sxs-lookup"><span data-stu-id="eca96-165">It has a robust set of features for building REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="eca96-166">Installera Restify</span><span class="sxs-lookup"><span data-stu-id="eca96-166">Install Restify</span></span>
<span data-ttu-id="eca96-167">Ändra katalogen till `azuread` från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="eca96-167">From the command line, change your directory to `azuread`.</span></span> <span data-ttu-id="eca96-168">Skapa `azuread`-katalogen om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="eca96-168">If the `azuread` directory doesn't exist, create it.</span></span>

<span data-ttu-id="eca96-169">`cd azuread` eller `mkdir azuread;`</span><span class="sxs-lookup"><span data-stu-id="eca96-169">`cd azuread` or `mkdir azuread;`</span></span>

<span data-ttu-id="eca96-170">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="eca96-170">Enter the following command:</span></span>

`npm install restify`

<span data-ttu-id="eca96-171">Restify installeras med det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="eca96-171">This command installs Restify.</span></span>

#### <a name="did-you-get-an-error"></a><span data-ttu-id="eca96-172">Fick du ett felmeddelande?</span><span class="sxs-lookup"><span data-stu-id="eca96-172">Did you get an error?</span></span>
<span data-ttu-id="eca96-173">När du använder `npm` kan du, i vissa operativsystem, få felmeddelandet `Error: EPERM, chmod '/usr/local/bin/..'` och en förfrågan om att köra kontot som administratör.</span><span class="sxs-lookup"><span data-stu-id="eca96-173">In some operating systems, when you use `npm`, you may receive the error `Error: EPERM, chmod '/usr/local/bin/..'` and a request that you run the account as an administrator.</span></span> <span data-ttu-id="eca96-174">Om detta problem inträffar använder du `sudo`-kommandot för att köra `npm` på en högre privilegienivå.</span><span class="sxs-lookup"><span data-stu-id="eca96-174">If this problem occurs, use the `sudo` command to run `npm` at a higher privilege level.</span></span>

#### <a name="did-you-get-a-dtrace-error"></a><span data-ttu-id="eca96-175">Fick du ett felmeddelande om DTrace?</span><span class="sxs-lookup"><span data-stu-id="eca96-175">Did you get a DTrace error?</span></span>
<span data-ttu-id="eca96-176">När du installerar Restify kan du stöta på något som ser ut som den här texten:</span><span class="sxs-lookup"><span data-stu-id="eca96-176">You may see something like this text when you install Restify:</span></span>

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

<span data-ttu-id="eca96-177">Restify erbjuder en kraftfull mekanism för att spåra REST-anrop med DTrace.</span><span class="sxs-lookup"><span data-stu-id="eca96-177">Restify provides a powerful mechanism for tracing REST calls by using DTrace.</span></span> <span data-ttu-id="eca96-178">Men många operativsystem har inte DTrace tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="eca96-178">However, many operating systems do not have DTrace available.</span></span> <span data-ttu-id="eca96-179">Du kan ignorera dessa fel utan att oroa dig.</span><span class="sxs-lookup"><span data-stu-id="eca96-179">You can safely ignore these errors.</span></span>

<span data-ttu-id="eca96-180">Resultatet av kommandot ska se ut ungefär som den här texten:</span><span class="sxs-lookup"><span data-stu-id="eca96-180">The output of the command should appear similar to this text:</span></span>

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

## <a name="install-passport-in-your-web-api"></a><span data-ttu-id="eca96-181">Installera Passport i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="eca96-181">Install Passport in your web API</span></span>
<span data-ttu-id="eca96-182">Ändra katalogen till `azuread` från kommandoraden, om den inte redan finns där.</span><span class="sxs-lookup"><span data-stu-id="eca96-182">From the command line, change your directory to `azuread`, if it's not already there.</span></span>

<span data-ttu-id="eca96-183">Installera Passport med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="eca96-183">Install Passport using the following command:</span></span>

`npm install passport`

<span data-ttu-id="eca96-184">Resultatet av kommandot ska se ut ungefär som den här texten:</span><span class="sxs-lookup"><span data-stu-id="eca96-184">The output of the command should be similar to this text:</span></span>

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a><span data-ttu-id="eca96-185">Lägg till passport-azuread i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="eca96-185">Add passport-azuread to your web API</span></span>
<span data-ttu-id="eca96-186">Lägg sedan till OAuth-strategin genom att använda `passport-azuread`, en uppsättning strategier som ansluter Azure AD med Passport.</span><span class="sxs-lookup"><span data-stu-id="eca96-186">Next, add the OAuth strategy by using `passport-azuread`, a suite of strategies that connect Azure AD with Passport.</span></span> <span data-ttu-id="eca96-187">Använd den här strategin för ägar-token i REST API-exemplet.</span><span class="sxs-lookup"><span data-stu-id="eca96-187">Use this strategy for bearer tokens in the REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="eca96-188">Även om OAuth2 erbjuder ett ramverk där alla kända token-typer kan utfärdas, är det bara vissa token-typer som används ofta.</span><span class="sxs-lookup"><span data-stu-id="eca96-188">Although OAuth2 provides a framework in which any known token type can be issued, only certain token types have gained widespread use.</span></span> <span data-ttu-id="eca96-189">De token som skyddar slutpunkter är ägar-token.</span><span class="sxs-lookup"><span data-stu-id="eca96-189">The tokens for protecting endpoints are bearer tokens.</span></span> <span data-ttu-id="eca96-190">Dessa typer av token är de mest utfärdade i OAuth2.</span><span class="sxs-lookup"><span data-stu-id="eca96-190">These types of tokens are the most widely issued in OAuth2.</span></span> <span data-ttu-id="eca96-191">Många olika implementeringar utgår ifrån att ägar-token är den enda typ av token som utfärdas.</span><span class="sxs-lookup"><span data-stu-id="eca96-191">Many implementations assume that bearer tokens are the only type of token issued.</span></span>
>
>

<span data-ttu-id="eca96-192">Ändra katalogen till `azuread` från kommandoraden, om den inte redan finns där.</span><span class="sxs-lookup"><span data-stu-id="eca96-192">From the command line, change your directory to `azuread`, if it's not already there.</span></span>

<span data-ttu-id="eca96-193">Installera Passport `passport-azure-ad`-modulen genom att ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="eca96-193">Install the Passport `passport-azure-ad` module using the following command:</span></span>

`npm install passport-azure-ad`

<span data-ttu-id="eca96-194">Resultatet av kommandot ska se ut ungefär som den här texten:</span><span class="sxs-lookup"><span data-stu-id="eca96-194">The output of the command should be similar to this text:</span></span>

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

## <a name="add-mongodb-modules-to-your-web-api"></a><span data-ttu-id="eca96-195">Lägga till MongoDB-moduler i ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="eca96-195">Add MongoDB modules to your web API</span></span>
<span data-ttu-id="eca96-196">Det här exemplet använder MongoDB som datalager.</span><span class="sxs-lookup"><span data-stu-id="eca96-196">This sample uses MongoDB as your data store.</span></span> <span data-ttu-id="eca96-197">Installera därför Mongoose, ett vanligt plugin-program för att hantera modeller och scheman.</span><span class="sxs-lookup"><span data-stu-id="eca96-197">For that install Mongoose, a widely used plug-in for managing models and schemas.</span></span>

* `npm install mongoose`

## <a name="install-additional-modules"></a><span data-ttu-id="eca96-198">Installera ytterligare moduler</span><span class="sxs-lookup"><span data-stu-id="eca96-198">Install additional modules</span></span>
<span data-ttu-id="eca96-199">Installera sedan resterande moduler som krävs.</span><span class="sxs-lookup"><span data-stu-id="eca96-199">Next, install the remaining required modules.</span></span>

<span data-ttu-id="eca96-200">Ändra katalogen till `azuread` från kommandoraden, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="eca96-200">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="eca96-201">Installera moduler i din `node_modules`-katalog:</span><span class="sxs-lookup"><span data-stu-id="eca96-201">Install the modules in your `node_modules` directory:</span></span>

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`

## <a name="create-a-serverjs-file-with-your-dependencies"></a><span data-ttu-id="eca96-202">Skapa en server.js-fil med dina beroenden</span><span class="sxs-lookup"><span data-stu-id="eca96-202">Create a server.js file with your dependencies</span></span>
<span data-ttu-id="eca96-203">`server.js`-filen innehåller de flesta funktioner för Webb-API-servern.</span><span class="sxs-lookup"><span data-stu-id="eca96-203">The `server.js` file provides the majority of the functionality for your Web API server.</span></span>

<span data-ttu-id="eca96-204">Ändra katalogen till `azuread` från kommandoraden, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="eca96-204">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="eca96-205">Skapa en `server.js`-fil i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="eca96-205">Create a `server.js` file in an editor.</span></span> <span data-ttu-id="eca96-206">Lägg till följande information:</span><span class="sxs-lookup"><span data-stu-id="eca96-206">Add the following information:</span></span>

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

<span data-ttu-id="eca96-207">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="eca96-207">Save the file.</span></span> <span data-ttu-id="eca96-208">Du kommer tillbaka till den senare.</span><span class="sxs-lookup"><span data-stu-id="eca96-208">You return to it later.</span></span>

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a><span data-ttu-id="eca96-209">Skapa en config.js-fil för att lagra dina Azure AD-inställningar</span><span class="sxs-lookup"><span data-stu-id="eca96-209">Create a config.js file to store your Azure AD settings</span></span>
<span data-ttu-id="eca96-210">Den här kodfilen skickar konfigurationsparametrarna från din Azure AD-portal till `Passport.js`-filen.</span><span class="sxs-lookup"><span data-stu-id="eca96-210">This code file passes the configuration parameters from your Azure AD Portal to the `Passport.js` file.</span></span> <span data-ttu-id="eca96-211">Du har skapat konfigurationsvärdena när du la till ditt webb-API i portalen under den första delen av genomgången.</span><span class="sxs-lookup"><span data-stu-id="eca96-211">You created these configuration values when you added the web API to the portal in the first part of the walk-through.</span></span> <span data-ttu-id="eca96-212">När du har kopierat koden förklarar vi vad du ska ange i värdena för dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="eca96-212">We explain what to put in the values of these parameters after you copy the code.</span></span>

<span data-ttu-id="eca96-213">Ändra katalogen till `azuread` från kommandoraden, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="eca96-213">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="eca96-214">Skapa en `config.js`-fil i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="eca96-214">Create a `config.js` file in an editor.</span></span> <span data-ttu-id="eca96-215">Lägg till följande information:</span><span class="sxs-lookup"><span data-stu-id="eca96-215">Add the following information:</span></span>

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>',
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a><span data-ttu-id="eca96-216">Värden som krävs</span><span class="sxs-lookup"><span data-stu-id="eca96-216">Required values</span></span>
<span data-ttu-id="eca96-217">`clientID`: Klient-ID för programmet Web API.</span><span class="sxs-lookup"><span data-stu-id="eca96-217">`clientID`: The client ID of your Web API application.</span></span>

<span data-ttu-id="eca96-218">`IdentityMetadata`: Det är där `passport-azure-ad` letar efter konfigurationsdata för din identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="eca96-218">`IdentityMetadata`: This is where `passport-azure-ad` looks for your configuration data for the identity provider.</span></span> <span data-ttu-id="eca96-219">Den letar också efter nycklarna för att validera JSON-webb-token.</span><span class="sxs-lookup"><span data-stu-id="eca96-219">It also looks for the keys to validate the JSON web tokens.</span></span>

<span data-ttu-id="eca96-220">`audience`: Den URI (Uniform Resource Identifier) från portalen som identifierar ditt anropande program.</span><span class="sxs-lookup"><span data-stu-id="eca96-220">`audience`: The uniform resource identifier (URI) from the portal that identifies your calling application.</span></span>

<span data-ttu-id="eca96-221">`tenantName`: Ditt klientnamn (till exempel **contoso.onmicrosoft.com**).</span><span class="sxs-lookup"><span data-stu-id="eca96-221">`tenantName`: Your tenant name (for example, **contoso.onmicrosoft.com**).</span></span>

<span data-ttu-id="eca96-222">`policyName`: Den princip som du vill använda för att validera token som kommer till servern.</span><span class="sxs-lookup"><span data-stu-id="eca96-222">`policyName`: The policy that you want to validate the tokens coming in to your server.</span></span> <span data-ttu-id="eca96-223">Det bör vara samma princip som du använder i klientprogrammet för inloggning.</span><span class="sxs-lookup"><span data-stu-id="eca96-223">This policy should be the same policy that you use on the client application for sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="eca96-224">Använd samma principer för både klient- och serverinstallationen.</span><span class="sxs-lookup"><span data-stu-id="eca96-224">For now, use the same policies across both client and server setup.</span></span> <span data-ttu-id="eca96-225">Om du har redan gjort en genomgång och skapat dessa principer behöver du inte göra det igen.</span><span class="sxs-lookup"><span data-stu-id="eca96-225">If you have already completed a walk-through and created these policies, you don't need to do so again.</span></span> <span data-ttu-id="eca96-226">Eftersom du har gjort genomgången ska du inte behöva ställa in nya principer för klientgenomgångarna på webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="eca96-226">Because you completed the walk-through, you shouldn't need to set up new policies for client walk-throughs on the site.</span></span>
>
>

## <a name="add-configuration-to-your-serverjs-file"></a><span data-ttu-id="eca96-227">Lägg till konfigurationen i din server.js-fil</span><span class="sxs-lookup"><span data-stu-id="eca96-227">Add configuration to your server.js file</span></span>
<span data-ttu-id="eca96-228">För att läsa värdena från `config.js`-filen du skapade, lägg till `.config`-filen som en nödvändig resurs i ditt program och sedan ange de globala variablerna till dem i `config.js`-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="eca96-228">To read the values from the `config.js` file you created, add the `.config` file as a required resource in your application, and then set the global variables to those in the `config.js` document.</span></span>

<span data-ttu-id="eca96-229">Ändra katalogen till `azuread` från kommandoraden, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="eca96-229">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="eca96-230">Öppna `server.js`-filen i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="eca96-230">Open the `server.js` file in an editor.</span></span> <span data-ttu-id="eca96-231">Lägg till följande information:</span><span class="sxs-lookup"><span data-stu-id="eca96-231">Add the following information:</span></span>

```Javascript
var config = require('./config');
```
<span data-ttu-id="eca96-232">Lägg till ett nytt avsnitt i `server.js` som innehåller följande kod:</span><span class="sxs-lookup"><span data-stu-id="eca96-232">Add a new section to `server.js` that includes the following code:</span></span>

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

<span data-ttu-id="eca96-233">Sedan lägger vi till platshållare för de användare som vi får från våra anropande program.</span><span class="sxs-lookup"><span data-stu-id="eca96-233">Next, let's add some placeholders for the users we receive from our calling applications.</span></span>

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

<span data-ttu-id="eca96-234">Vi fortsätter med att skapa våra transaktionsloggfiler också.</span><span class="sxs-lookup"><span data-stu-id="eca96-234">Let's go ahead and create our logger too.</span></span>

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="eca96-235">Lägg till informationen om MongoDB-modellen och -schemat genom att använda Mongoose</span><span class="sxs-lookup"><span data-stu-id="eca96-235">Add the MongoDB model and schema information by using Mongoose</span></span>
<span data-ttu-id="eca96-236">Det du har förberett tidigare kommer att vara till hjälp när du för ihop dessa tre filer i en REST API-tjänst.</span><span class="sxs-lookup"><span data-stu-id="eca96-236">The earlier preparation pays off as you bring these three files together in a REST API service.</span></span>

<span data-ttu-id="eca96-237">Lagra dina uppgifter genom att använda MongoDB för den här genomgången, som beskrivits tidigare.</span><span class="sxs-lookup"><span data-stu-id="eca96-237">For this walk-through, use MongoDB to store your tasks, as discussed earlier.</span></span>

<span data-ttu-id="eca96-238">I `config.js`-filen, kallade du databasen för **Aktivitetslista**.</span><span class="sxs-lookup"><span data-stu-id="eca96-238">In the `config.js` file, you called your database **tasklist**.</span></span> <span data-ttu-id="eca96-239">Detta namn var även det du angav i slutet av anslutnings-URL:en för `mongoose_auth_local`.</span><span class="sxs-lookup"><span data-stu-id="eca96-239">That name was also what you put at the end of the `mongoose_auth_local` connection URL.</span></span> <span data-ttu-id="eca96-240">Du behöver inte skapa den här databasen i förväg i MongoDB.</span><span class="sxs-lookup"><span data-stu-id="eca96-240">You don't need to create this database beforehand in MongoDB.</span></span> <span data-ttu-id="eca96-241">Den skapar databasen på den första körningen av serverprogrammet.</span><span class="sxs-lookup"><span data-stu-id="eca96-241">It creates the database for you on the first run of your server application.</span></span>

<span data-ttu-id="eca96-242">När du har talat om för servern vilken MongoDB-databas som ska användas, måste du skriva ytterligare kod för att skapa modellen och schemat för serveraktiviteterna.</span><span class="sxs-lookup"><span data-stu-id="eca96-242">After you tell the server which MongoDB database to use, you need to write some additional code to create the model and schema for your server tasks.</span></span>

### <a name="expand-the-model"></a><span data-ttu-id="eca96-243">Expandera modellen</span><span class="sxs-lookup"><span data-stu-id="eca96-243">Expand the model</span></span>
<span data-ttu-id="eca96-244">Den här schemamodellen är enkel.</span><span class="sxs-lookup"><span data-stu-id="eca96-244">This schema model is simple.</span></span> <span data-ttu-id="eca96-245">Du kan expandera den efter behov.</span><span class="sxs-lookup"><span data-stu-id="eca96-245">You can expand it as required.</span></span>

<span data-ttu-id="eca96-246">`owner`: Den som har tilldelats aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="eca96-246">`owner`: Who is assigned to the task.</span></span> <span data-ttu-id="eca96-247">Det här objektet är en **sträng**.</span><span class="sxs-lookup"><span data-stu-id="eca96-247">This object is a **string**.</span></span>  

<span data-ttu-id="eca96-248">`Text`: Själva aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="eca96-248">`Text`: The task itself.</span></span> <span data-ttu-id="eca96-249">Det här objektet är en **sträng**.</span><span class="sxs-lookup"><span data-stu-id="eca96-249">This object is a **string**.</span></span>

<span data-ttu-id="eca96-250">`date`: Det datum då aktiviteten ska vara klar.</span><span class="sxs-lookup"><span data-stu-id="eca96-250">`date`: The date that the task is due.</span></span> <span data-ttu-id="eca96-251">Det här objektet är en **datetime**.</span><span class="sxs-lookup"><span data-stu-id="eca96-251">This object is a **datetime**.</span></span>

<span data-ttu-id="eca96-252">`completed`: Om aktiviteten har slutförts.</span><span class="sxs-lookup"><span data-stu-id="eca96-252">`completed`: If the task is complete.</span></span> <span data-ttu-id="eca96-253">Det här objektet är en **boolesk**.</span><span class="sxs-lookup"><span data-stu-id="eca96-253">This object is a **Boolean**.</span></span>

### <a name="create-the-schema-in-the-code"></a><span data-ttu-id="eca96-254">Skapa schemat i koden</span><span class="sxs-lookup"><span data-stu-id="eca96-254">Create the schema in the code</span></span>
<span data-ttu-id="eca96-255">Ändra katalogen till `azuread` från kommandoraden, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="eca96-255">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="eca96-256">Öppna `server.js`-filen i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="eca96-256">Open the `server.js` file in an editor.</span></span> <span data-ttu-id="eca96-257">Lägg till följande information under konfigurationsposten:</span><span class="sxs-lookup"><span data-stu-id="eca96-257">Add the following information below the configuration entry:</span></span>

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
<span data-ttu-id="eca96-258">Du skapar först schemat och sedan ett modellobjekt som du använder för att lagra data i hela koden när du definierar dina **vägar**.</span><span class="sxs-lookup"><span data-stu-id="eca96-258">You first create the schema, and then you create a model object that you use to store your data throughout the code when you define your **routes**.</span></span>

## <a name="add-routes-for-your-rest-api-task-server"></a><span data-ttu-id="eca96-259">Lägga till vägar för din REST API-aktivitetsserver</span><span class="sxs-lookup"><span data-stu-id="eca96-259">Add routes for your REST API task server</span></span>
<span data-ttu-id="eca96-260">Lägg till de vägar du ska använda för REST API-servern nu när du har en databasmodell att arbeta med.</span><span class="sxs-lookup"><span data-stu-id="eca96-260">Now that you have a database model to work with, add the routes you use for your REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="eca96-261">Om vägar i Restify</span><span class="sxs-lookup"><span data-stu-id="eca96-261">About routes in Restify</span></span>
<span data-ttu-id="eca96-262">I Restify fungerar vägar på samma sätt som när de använder Express-stacken.</span><span class="sxs-lookup"><span data-stu-id="eca96-262">Routes work in Restify in the same way that they work when they use the Express stack.</span></span> <span data-ttu-id="eca96-263">Du definierar vägar genom att använda den URI som du förväntar dig att klientprogram anropar.</span><span class="sxs-lookup"><span data-stu-id="eca96-263">You define routes by using the URI that you expect the client applications to call.</span></span>

<span data-ttu-id="eca96-264">Ett typiskt mönster för en Restify-väg är:</span><span class="sxs-lookup"><span data-stu-id="eca96-264">A typical pattern for a Restify route is:</span></span>

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

<span data-ttu-id="eca96-265">Restify och Express kan ge mycket mer ingående funktioner, till exempel att definiera programtyper och göra komplex routning över olika slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="eca96-265">Restify and Express can provide much deeper functionality, such as defining application types and doing complex routing across different endpoints.</span></span> <span data-ttu-id="eca96-266">För dessa självstudier håller vi vägarna enkla.</span><span class="sxs-lookup"><span data-stu-id="eca96-266">For the purposes of this tutorial, we keep these routes simple.</span></span>

#### <a name="add-default-routes-to-your-server"></a><span data-ttu-id="eca96-267">Lägga till standardvägar i servern</span><span class="sxs-lookup"><span data-stu-id="eca96-267">Add default routes to your server</span></span>
<span data-ttu-id="eca96-268">Du lägger nu till grundläggande CRUD-vägar för **skapa** och **lista** för vår REST API.</span><span class="sxs-lookup"><span data-stu-id="eca96-268">You now add the basic CRUD routes of **create** and **list** for our REST API.</span></span> <span data-ttu-id="eca96-269">Andra vägar finns i `complete`-grenen av exemplet.</span><span class="sxs-lookup"><span data-stu-id="eca96-269">Other routes can be found in the `complete` branch of the sample.</span></span>

<span data-ttu-id="eca96-270">Ändra katalogen till `azuread` från kommandoraden, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="eca96-270">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="eca96-271">Öppna `server.js`-filen i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="eca96-271">Open the `server.js` file in an editor.</span></span> <span data-ttu-id="eca96-272">Lägg till följande information under de databasposter du gjorde ovan:</span><span class="sxs-lookup"><span data-stu-id="eca96-272">Below the database entries you made above add the following information:</span></span>

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
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
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

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
            log.warn(err, "There is no tasks in the database. Add one!");
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


#### <a name="add-error-handling-for-the-routes"></a><span data-ttu-id="eca96-273">Lägg till felhantering för vägarna</span><span class="sxs-lookup"><span data-stu-id="eca96-273">Add error handling for the routes</span></span>
<span data-ttu-id="eca96-274">Lägg till viss felhantering, så att du kan informera klienten om eventuella problem på ett sätt som den kan förstå.</span><span class="sxs-lookup"><span data-stu-id="eca96-274">Add some error handling so that you can communicate any problems you encounter back to the client in a way that it can understand.</span></span>

<span data-ttu-id="eca96-275">Lägg till följande kod:</span><span class="sxs-lookup"><span data-stu-id="eca96-275">Add the following code:</span></span>

```Javascript
///--- Errors for communicating something interesting back to the client
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


## <a name="create-your-server"></a><span data-ttu-id="eca96-276">Skapa servern</span><span class="sxs-lookup"><span data-stu-id="eca96-276">Create your server</span></span>
<span data-ttu-id="eca96-277">Du har nu definierat databasen och placerat ut vägarna.</span><span class="sxs-lookup"><span data-stu-id="eca96-277">You have now defined your database and put your routes in place.</span></span> <span data-ttu-id="eca96-278">Det som är kvar är att lägga till serverinstansen som hanterar dina anrop.</span><span class="sxs-lookup"><span data-stu-id="eca96-278">The last thing for you to do is to add the server instance that manages your calls.</span></span>

<span data-ttu-id="eca96-279">Med Restify och Express kan anpassa för en REST API-server på en mycket ingående nivå, men här använder vi den mest grundläggande installationen.</span><span class="sxs-lookup"><span data-stu-id="eca96-279">Restify and Express provide deep customization for a REST API server, but we use the most basic setup here.</span></span>

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

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a><span data-ttu-id="eca96-280">Lägg till vägar på servern (utan autentisering)</span><span class="sxs-lookup"><span data-stu-id="eca96-280">Add the routes to the server (without authentication)</span></span>
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
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

```

## <a name="add-authentication-to-your-rest-api-server"></a><span data-ttu-id="eca96-281">Lägg till autentisering i REST API-servern</span><span class="sxs-lookup"><span data-stu-id="eca96-281">Add authentication to your REST API server</span></span>
<span data-ttu-id="eca96-282">Nu när du har en aktiv REST API-server kan du använda den med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eca96-282">Now that you have a running REST API server, you can make it useful against Azure AD.</span></span>

<span data-ttu-id="eca96-283">Ändra katalogen till `azuread` från kommandoraden, om den inte redan finns där:</span><span class="sxs-lookup"><span data-stu-id="eca96-283">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a><span data-ttu-id="eca96-284">Använd den OIDC-ägarstrategi som ingår i passport-azure-ad</span><span class="sxs-lookup"><span data-stu-id="eca96-284">Use the OIDCBearerStrategy that is included with passport-azure-ad</span></span>
> [!TIP]
> <span data-ttu-id="eca96-285">När du skriver API: er bör du alltid länka dina data till något unikt från token, något som användaren inte kan förfalska.</span><span class="sxs-lookup"><span data-stu-id="eca96-285">When you write APIs, you should always link the data to something unique from the token that the user can’t spoof.</span></span> <span data-ttu-id="eca96-286">När servern lagrar ToDo-objekt, sker det baserat på **oid** för användaren i token (anropas via token.oid). Det anges i fältet ”ägare”.</span><span class="sxs-lookup"><span data-stu-id="eca96-286">When the server stores ToDo items, it does so based on the **oid** of the user in the token (called through token.oid), which goes in the “owner” field.</span></span> <span data-ttu-id="eca96-287">Det här värdet garanterar att bara denna användaren kan komma åt sina egna ToDo-objekt.</span><span class="sxs-lookup"><span data-stu-id="eca96-287">This value ensures that only that user can access their own ToDo items.</span></span> <span data-ttu-id="eca96-288">Ägarens API exponeras aldrig, så en extern användare kan begära andras ToDo-objekt, även om de autentiseras.</span><span class="sxs-lookup"><span data-stu-id="eca96-288">There is no exposure in the API of “owner,” so an external user can request others’ ToDo items even if they are authenticated.</span></span>
>
>

<span data-ttu-id="eca96-289">Använd sedan den ägarstrategi som kommer med `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="eca96-289">Next, use the bearer strategy that comes with `passport-azure-ad`.</span></span>

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
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
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

<span data-ttu-id="eca96-290">Passport använder samma mönster för alla strategier.</span><span class="sxs-lookup"><span data-stu-id="eca96-290">Passport uses the same pattern for all its strategies.</span></span> <span data-ttu-id="eca96-291">Du skickar en `function()` som har `token` och `done` som parametrar.</span><span class="sxs-lookup"><span data-stu-id="eca96-291">You pass it a `function()` that has `token` and `done` as parameters.</span></span> <span data-ttu-id="eca96-292">Strategin kommer tillbaka när den har utfört allt sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="eca96-292">The strategy comes back to you after it has done all of its work.</span></span> <span data-ttu-id="eca96-293">Du bör då lagra användaren och spara token så att du inte behöver fråga efter den igen.</span><span class="sxs-lookup"><span data-stu-id="eca96-293">You should then store the user and save the token so that you don’t need to ask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eca96-294">Koden ovan tar alla användare som autentiserar till servern.</span><span class="sxs-lookup"><span data-stu-id="eca96-294">The code above takes any user who happens to authenticate to your server.</span></span> <span data-ttu-id="eca96-295">Denna process kallas autoregistrering.</span><span class="sxs-lookup"><span data-stu-id="eca96-295">This process is known as autoregistration.</span></span> <span data-ttu-id="eca96-296">I produktionsservrar ska du inte ge alla användare åtkomst till API:en utan att de först går igenom en registreringsprocess.</span><span class="sxs-lookup"><span data-stu-id="eca96-296">In production servers, don't let in any users access the API without first having them go through a registration process.</span></span> <span data-ttu-id="eca96-297">Denna process är oftast det mönster du ser i konsumentappar. Det gör att du kan registrera dig med hjälp av Facebook men du blir sedan uppmanad att fylla i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="eca96-297">This process is usually the pattern you see in consumer apps that allow you to register by using Facebook but then ask you to fill out additional information.</span></span> <span data-ttu-id="eca96-298">Om detta program inte var ett kommandoradsprogram hade vi kunnat extrahera mejladressen från det token-objekt som returneras och sedan bett användarna att fylla i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="eca96-298">If this program wasn’t a command-line program, we could have extracted the email from the token object that is returned and then asked users to fill out additional information.</span></span> <span data-ttu-id="eca96-299">Eftersom det här är ett exempel, lägger vi till dem till en databas i minnet.</span><span class="sxs-lookup"><span data-stu-id="eca96-299">Because this is a sample, we add them to an in-memory database.</span></span>
>
>

## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a><span data-ttu-id="eca96-300">Kör serverprogrammet för att verifiera att det avvisar dig</span><span class="sxs-lookup"><span data-stu-id="eca96-300">Run your server application to verify that it rejects you</span></span>
<span data-ttu-id="eca96-301">Du kan använda `curl` om du vill kontrollera att du nu har OAuth2-skydd mot dina slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="eca96-301">You can use `curl` to see if you now have OAuth2 protection against your endpoints.</span></span> <span data-ttu-id="eca96-302">De rubriker som returneras borde räcka för att se om att du är på rätt väg.</span><span class="sxs-lookup"><span data-stu-id="eca96-302">The headers returned should be enough to tell you that you are on the right path.</span></span>

<span data-ttu-id="eca96-303">Kontrollera att MongoDB-instansen körs:</span><span class="sxs-lookup"><span data-stu-id="eca96-303">Make sure that your MongoDB instance is running:</span></span>

    $sudo mongodb

<span data-ttu-id="eca96-304">Gå till katalogen och kör servern:</span><span class="sxs-lookup"><span data-stu-id="eca96-304">Change to the directory and run the server:</span></span>

    $ cd azuread
    $ node server.js

<span data-ttu-id="eca96-305">I ett nytt terminalfönster kör `curl`</span><span class="sxs-lookup"><span data-stu-id="eca96-305">In a new terminal window, run `curl`</span></span>

<span data-ttu-id="eca96-306">Prova med ett grundläggande INLÄGG:</span><span class="sxs-lookup"><span data-stu-id="eca96-306">Try a basic POST:</span></span>

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

<span data-ttu-id="eca96-307">Ett 401-fel är det svar du vill ha.</span><span class="sxs-lookup"><span data-stu-id="eca96-307">A 401 error is the response you want.</span></span> <span data-ttu-id="eca96-308">Det visar att Passport-lagret försöker omdirigera till slutpunkten för auktorisering .</span><span class="sxs-lookup"><span data-stu-id="eca96-308">It indicates that the Passport layer is trying to redirect to the authorize endpoint.</span></span>

## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a><span data-ttu-id="eca96-309">Du har nu en REST API-tjänst som använder OAuth2</span><span class="sxs-lookup"><span data-stu-id="eca96-309">You now have a REST API service that uses OAuth2</span></span>
<span data-ttu-id="eca96-310">Du har implementerat en REST API med Restify och OAuth!</span><span class="sxs-lookup"><span data-stu-id="eca96-310">You have implemented a REST API by using Restify and OAuth!</span></span> <span data-ttu-id="eca96-311">Du har nu tillräcklig kod så att du kan fortsätta att utveckla din tjänst och bygga på det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="eca96-311">You now have sufficient code so that you can continue to develop your service and build on this example.</span></span> <span data-ttu-id="eca96-312">Du har gjort så mycket du kan med den här servern utan att använda en OAuth2-kompatibel klient.</span><span class="sxs-lookup"><span data-stu-id="eca96-312">You have gone as far as you can with this server without using an OAuth2-compatible client.</span></span> <span data-ttu-id="eca96-313">För nästkommande steg använder du ytterligare en genomgång, såsom genomgången [Anslut till en webb-API med iOS har B2C](active-directory-b2c-devquickstarts-ios.md).</span><span class="sxs-lookup"><span data-stu-id="eca96-313">For that next step use an additional walk-through like our [Connect to a web API by using iOS with B2C](active-directory-b2c-devquickstarts-ios.md) walkthrough.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eca96-314">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eca96-314">Next steps</span></span>
<span data-ttu-id="eca96-315">Du kan nu gå vidare  till mer avancerade ämnen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="eca96-315">You can now move to more advanced topics, such as:</span></span>

[<span data-ttu-id="eca96-316">Ansluta till ett webb-API genom att använda iOS med B2C</span><span class="sxs-lookup"><span data-stu-id="eca96-316">Connect to a web API by using iOS with B2C</span></span>](active-directory-b2c-devquickstarts-ios.md)
