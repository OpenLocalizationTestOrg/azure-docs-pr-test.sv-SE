---
title: "aaaAdd inloggning tooa Node.js-webbapp för Azure B2C | Microsoft Docs"
description: "Hur toobuild Node.js-webbapp som loggar in användare med hjälp av en B2C-klient."
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
ms.openlocfilehash: b4c334b1f7a0669df2d0864140603dc55bbb5408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-add-sign-in-tooa-nodejs-web-app"></a><span data-ttu-id="5a5bf-103">Azure AD B2C: Lägga till inloggning tooa Node.js-webbapp</span><span class="sxs-lookup"><span data-stu-id="5a5bf-103">Azure AD B2C: Add sign-in tooa Node.js web app</span></span>

<span data-ttu-id="5a5bf-104">**Passport** är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-104">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="5a5bf-105">Modulbaserade Passport är mycket flexibelt och kan diskret installeras i alla Express-baserade webbappar eller Restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-105">Extremely flexible and modular, Passport can be unobtrusively installed in any Express-based or Restify web application.</span></span> <span data-ttu-id="5a5bf-106">En omfattande uppsättning strategier stöder autentisering med användarnamn och lösenord, Facebook, Twitter och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-106">A comprehensive set of strategies supports authentication by using a user name and password, Facebook, Twitter, and more.</span></span>

<span data-ttu-id="5a5bf-107">Vi har utvecklat en strategi för Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5a5bf-107">We have developed a strategy for Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="5a5bf-108">Du installerar den här modulen och lägger sedan till hello Azure AD `passport-azure-ad` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-108">You will install this module and then add hello Azure AD `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="5a5bf-109">toodo, måste du:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-109">toodo this, you need to:</span></span>

1. <span data-ttu-id="5a5bf-110">Registrera ett program med hjälp av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-110">Register an application by using Azure AD.</span></span>
2. <span data-ttu-id="5a5bf-111">Konfigurera din app toouse hello `passport-azure-ad` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-111">Set up your app toouse hello `passport-azure-ad` plug-in.</span></span>
3. <span data-ttu-id="5a5bf-112">Använda Passport tooissue inloggning och utloggning begäranden tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-112">Use Passport tooissue sign-in and sign-out requests tooAzure AD.</span></span>
4. <span data-ttu-id="5a5bf-113">Skriva ut användardata.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-113">Print user data.</span></span>

<span data-ttu-id="5a5bf-114">Hej koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="5a5bf-114">hello code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS).</span></span> <span data-ttu-id="5a5bf-115">toofollow längs kan du [hämta hello appens stomme som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="5a5bf-115">toofollow along, you can [download hello app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip).</span></span> <span data-ttu-id="5a5bf-116">Du kan också klona stommen hello:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-116">You can also clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="5a5bf-117">programmet hello slutförts tillhandahålls hello slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-117">hello completed application is provided at hello end of this tutorial.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="5a5bf-118">Skaffa en Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="5a5bf-118">Get an Azure AD B2C directory</span></span>

<span data-ttu-id="5a5bf-119">Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-119">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="5a5bf-120">En katalog är en behållare för alla användare, appar, grupper och mer.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-120">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="5a5bf-121">Om du inte redan har en B2C-katalog [skapar du en](active-directory-b2c-get-started.md) innan du fortsätter den här guiden.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-121">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="5a5bf-122">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="5a5bf-122">Create an application</span></span>

<span data-ttu-id="5a5bf-123">Sedan måste toocreate en app i B2C-katalogen.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-123">Next, you need toocreate an app in your B2C directory.</span></span> <span data-ttu-id="5a5bf-124">Det ger Azure AD den information som behövs toocommunicate säkert med din app.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-124">This gives Azure AD information that it needs toocommunicate securely with your app.</span></span> <span data-ttu-id="5a5bf-125">Både hello-klientappen och webb-API representeras av ett enda **program-ID**eftersom de bildar en logisk app.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-125">Both hello client app and web API will be represented by a single **Application ID**, because they comprise one logical app.</span></span> <span data-ttu-id="5a5bf-126">toocreate en app, Följ [instruktionerna](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="5a5bf-126">toocreate an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="5a5bf-127">Se till att:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-127">Be sure to:</span></span>

- <span data-ttu-id="5a5bf-128">Inkludera en **webbapp**/**webb-API** i hello program.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-128">Include a **web app**/**web API** in hello application.</span></span>
- <span data-ttu-id="5a5bf-129">Ange `http://localhost:3000/auth/openid/return` som en **Reply URL**.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-129">Enter `http://localhost:3000/auth/openid/return` as a **Reply URL**.</span></span> <span data-ttu-id="5a5bf-130">Det är hello standard-URL för det här kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-130">It is hello default URL for this code sample.</span></span>
- <span data-ttu-id="5a5bf-131">Skapa en **programhemlighet** för programmet och kopiera den.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-131">Create an **Application secret** for your application and copy it.</span></span> <span data-ttu-id="5a5bf-132">Du behöver den senare.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-132">You will need it later.</span></span> <span data-ttu-id="5a5bf-133">Observera att det här värdet måste toobe [ESC-XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) innan du använder den.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-133">Note that this value needs toobe [XML escaped](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) before you use it.</span></span>
- <span data-ttu-id="5a5bf-134">Kopiera hello **program-ID** som är tilldelade tooyour app.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-134">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="5a5bf-135">Du behöver även det senare.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-135">You'll also need this later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="5a5bf-136">Skapa dina principer</span><span class="sxs-lookup"><span data-stu-id="5a5bf-136">Create your policies</span></span>

