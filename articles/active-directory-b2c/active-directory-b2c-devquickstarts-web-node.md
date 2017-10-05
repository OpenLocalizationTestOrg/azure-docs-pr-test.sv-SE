---
title: "Lägga till inloggning till en Node.js-webbapp för Azure B2C | Microsoft Docs"
description: "Hur du skapar en Node.js-webbapp som loggar in användare med hjälp av en B2C-klient."
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: db97f84a-1f24-447b-b6d2-0265c6896b27
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: hero-article
ms.date: 03/10/2017
ms.author: xerners
ms.openlocfilehash: c85b8f8434d1e837ac96ac63b9b37f990677ed6e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-b2c-add-sign-in-to-a-nodejs-web-app"></a><span data-ttu-id="6c2fe-103">Azure AD B2C: Lägga till inloggning till en Node.js-webbapp</span><span class="sxs-lookup"><span data-stu-id="6c2fe-103">Azure AD B2C: Add sign-in to a Node.js web app</span></span>

<span data-ttu-id="6c2fe-104">**Passport** är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-104">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="6c2fe-105">Modulbaserade Passport är mycket flexibelt och kan diskret installeras i alla Express-baserade webbappar eller Restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-105">Extremely flexible and modular, Passport can be unobtrusively installed in any Express-based or Restify web application.</span></span> <span data-ttu-id="6c2fe-106">En omfattande uppsättning strategier stöder autentisering med användarnamn och lösenord, Facebook, Twitter och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-106">A comprehensive set of strategies supports authentication by using a user name and password, Facebook, Twitter, and more.</span></span>

<span data-ttu-id="6c2fe-107">Vi har utvecklat en strategi för Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6c2fe-107">We have developed a strategy for Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="6c2fe-108">Du installerar den här modulen och lägger sedan till Azure AD-plugin-programmet `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-108">You will install this module and then add the Azure AD `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="6c2fe-109">Om du vill göra det måste du:</span><span class="sxs-lookup"><span data-stu-id="6c2fe-109">To do this, you need to:</span></span>

1. <span data-ttu-id="6c2fe-110">Registrera ett program med hjälp av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-110">Register an application by using Azure AD.</span></span>
2. <span data-ttu-id="6c2fe-111">Konfigurera din app att använda plugin-programmet `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-111">Set up your app to use the `passport-azure-ad` plug-in.</span></span>
3. <span data-ttu-id="6c2fe-112">Använda Passport för att utfärda inloggnings- och utloggningsförfrågningar till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-112">Use Passport to issue sign-in and sign-out requests to Azure AD.</span></span>
4. <span data-ttu-id="6c2fe-113">Skriva ut användardata.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-113">Print user data.</span></span>