<span data-ttu-id="5a5bf-137">I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="5a5bf-137">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="5a5bf-138">Den här appen innehåller tre identitetsupplevelser: registrering, inloggning och inloggning med Facebook.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-138">This app contains three identity experiences: sign up, sign in, and sign in by using Facebook.</span></span> <span data-ttu-id="5a5bf-139">Du behöver toocreate denna princip av varje typ, enligt beskrivningen i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="5a5bf-139">You need toocreate this policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="5a5bf-140">Tänk på följande när du skapar dina tre principer:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-140">When you create your three policies, be sure to:</span></span>

- <span data-ttu-id="5a5bf-141">Välj hello **visningsnamn** och andra registreringsattribut i registreringsprincipen.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-141">Choose hello **Display name** and other sign-up attributes in your sign-up policy.</span></span>
- <span data-ttu-id="5a5bf-142">Välj hello **visningsnamn** och **objekt-ID** programanspråken i varje princip.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-142">Choose hello **Display name** and **Object ID** application claims in every policy.</span></span> <span data-ttu-id="5a5bf-143">Du kan också välja andra anspråk.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-143">You can choose other claims as well.</span></span>
- <span data-ttu-id="5a5bf-144">Kopiera hello **namn** på varje princip när du har skapat den.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-144">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="5a5bf-145">Det bör ha hello prefixet `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-145">It should have hello prefix `b2c_1_`.</span></span>  <span data-ttu-id="5a5bf-146">Du behöver principnamnen senare.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-146">You'll need these policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="5a5bf-147">När du har skapat dina tre principer du är klar toobuild din app.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-147">After you create your three policies, you're ready toobuild your app.</span></span>

<span data-ttu-id="5a5bf-148">Observera att den här artikeln inte beskriver hur toouse hello principer du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-148">Note that this article does not cover how toouse hello policies you just created.</span></span> <span data-ttu-id="5a5bf-149">toolearn om hur principer fungerar i Azure AD B2C, börja med hello [.NET komma igång med webbappen kursen](active-directory-b2c-devquickstarts-web-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="5a5bf-149">toolearn about how policies work in Azure AD B2C, start with hello [.NET web app getting started tutorial](active-directory-b2c-devquickstarts-web-dotnet.md).</span></span>

## <a name="add-prerequisites-tooyour-directory"></a><span data-ttu-id="5a5bf-150">Lägg till krav tooyour katalog</span><span class="sxs-lookup"><span data-stu-id="5a5bf-150">Add prerequisites tooyour directory</span></span>

<span data-ttu-id="5a5bf-151">Ändra kataloger tooyour rotmapp om du inte redan det från hello kommandorad.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-151">From hello command line, change directories tooyour root folder, if you're not already there.</span></span> <span data-ttu-id="5a5bf-152">Kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-152">Run hello following commands:</span></span>

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

<span data-ttu-id="5a5bf-153">Dessutom använde vi `passport-azure-ad` för vår förhandsgranskning i hello stommen i hello Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-153">In addition, we used `passport-azure-ad` for our preview in hello skeleton of hello Quickstart.</span></span>

- `npm install passport-azure-ad`

<span data-ttu-id="5a5bf-154">Detta installerar hello bibliotek som `passport-azure-ad` beror på.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-154">This will install hello libraries that `passport-azure-ad` depends on.</span></span>

## <a name="set-up-your-app-toouse-hello-passport-nodejs-strategy"></a><span data-ttu-id="5a5bf-155">Konfigurera din app toouse hello Passport-Node.js-strategin</span><span class="sxs-lookup"><span data-stu-id="5a5bf-155">Set up your app toouse hello Passport-Node.js strategy</span></span>
<span data-ttu-id="5a5bf-156">Konfigurera hello Express mellanprogram toouse hello autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-156">Configure hello Express middleware toouse hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="5a5bf-157">Passport kommer att använda tooissue inloggning och utloggningsförfrågningar, hantera användarsessioner och få information om användare, bland andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-157">Passport will be used tooissue sign-in and sign-out requests, manage user sessions, and get information about users, among other actions.</span></span>

<span data-ttu-id="5a5bf-158">Öppna hello `config.js` filen i hello roten av projektet hello och ange din Apps konfigurationsvärden i hello `exports.creds` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-158">Open hello `config.js` file in hello root of hello project and enter your app's configuration values in hello `exports.creds` section.</span></span>
- <span data-ttu-id="5a5bf-159">`clientID`: hello **program-ID** tilldelade tooyour app i portalen för registrering av hello.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-159">`clientID`: hello **Application ID** assigned tooyour app in hello registration portal.</span></span>
- <span data-ttu-id="5a5bf-160">`returnURL`: hello **omdirigerings-URI** du angav i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-160">`returnURL`: hello **Redirect URI** you entered in hello portal.</span></span>
- <span data-ttu-id="5a5bf-161">`tenantName`: hello innehavarens namn för din app, till exempel **contoso.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-161">`tenantName`: hello tenant name of your app, for example, **contoso.onmicrosoft.com**.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

<span data-ttu-id="5a5bf-162">Öppna hello `app.js` filen i projektet hello hello rot.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-162">Open hello `app.js` file in hello root of hello project.</span></span> <span data-ttu-id="5a5bf-163">Lägg till följande anrop tooinvoke hello hello `OIDCStrategy` strategin som medföljer `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-163">Add hello following call tooinvoke hello `OIDCStrategy` strategy that comes with `passport-azure-ad`.</span></span>


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

<span data-ttu-id="5a5bf-164">Använd hello strategi som du precis refererade till toohandle inloggning begäranden.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-164">Use hello strategy you just referenced toohandle sign-in requests.</span></span>