<span data-ttu-id="6c2fe-114">Koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="6c2fe-114">The code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS).</span></span> <span data-ttu-id="6c2fe-115">Om du vill kan du [ladda ned appens stomme som en ZIP-fil](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="6c2fe-115">To follow along, you can [download the app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip).</span></span> <span data-ttu-id="6c2fe-116">Du kan också klona stommen:</span><span class="sxs-lookup"><span data-stu-id="6c2fe-116">You can also clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="6c2fe-117">Du hittar det färdiga programmet i slutet av dessa självstudier.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-117">The completed application is provided at the end of this tutorial.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="6c2fe-118">Skaffa en Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="6c2fe-118">Get an Azure AD B2C directory</span></span>

<span data-ttu-id="6c2fe-119">Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-119">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="6c2fe-120">En katalog är en behållare för alla användare, appar, grupper och mer.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-120">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="6c2fe-121">Om du inte redan har en B2C-katalog [skapar du en](active-directory-b2c-get-started.md) innan du fortsätter den här guiden.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-121">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="6c2fe-122">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="6c2fe-122">Create an application</span></span>

<span data-ttu-id="6c2fe-123">Därefter måste du skapa en app i B2C-katalogen.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-123">Next, you need to create an app in your B2C directory.</span></span> <span data-ttu-id="6c2fe-124">Det ger Azure AD den information som tjänsten behöver för att kommunicera säkert med din app.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-124">This gives Azure AD information that it needs to communicate securely with your app.</span></span> <span data-ttu-id="6c2fe-125">Både klientappen och webb-API:et representeras av ett enda **program-ID** eftersom de bildar en logisk app.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-125">Both the client app and web API will be represented by a single **Application ID**, because they comprise one logical app.</span></span> <span data-ttu-id="6c2fe-126">Du skapar en app genom att följa [dessa anvisningar](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="6c2fe-126">To create an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="6c2fe-127">Se till att:</span><span class="sxs-lookup"><span data-stu-id="6c2fe-127">Be sure to:</span></span>

- <span data-ttu-id="6c2fe-128">Lägga till ett **webb-API** /**för webbappen** i programmet.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-128">Include a **web app**/**web API** in the application.</span></span>
- <span data-ttu-id="6c2fe-129">Ange `http://localhost:3000/auth/openid/return` som en **Reply URL**.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-129">Enter `http://localhost:3000/auth/openid/return` as a **Reply URL**.</span></span> <span data-ttu-id="6c2fe-130">Den är standard-URL:en för det här kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-130">It is the default URL for this code sample.</span></span>
- <span data-ttu-id="6c2fe-131">Skapa en **programhemlighet** för programmet och kopiera den.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-131">Create an **Application secret** for your application and copy it.</span></span> <span data-ttu-id="6c2fe-132">Du behöver den senare.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-132">You will need it later.</span></span> <span data-ttu-id="6c2fe-133">Tänk på att det här värdet måste ha [ett XM-escape-tecken](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) innan du använder det.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-133">Note that this value needs to be [XML escaped](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) before you use it.</span></span>
- <span data-ttu-id="6c2fe-134">Kopiera **program-ID:t** som har tilldelats din app.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-134">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="6c2fe-135">Du behöver även det senare.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-135">You'll also need this later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="6c2fe-136">Skapa dina principer</span><span class="sxs-lookup"><span data-stu-id="6c2fe-136">Create your policies</span></span>

<span data-ttu-id="6c2fe-137">I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="6c2fe-137">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="6c2fe-138">Den här appen innehåller tre identitetsupplevelser: registrering, inloggning och inloggning med Facebook.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-138">This app contains three identity experiences: sign up, sign in, and sign in by using Facebook.</span></span> <span data-ttu-id="6c2fe-139">Du måste skapa den här principen för varje typ. Mer information finns i [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="6c2fe-139">You need to create this policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="6c2fe-140">Tänk på följande när du skapar dina tre principer:</span><span class="sxs-lookup"><span data-stu-id="6c2fe-140">When you create your three policies, be sure to:</span></span>

- <span data-ttu-id="6c2fe-141">Välj **visningsnamnet** och andra registreringsattribut i registreringsprincipen.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-141">Choose the **Display name** and other sign-up attributes in your sign-up policy.</span></span>
- <span data-ttu-id="6c2fe-142">Välj det **visningsnamn** och **objekt-ID** som programmet gör anspråk på i varje princip.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-142">Choose the **Display name** and **Object ID** application claims in every policy.</span></span> <span data-ttu-id="6c2fe-143">Du kan också välja andra anspråk.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-143">You can choose other claims as well.</span></span>
- <span data-ttu-id="6c2fe-144">Kopiera **namnet** på varje princip när du har skapat den.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-144">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="6c2fe-145">Det bör ha prefixet `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-145">It should have the prefix `b2c_1_`.</span></span>  <span data-ttu-id="6c2fe-146">Du behöver principnamnen senare.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-146">You'll need these policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="6c2fe-147">När du har skapat dina tre principer kan du börja bygga appen.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-147">After you create your three policies, you're ready to build your app.</span></span>

<span data-ttu-id="6c2fe-148">Tänk på att den här artikeln inte beskriver hur du använder de principer som du nyss har skapat.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-148">Note that this article does not cover how to use the policies you just created.</span></span> <span data-ttu-id="6c2fe-149">Börja med [självstudierna för att komma igång med .NET-webbappar](active-directory-b2c-devquickstarts-web-dotnet.md) om du vill veta mer om hur principer fungerar i Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-149">To learn about how policies work in Azure AD B2C, start with the [.NET web app getting started tutorial](active-directory-b2c-devquickstarts-web-dotnet.md).</span></span>

## <a name="add-prerequisites-to-your-directory"></a><span data-ttu-id="6c2fe-150">Gör nödvändiga förberedelser för katalogen</span><span class="sxs-lookup"><span data-stu-id="6c2fe-150">Add prerequisites to your directory</span></span>

<span data-ttu-id="6c2fe-151">Från kommandoraden byter du katalog till din rotmapp om du inte redan är där.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-151">From the command line, change directories to your root folder, if you're not already there.</span></span> <span data-ttu-id="6c2fe-152">Kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="6c2fe-152">Run the following commands:</span></span>

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`
- `npm install webfinger`
- `npm install body-parser`
- `npm install express-session`
- `npm install cookie-parser`

<span data-ttu-id="6c2fe-153">Dessutom använde vi `passport-azure-ad` för vår förhandsgranskning i stommen i Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-153">In addition, we used `passport-azure-ad` for our preview in the skeleton of the Quickstart.</span></span>

- `npm install passport-azure-ad`

<span data-ttu-id="6c2fe-154">Detta installerar biblioteken som `passport-azure-ad` är beroende av.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-154">This will install the libraries that `passport-azure-ad` depends on.</span></span>

## <a name="set-up-your-app-to-use-the-passport-nodejs-strategy"></a><span data-ttu-id="6c2fe-155">Konfigurera din app att använda Passport-Node.js-strategin</span><span class="sxs-lookup"><span data-stu-id="6c2fe-155">Set up your app to use the Passport-Node.js strategy</span></span>
<span data-ttu-id="6c2fe-156">Konfigurera Express-mellanprogrammet att använda autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-156">Configure the Express middleware to use the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="6c2fe-157">Passport används för att utfärda inloggnings- och utloggningsförfrågningar, hantera användarsessioner, få information om användare och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-157">Passport will be used to issue sign-in and sign-out requests, manage user sessions, and get information about users, among other actions.</span></span>

<span data-ttu-id="6c2fe-158">Öppna `config.js`-filen i projektroten och ange din apps konfigurationsvärden i `exports.creds`-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-158">Open the `config.js` file in the root of the project and enter your app's configuration values in the `exports.creds` section.</span></span>
- <span data-ttu-id="6c2fe-159">`clientID`: Det **Program-ID** som din app tilldelats på registreringsportalen.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-159">`clientID`: The **Application ID** assigned to your app in the registration portal.</span></span>
- <span data-ttu-id="6c2fe-160">`returnURL`: Det **Omdirigerings-URI** som du angav i portalen.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-160">`returnURL`: The **Redirect URI** you entered in the portal.</span></span>
- <span data-ttu-id="6c2fe-161">`tenantName`: Klientnamnet för din app, till exempel **contoso.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-161">`tenantName`: The tenant name of your app, for example, **contoso.onmicrosoft.com**.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

<span data-ttu-id="6c2fe-162">Öppna `app.js`-filen i projektroten.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-162">Open the `app.js` file in the root of the project.</span></span> <span data-ttu-id="6c2fe-163">Lägg till följande anrop för att anropa `OIDCStrategy`-strategin som medföljer `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-163">Add the following call to invoke the `OIDCStrategy` strategy that comes with `passport-azure-ad`.</span></span>


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

<span data-ttu-id="6c2fe-164">Använd strategin som du precis refererade till för att hantera inloggningsförfrågningar.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-164">Use the strategy you just referenced to handle sign-in requests.</span></span>

```JavaScript
// Use the OIDCStrategy in Passport (Section 2).
//
//   Strategies in Passport require a "validate" function that accepts
//   credentials (in this case, an OpenID identifier), and invokes a callback
//   by using a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    skipUserProfile: config.creds.skipUserProfile,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode,
    tenantName: config.creds.tenantName
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    log.info('Example: Email address we received was: ', profile.email);
    // asynchronous verification, for effect...
    process.nextTick(function () {
      findByEmail(profile.email, function(err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          // "Auto-registration"
          users.push(profile);
          return done(null, profile);
        }
        return done(null, user);
      });
    });
  }
));
```
<span data-ttu-id="6c2fe-165">Passport använder ett liknande mönster för alla dess strategier (inklusive Twitter och Facebook).</span><span class="sxs-lookup"><span data-stu-id="6c2fe-165">Passport uses a similar pattern for all of its strategies (including Twitter and Facebook).</span></span> <span data-ttu-id="6c2fe-166">Alla strategigenererare följer detta mönster.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-166">All strategy writers adhere to this pattern.</span></span> <span data-ttu-id="6c2fe-167">När du tittar på strategin kan du se att du skickar ett `function()` som har en token och en `done` som parametrar.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-167">When you look at the strategy, you can see that you pass it a `function()` that has a token and a `done` as the parameters.</span></span> <span data-ttu-id="6c2fe-168">Strategin kommer tillbaka när den har utfört allt sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-168">The strategy comes back to you after it has done all of its work.</span></span> <span data-ttu-id="6c2fe-169">Lagra användaren och token så att du inte behöver fråga efter den igen.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-169">Store the user and stash the token so that you don’t need to ask for it again.</span></span>

> [!IMPORTANT]
<span data-ttu-id="6c2fe-170">Föregående kod körs på alla användare som servern autentiserar.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-170">The preceding code takes all users whom the server authenticates.</span></span> <span data-ttu-id="6c2fe-171">Det här är automatisk registrering.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-171">This is autoregistration.</span></span> <span data-ttu-id="6c2fe-172">När du använder produktionsservrar vill du inte släppa in användare om de inte har genomgått en registreringsprocess som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-172">When you use production servers, you don’t want to let in users unless they have gone through a registration process that you have set up.</span></span> <span data-ttu-id="6c2fe-173">Det här mönstret är vanligt i konsumentappar.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-173">You can often see this pattern in consumer apps.</span></span> <span data-ttu-id="6c2fe-174">Dessa appar låter användaren registrera sig med hjälp av Facebook, men uppmanar sedan användaren att fylla i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-174">These allow you to register by using Facebook, but then they ask you to fill out additional information.</span></span> <span data-ttu-id="6c2fe-175">Om vårt program inte var ett exempelprogram skulle vi kunna extrahera en e-postadress från tokenobjektet som returneras och sedan be användaren att fylla i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-175">If our application wasn’t a sample, we could extract an email address from the token object that is returned, and then ask the user to fill out additional information.</span></span> <span data-ttu-id="6c2fe-176">Eftersom det här är en testserver lägger vi bara till användare i den minnesinterna databasen.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-176">Because this is a test server, we simply add users to the in-memory database.</span></span>

<span data-ttu-id="6c2fe-177">Lägg till metoder som gör att du kan hålla reda på användare som har loggat in, på det sätt som krävs av Passport.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-177">Add the methods that allow you to keep track of users who have signed in, as required by Passport.</span></span> <span data-ttu-id="6c2fe-178">Serialisering och avserialisering av användarinformation är ett par exempel på den här typen av metoder:</span><span class="sxs-lookup"><span data-stu-id="6c2fe-178">This includes serializing and deserializing user information:</span></span>

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent sign-in sessions, Passport needs to be able to
//   serialize users into and deserialize users out of sessions. Typically,
//   this is as simple as storing the user ID when Passport serializes a user
//   and finding the user by ID when Passport deserializes that user.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// Array to hold users who have signed in
var users = [];

var findByEmail = function(email, fn) {
  for (var i = 0, len = users.length; i < len; i++) {
    var user = users[i];
    log.info('we are using user: ', user);
    if (user.email === email) {
      return fn(null, user);
    }
  }
  return fn(null, null);
};

```

<span data-ttu-id="6c2fe-179">Lägg till koden för att läsa in Express-motorn.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-179">Add the code to load the Express engine.</span></span> <span data-ttu-id="6c2fe-180">Nedan kan du se att vi använder `/views`- och `/routes`-standardmönstret som Express tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-180">In the following, you can see that we use the default `/views` and `/routes` pattern that Express provides.</span></span>

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware to support
  // persistent sign-in sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

<span data-ttu-id="6c2fe-181">Lägg till `POST`-dirigeringskommandona som lämnar över själva inloggningsförfrågningarna till `passport-azure-ad`-motorn:</span><span class="sxs-lookup"><span data-stu-id="6c2fe-181">Add the `POST` routes that hand off the actual sign-in requests to the `passport-azure-ad` engine:</span></span>

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request. The first step in OpenID authentication involves redirecting
//   the user to an OpenID provider. After the user is authenticated,
//   the OpenID provider redirects the user back to this application at
//   /auth/openid/return

app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authentication was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request. If authentication fails, the user will be redirected back to the
//   sign-in page. Otherwise, the primary route function will be called.
//   In this example, it redirects the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request. If authentication fails, the user will be redirected back to the
//   sign-in page. Otherwise, the primary route function will be called.
//   In this example, it will redirect the user to the home page.

app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });
```

## <a name="use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a><span data-ttu-id="6c2fe-182">Använda Passport för att utfärda inloggnings- och utloggningsförfrågningar till Azure AD</span><span class="sxs-lookup"><span data-stu-id="6c2fe-182">Use Passport to issue sign-in and sign-out requests to Azure AD</span></span>

<span data-ttu-id="6c2fe-183">Nu är din app korrekt konfigurerad för att kommunicera med v2.0-slutpunkten med hjälp av autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-183">Your app is now properly configured to communicate with the v2.0 endpoint by using the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="6c2fe-184">`passport-azure-ad` ansvarar för att utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla användarsessioner.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-184">`passport-azure-ad` has taken care of the details of crafting authentication messages, validating tokens from Azure AD, and maintaining user session.</span></span> <span data-ttu-id="6c2fe-185">Allt som återstår är att ge användarna en metod för att logga in och logga ut och att samla in ytterligare information om användare som har loggat in.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-185">All that remains is to give your users a way to sign in and sign out, and to gather additional information on users who have signed in.</span></span>

<span data-ttu-id="6c2fe-186">Börja med att lägga till de förvalda inloggnings-, konto- och utloggningsmetoderna i `app.js`-filen:</span><span class="sxs-lookup"><span data-stu-id="6c2fe-186">First, add the default, sign-in, account, and sign-out methods to your `app.js` file:</span></span>

```JavaScript

//Routes (Section 4)

app.get('/', function(req, res){
  res.render('index', { user: req.user });
});

app.get('/account', ensureAuthenticated, function(req, res){
  res.render('account', { user: req.user });
});

app.get('/login',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

<span data-ttu-id="6c2fe-187">Granska dessa metoder i detalj:</span><span class="sxs-lookup"><span data-stu-id="6c2fe-187">To review these methods in detail:</span></span>
- <span data-ttu-id="6c2fe-188">`/`-kommandot omdirigerar till `index.ejs`-vyn genom att skicka användaren i begäran (om den finns).</span><span class="sxs-lookup"><span data-stu-id="6c2fe-188">The `/` route redirects to the `index.ejs` view by passing the user in the request (if it exists).</span></span>
- <span data-ttu-id="6c2fe-189">`/account`-kommandot kontrollerar först att du har autentiserats (du ser implementeringen för detta nedan).</span><span class="sxs-lookup"><span data-stu-id="6c2fe-189">The `/account` route first verifies that you are authenticated (the implementation for this is below).</span></span> <span data-ttu-id="6c2fe-190">Därefter skickas användaren med i begäran så att du kan få mer information om användaren.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-190">It then passes the user in the request so that you can get additional information about the user.</span></span>
- <span data-ttu-id="6c2fe-191">`/login`-kommandot anropar `azuread-openidconnect`-autentiseraren från `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-191">The `/login` route calls the `azuread-openidconnect` authenticator from `passport-azure-ad`.</span></span> <span data-ttu-id="6c2fe-192">Om det inte lyckas omdirigerar kommandot användaren tillbaka till `/login`.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-192">If it doesn't succeed, the route redirects the user back to `/login`.</span></span>
- <span data-ttu-id="6c2fe-193">`/logout` anropar bara `logout.ejs` (och dess dirigeringskommando).</span><span class="sxs-lookup"><span data-stu-id="6c2fe-193">`/logout` simply calls `logout.ejs` (and its route).</span></span> <span data-ttu-id="6c2fe-194">Detta kommando rensar cookies och skickar tillbaka användaren till `index.ejs`.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-194">This clears cookies and then returns the user back to `index.ejs`.</span></span>


<span data-ttu-id="6c2fe-195">För den sista delen av `app.js` lägger du till `EnsureAuthenticated`-metoden som används i `/account`-vägen.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-195">For the last part of `app.js`, add the `EnsureAuthenticated` method that is used in the `/account` route.</span></span>

```JavaScript

// Simple route middleware to ensure that the user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected. If
//   the request is authenticated (typically via a persistent sign-in session),
//   then the request will proceed. Otherwise, the user will be redirected to the
//   sign-in page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}

```

<span data-ttu-id="6c2fe-196">Slutligen skapar du själva servern i `app.js`.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-196">Finally, create the server itself in `app.js`.</span></span>

```JavaScript

app.listen(3000);

```


## <a name="create-the-views-and-routes-in-express-to-call-your-policies"></a><span data-ttu-id="6c2fe-197">Skapa vyerna och vägarna i Express för att anropa dina principer</span><span class="sxs-lookup"><span data-stu-id="6c2fe-197">Create the views and routes in Express to call your policies</span></span>

<span data-ttu-id="6c2fe-198">Nu är din `app.js` klar.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-198">Your `app.js` is now complete.</span></span> <span data-ttu-id="6c2fe-199">Du behöver bara lägga till vägarna och vyerna som gör att du kan anropa inloggnings- och registreringsprinciperna.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-199">You just need to add the routes and views that allow you to call the sign-in and sign-up policies.</span></span> <span data-ttu-id="6c2fe-200">Dessa hanterar även de `/logout`- och `/login`-vägar som du skapade.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-200">These also handle the `/logout` and `/login` routes you created.</span></span>

<span data-ttu-id="6c2fe-201">Skapa `/routes/index.js`-vägen under rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-201">Create the `/routes/index.js` route under the root directory.</span></span>

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

<span data-ttu-id="6c2fe-202">Skapa `/routes/user.js`-vägen under rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-202">Create the `/routes/user.js` route under the root directory.</span></span>

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

<span data-ttu-id="6c2fe-203">Dessa enkla vägar skickar vidare förfrågningar till dina vyer.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-203">These simple routes pass along requests to your views.</span></span> <span data-ttu-id="6c2fe-204">De innehåller användaren, om en sådan finns.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-204">They include the user, if one is present.</span></span>

<span data-ttu-id="6c2fe-205">Skapa `/views/index.ejs`-vyn under rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-205">Create the `/views/index.ejs` view under the root directory.</span></span> <span data-ttu-id="6c2fe-206">Det här är en enkel sida som anropar principer för in- och utloggning.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-206">This is a simple page that calls policies for sign-in and sign-out.</span></span> <span data-ttu-id="6c2fe-207">Du kan också använda den för att hämta kontoinformation.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-207">You can also use it to grab account information.</span></span> <span data-ttu-id="6c2fe-208">Notera att du kan använda det villkorliga `if (!user)` som användaren som skickas med i begäran för att bevisa att användaren är inloggad.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-208">Note that you can use the conditional `if (!user)` as the user is passed through in the request to provide evidence that the user is signed in.</span></span>

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please sign in.</h2>
    <a href="/login/?p=your facebook policy">Sign in with Facebook</a>
    <a href="/login/?p=your email sign-in policy">Sign in with email</a>
    <a href="/login/?p=your email sign-up policy">Sign up with email</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account info</a></br>
    <a href="/logout">Log out</a>
<% } %>
```

<span data-ttu-id="6c2fe-209">Skapa `/views/account.ejs`-vyn under rotkatalogen så att du kan visa ytterligare information som `passport-azure-ad` har lagt till i användarbegäran.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-209">Create the `/views/account.ejs` view under the root directory so that you can view additional information that `passport-azure-ad` put in the user request.</span></span>

```Javascript
<% if (!user) { %>
    <h2>Welcome! Please sign in.</h2>
    <a href="/login">Sign in</a>
<% } else { %>
<p>displayName: <%= user.displayName %></p>
<p>givenName: <%= user.name.givenName %></p>
<p>familyName: <%= user.name.familyName %></p>
<p>UPN: <%= user._json.upn %></p>
<p>Profile ID: <%= user.id %></p>
<p>Full Claims</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

<span data-ttu-id="6c2fe-210">Nu kan du skapa och köra din app.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-210">You can now build and run your app.</span></span>

<span data-ttu-id="6c2fe-211">Kör `node app.js` och gå till `http://localhost:3000`</span><span class="sxs-lookup"><span data-stu-id="6c2fe-211">Run `node app.js` and navigate to `http://localhost:3000`</span></span>


<span data-ttu-id="6c2fe-212">Registrera dig eller logga in i appen med din e-postadress eller via Facebook.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-212">Sign up or sign in to the app by using email or Facebook.</span></span> <span data-ttu-id="6c2fe-213">Logga ut och logga in igen som en annan användare.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-213">Sign out and sign back in as a different user.</span></span>

##<a name="next-steps"></a><span data-ttu-id="6c2fe-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c2fe-214">Next steps</span></span>

<span data-ttu-id="6c2fe-215">Det färdiga exemplet (utan dina konfigurationsvärden) är tillgängligt som referens i form av en [ZIP-fil](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="6c2fe-215">For reference, the completed sample (without your configuration values) [is provided as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span></span> <span data-ttu-id="6c2fe-216">Du kan också klona det från GitHub:</span><span class="sxs-lookup"><span data-stu-id="6c2fe-216">You can also clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="6c2fe-217">Nu kan du gå vidare till mer avancerade avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6c2fe-217">You can now move on to more advanced topics.</span></span> <span data-ttu-id="6c2fe-218">Du kan prova följande:</span><span class="sxs-lookup"><span data-stu-id="6c2fe-218">You might try:</span></span>

[<span data-ttu-id="6c2fe-219">Skydda ett webb-API med hjälp av B2C-modellen i Node.js</span><span class="sxs-lookup"><span data-stu-id="6c2fe-219">Secure a web API by using the B2C model in Node.js</span></span>](active-directory-b2c-devquickstarts-api-node.md)

<!--

For additional resources, check out:
You can now move on to more advanced B2C topics. You might try:

[Call a Node.js web API from a Node.js web app]()

[Customizing the your B2C App's UX >>]()

-->