```JavaScript
// Use hello OIDCStrategy in Passport (Section 2).
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
<span data-ttu-id="5a5bf-165">Passport använder ett liknande mönster för alla dess strategier (inklusive Twitter och Facebook).</span><span class="sxs-lookup"><span data-stu-id="5a5bf-165">Passport uses a similar pattern for all of its strategies (including Twitter and Facebook).</span></span> <span data-ttu-id="5a5bf-166">Alla strategigenererare följer toothis mönster.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-166">All strategy writers adhere toothis pattern.</span></span> <span data-ttu-id="5a5bf-167">När du tittar på strategin för hello kan du se att du skickar en `function()` som har en token och en `done` som hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-167">When you look at hello strategy, you can see that you pass it a `function()` that has a token and a `done` as hello parameters.</span></span> <span data-ttu-id="5a5bf-168">hello strategin kommer tillbaka tooyou när den har utfört allt sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-168">hello strategy comes back tooyou after it has done all of its work.</span></span> <span data-ttu-id="5a5bf-169">Lagra hello användaren och stash hello token så att du inte behöver tooask för den igen.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-169">Store hello user and stash hello token so that you don’t need tooask for it again.</span></span>

> [!IMPORTANT]
<span data-ttu-id="5a5bf-170">hello föregående kod körs på alla användare som autentiseras hello-servern.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-170">hello preceding code takes all users whom hello server authenticates.</span></span> <span data-ttu-id="5a5bf-171">Det här är automatisk registrering.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-171">This is autoregistration.</span></span> <span data-ttu-id="5a5bf-172">När du använder produktionsservrar vill du inte toolet användare om de inte har genomgått en registreringsprocess som du har ställt in.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-172">When you use production servers, you don’t want toolet in users unless they have gone through a registration process that you have set up.</span></span> <span data-ttu-id="5a5bf-173">Det här mönstret är vanligt i konsumentappar.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-173">You can often see this pattern in consumer apps.</span></span> <span data-ttu-id="5a5bf-174">Dessa kan du tooregister med hjälp av Facebook men uppmanar sedan användaren toofill i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-174">These allow you tooregister by using Facebook, but then they ask you toofill out additional information.</span></span> <span data-ttu-id="5a5bf-175">Om vårt program inte var ett exempelprogram, kan vi extrahera en e-postadress från token hello-objekt som returneras och sedan ber du hello användaren toofill i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-175">If our application wasn’t a sample, we could extract an email address from hello token object that is returned, and then ask hello user toofill out additional information.</span></span> <span data-ttu-id="5a5bf-176">Eftersom detta är en testserver lägger vi helt enkelt lägga till användare toohello minnesinterna databasen.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-176">Because this is a test server, we simply add users toohello in-memory database.</span></span>

<span data-ttu-id="5a5bf-177">Lägga till hello-metoder som gör att du tookeep reda på användare som har loggat in, vilket krävs av Passport.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-177">Add hello methods that allow you tookeep track of users who have signed in, as required by Passport.</span></span> <span data-ttu-id="5a5bf-178">Serialisering och avserialisering av användarinformation är ett par exempel på den här typen av metoder:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-178">This includes serializing and deserializing user information:</span></span>

```JavaScript

// Passport session setup. (Section 2)

//   toosupport persistent sign-in sessions, Passport needs toobe able to
//   serialize users into and deserialize users out of sessions. Typically,
//   this is as simple as storing hello user ID when Passport serializes a user
//   and finding hello user by ID when Passport deserializes that user.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// Array toohold users who have signed in
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

<span data-ttu-id="5a5bf-179">Lägg till hello kod tooload hello Express-motorn.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-179">Add hello code tooload hello Express engine.</span></span> <span data-ttu-id="5a5bf-180">I följande hello, ser du att vi använder hello standard `/views` och `/routes` Standardmönstret som Express tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-180">In hello following, you can see that we use hello default `/views` and `/routes` pattern that Express provides.</span></span>

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
  // Initialize Passport!  Also use passport.session() middleware toosupport
  // persistent sign-in sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

<span data-ttu-id="5a5bf-181">Lägg till hello `POST` dirigeringskommandona som lämnar hello faktiska inloggning begäranden toohello `passport-azure-ad` motorn:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-181">Add hello `POST` routes that hand off hello actual sign-in requests toohello `passport-azure-ad` engine:</span></span>

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware tooauthenticate the
//   request. hello first step in OpenID authentication involves redirecting
//   hello user tooan OpenID provider. After hello user is authenticated,
//   hello OpenID provider redirects hello user back toothis application at
//   /auth/openid/return

app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authentication was called in hello Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware tooauthenticate the
//   request. If authentication fails, hello user will be redirected back toothe
//   sign-in page. Otherwise, hello primary route function will be called.
//   In this example, it redirects hello user toohello home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware tooauthenticate the
//   request. If authentication fails, hello user will be redirected back toothe
//   sign-in page. Otherwise, hello primary route function will be called.
//   In this example, it will redirect hello user toohello home page.

app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });
```

## <a name="use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a><span data-ttu-id="5a5bf-182">Använda Passport tooissue inloggning och utloggning begäranden tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="5a5bf-182">Use Passport tooissue sign-in and sign-out requests tooAzure AD</span></span>

<span data-ttu-id="5a5bf-183">Appen är nu korrekt konfigurerade toocommunicate med hello v2.0-slutpunkten med hjälp av autentiseringsprotokollet för hello OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-183">Your app is now properly configured toocommunicate with hello v2.0 endpoint by using hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="5a5bf-184">`passport-azure-ad`har tagit hand om hello information utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla användarsessioner.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-184">`passport-azure-ad` has taken care of hello details of crafting authentication messages, validating tokens from Azure AD, and maintaining user session.</span></span> <span data-ttu-id="5a5bf-185">Allt som fortfarande är toogive användarna en sätt toosign i och logga ut och toogather ytterligare information om användare som har loggat in.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-185">All that remains is toogive your users a way toosign in and sign out, and toogather additional information on users who have signed in.</span></span>

<span data-ttu-id="5a5bf-186">Lägg först till hello standard, logga in, konto och utloggningsmetoderna tooyour `app.js` fil:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-186">First, add hello default, sign-in, account, and sign-out methods tooyour `app.js` file:</span></span>

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
    log.info('Login was called in hello Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

<span data-ttu-id="5a5bf-187">tooreview dessa metoder i detalj:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-187">tooreview these methods in detail:</span></span>
- <span data-ttu-id="5a5bf-188">Hej `/` kommandot omdirigerar toohello `index.ejs` vyn genom att skicka hello användaren i hello begäran (om den finns).</span><span class="sxs-lookup"><span data-stu-id="5a5bf-188">hello `/` route redirects toohello `index.ejs` view by passing hello user in hello request (if it exists).</span></span>
- <span data-ttu-id="5a5bf-189">Hej `/account` kontrollerar först att du har autentiserats (hello implementeringen för detta visas nedan).</span><span class="sxs-lookup"><span data-stu-id="5a5bf-189">hello `/account` route first verifies that you are authenticated (hello implementation for this is below).</span></span> <span data-ttu-id="5a5bf-190">Därefter skickas användaren hello i hello begäran så att du kan få ytterligare information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-190">It then passes hello user in hello request so that you can get additional information about hello user.</span></span>
- <span data-ttu-id="5a5bf-191">Hej `/login` väg anrop hello `azuread-openidconnect` autentiseraren från `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-191">hello `/login` route calls hello `azuread-openidconnect` authenticator from `passport-azure-ad`.</span></span> <span data-ttu-id="5a5bf-192">Om det inte lyckas hello kommandot omdirigerar hello användaren tillbaka för`/login`.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-192">If it doesn't succeed, hello route redirects hello user back too`/login`.</span></span>
- <span data-ttu-id="5a5bf-193">`/logout` anropar bara `logout.ejs` (och dess dirigeringskommando).</span><span class="sxs-lookup"><span data-stu-id="5a5bf-193">`/logout` simply calls `logout.ejs` (and its route).</span></span> <span data-ttu-id="5a5bf-194">Detta kommando rensar cookies och sedan returnerar hello användaren tillbaka för`index.ejs`.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-194">This clears cookies and then returns hello user back too`index.ejs`.</span></span>


<span data-ttu-id="5a5bf-195">För hello sista delen av `app.js`, lägga till hello `EnsureAuthenticated` metod som används i hello `/account` vägen.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-195">For hello last part of `app.js`, add hello `EnsureAuthenticated` method that is used in hello `/account` route.</span></span>

```JavaScript

// Simple route middleware tooensure that hello user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs toobe protected. If
//   hello request is authenticated (typically via a persistent sign-in session),
//   then hello request will proceed. Otherwise, hello user will be redirected toothe
//   sign-in page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}

```

<span data-ttu-id="5a5bf-196">Skapa slutligen hello-servern i `app.js`.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-196">Finally, create hello server itself in `app.js`.</span></span>

```JavaScript

app.listen(3000);

```


## <a name="create-hello-views-and-routes-in-express-toocall-your-policies"></a><span data-ttu-id="5a5bf-197">Skapa hello vyer och dirigerar i Express toocall dina principer</span><span class="sxs-lookup"><span data-stu-id="5a5bf-197">Create hello views and routes in Express toocall your policies</span></span>

<span data-ttu-id="5a5bf-198">Nu är din `app.js` klar.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-198">Your `app.js` is now complete.</span></span> <span data-ttu-id="5a5bf-199">Du behöver bara tooadd hello vägarna och vyerna som gör att du toocall hello principer för inloggning och registrering.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-199">You just need tooadd hello routes and views that allow you toocall hello sign-in and sign-up policies.</span></span> <span data-ttu-id="5a5bf-200">Dessa hanterar även hello `/logout` och `/login` vägar som du skapade.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-200">These also handle hello `/logout` and `/login` routes you created.</span></span>

<span data-ttu-id="5a5bf-201">Skapa hello `/routes/index.js` vägen under rotkatalogen hello.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-201">Create hello `/routes/index.js` route under hello root directory.</span></span>

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

<span data-ttu-id="5a5bf-202">Skapa hello `/routes/user.js` vägen under rotkatalogen hello.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-202">Create hello `/routes/user.js` route under hello root directory.</span></span>

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

<span data-ttu-id="5a5bf-203">Dessa enkla vägar skickar vidare förfrågningar tooyour vyer.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-203">These simple routes pass along requests tooyour views.</span></span> <span data-ttu-id="5a5bf-204">De omfattar hello användaren, om en sådan finns.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-204">They include hello user, if one is present.</span></span>

<span data-ttu-id="5a5bf-205">Skapa hello `/views/index.ejs` vyn under rotkatalogen hello.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-205">Create hello `/views/index.ejs` view under hello root directory.</span></span> <span data-ttu-id="5a5bf-206">Det här är en enkel sida som anropar principer för in- och utloggning. Du kan också använda den toograb kontoinformation.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-206">This is a simple page that calls policies for sign-in and sign-out. You can also use it toograb account information.</span></span> <span data-ttu-id="5a5bf-207">Observera att du kan använda villkorlig hello `if (!user)` som hello användaren skickades via hello begäran tooprovide bevis hello användaren är inloggad.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-207">Note that you can use hello conditional `if (!user)` as hello user is passed through in hello request tooprovide evidence that hello user is signed in.</span></span>

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

<span data-ttu-id="5a5bf-208">Skapa hello `/views/account.ejs` vyn under rotkatalogen hello så att du kan visa ytterligare information som `passport-azure-ad` i hello användarens begäran.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-208">Create hello `/views/account.ejs` view under hello root directory so that you can view additional information that `passport-azure-ad` put in hello user request.</span></span>

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

<span data-ttu-id="5a5bf-209">Nu kan du skapa och köra din app.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-209">You can now build and run your app.</span></span>

<span data-ttu-id="5a5bf-210">Kör `node app.js` och navigera för`http://localhost:3000`</span><span class="sxs-lookup"><span data-stu-id="5a5bf-210">Run `node app.js` and navigate too`http://localhost:3000`</span></span>


<span data-ttu-id="5a5bf-211">Registrering eller inloggning toohello appen med hjälp av e-post eller Facebook.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-211">Sign up or sign in toohello app by using email or Facebook.</span></span> <span data-ttu-id="5a5bf-212">Logga ut och logga in igen som en annan användare.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-212">Sign out and sign back in as a different user.</span></span>

##<a name="next-steps"></a><span data-ttu-id="5a5bf-213">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5a5bf-213">Next steps</span></span>

<span data-ttu-id="5a5bf-214">För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [har angetts som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="5a5bf-214">For reference, hello completed sample (without your configuration values) [is provided as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span></span> <span data-ttu-id="5a5bf-215">Du kan också klona det från GitHub:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-215">You can also clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="5a5bf-216">Du kan nu gå vidare toomore avancerade alternativ.</span><span class="sxs-lookup"><span data-stu-id="5a5bf-216">You can now move on toomore advanced topics.</span></span> <span data-ttu-id="5a5bf-217">Du kan prova följande:</span><span class="sxs-lookup"><span data-stu-id="5a5bf-217">You might try:</span></span>

[<span data-ttu-id="5a5bf-218">Skydda ett webb-API med hjälp av hello B2C-modellen i Node.js</span><span class="sxs-lookup"><span data-stu-id="5a5bf-218">Secure a web API by using hello B2C model in Node.js</span></span>](active-directory-b2c-devquickstarts-api-node.md)

<!--

For additional resources, check out:
You can now move on toomore advanced B2C topics. You might try:

[Call a Node.js web API from a Node.js web app]()

[Customizing hello your B2C App's UX >>]()

-->
