---
title: Azure Active Directory v2.0 Node.js web app inloggning | Microsoft Docs
description: "Lär dig hur du skapar en Node.js-webbapp som loggar in användare med hjälp av både ett personligt microsoftkonto och ett arbets- eller skolkonto konto."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 1b889e72-f5c3-464a-af57-79abf5e2e147
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 6d49c742f72440e22830915c90de009d9188db2a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-a-nodejs-web-app"></a><span data-ttu-id="bda13-103">Lägga till inloggning till en Node.js-webbapp</span><span class="sxs-lookup"><span data-stu-id="bda13-103">Add sign-in to a Node.js web app</span></span>

> [!NOTE]
> <span data-ttu-id="bda13-104">Inte alla Azure Active Directory-scenarier och funktioner fungerar med v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="bda13-104">Not all Azure Active Directory scenarios and features work with the v2.0 endpoint.</span></span> <span data-ttu-id="bda13-105">Läs mer om för att avgöra om du ska använda v2.0-slutpunkten eller v1.0 slutpunkten [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="bda13-105">To determine whether you should use the v2.0 endpoint or the v1.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 

<span data-ttu-id="bda13-106">I den här självstudiekursen använda vi Passport för att göra följande:</span><span class="sxs-lookup"><span data-stu-id="bda13-106">In this tutorial, we use Passport to do the following tasks:</span></span>

* <span data-ttu-id="bda13-107">Logga in användaren med hjälp av Azure Active Directory (Azure AD) och v2.0-slutpunkten i en webbapp.</span><span class="sxs-lookup"><span data-stu-id="bda13-107">In a web app, sign in the user by using Azure Active Directory (Azure AD) and the v2.0 endpoint.</span></span>
* <span data-ttu-id="bda13-108">Visa information om användaren.</span><span class="sxs-lookup"><span data-stu-id="bda13-108">Display information about the user.</span></span>
* <span data-ttu-id="bda13-109">Logga ut från appen användaren.</span><span class="sxs-lookup"><span data-stu-id="bda13-109">Sign the user out of the app.</span></span>

<span data-ttu-id="bda13-110">**Passport** är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="bda13-110">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="bda13-111">Flexibel och modulära, Passport kan släppas diskret in i alla Express-baserade eller restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="bda13-111">Flexible and modular, Passport can be unobtrusively dropped into any Express-based or restify web application.</span></span> <span data-ttu-id="bda13-112">I Passport, en omfattande uppsättning strategier stöder autentisering med ett användarnamn och lösenord, Facebook, Twitter eller andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="bda13-112">In Passport, a comprehensive set of strategies support authentication by using a username and password, Facebook, Twitter, or other options.</span></span> <span data-ttu-id="bda13-113">Vi har utvecklat en strategi för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bda13-113">We have developed a strategy for Azure AD.</span></span> <span data-ttu-id="bda13-114">I den här artikeln vi hur du kan installera modulen och Lägg sedan till Azure AD `passport-azure-ad` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="bda13-114">In this article, we show you how to install the module, and then add the Azure AD `passport-azure-ad` plug-in.</span></span>

## <a name="download"></a><span data-ttu-id="bda13-115">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="bda13-115">Download</span></span>
<span data-ttu-id="bda13-116">Koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).</span><span class="sxs-lookup"><span data-stu-id="bda13-116">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).</span></span> <span data-ttu-id="bda13-117">Om du vill följa kursen, kan du [ladda ned appens stomme som en .zip-fil](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) eller klona stommen:</span><span class="sxs-lookup"><span data-stu-id="bda13-117">To follow the tutorial, you can [download the app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="bda13-118">Du kan också få det färdiga programmet i slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="bda13-118">You also can get the completed application at the end of this tutorial.</span></span>

## <a name="1-register-an-app"></a><span data-ttu-id="bda13-119">1: registrera en app</span><span class="sxs-lookup"><span data-stu-id="bda13-119">1: Register an app</span></span>
<span data-ttu-id="bda13-120">Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa [dessa detaljerade steg](active-directory-v2-app-registration.md) att registrera en app.</span><span class="sxs-lookup"><span data-stu-id="bda13-120">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow [these detailed steps](active-directory-v2-app-registration.md) to register an app.</span></span> <span data-ttu-id="bda13-121">Se till att du:</span><span class="sxs-lookup"><span data-stu-id="bda13-121">Make sure you:</span></span>

* <span data-ttu-id="bda13-122">Kopiera den **program-Id** tilldelats din app.</span><span class="sxs-lookup"><span data-stu-id="bda13-122">Copy the **Application Id** assigned to your app.</span></span> <span data-ttu-id="bda13-123">Du behöver den för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="bda13-123">You need it for this tutorial.</span></span>
* <span data-ttu-id="bda13-124">Lägg till den **Web** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="bda13-124">Add the **Web** platform for your app.</span></span>
* <span data-ttu-id="bda13-125">Kopiera den **omdirigerings-URI** från portalen.</span><span class="sxs-lookup"><span data-stu-id="bda13-125">Copy the **Redirect URI** from the portal.</span></span> <span data-ttu-id="bda13-126">Du måste använda URI standardvärdet `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="bda13-126">You must use the default URI value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="2-add-prerequisities-to-your-directory"></a><span data-ttu-id="bda13-127">2: lägga till prerequisities i katalogen</span><span class="sxs-lookup"><span data-stu-id="bda13-127">2: Add prerequisities to your directory</span></span>
<span data-ttu-id="bda13-128">Ändra kataloger för att gå till din rotmapp om du inte redan har det i en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="bda13-128">At a command prompt, change directories to go to your root folder, if you are not already there.</span></span> <span data-ttu-id="bda13-129">Kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="bda13-129">Run the following commands:</span></span>

* `npm install express`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install restify`
* `npm install mongoose`
* `npm install bunyan`
* `npm install assert-plus`
* `npm install passport`
* `npm install webfinger`
* `npm install body-parser`
* `npm install express-session`
* `npm install cookie-parser`

<span data-ttu-id="bda13-130">Dessutom kan vi använda `passport-azure-ad` i stommen i Snabbstart:</span><span class="sxs-lookup"><span data-stu-id="bda13-130">In addition, we use `passport-azure-ad` in the skeleton of the quickstart:</span></span>

* `npm install passport-azure-ad`

<span data-ttu-id="bda13-131">Detta installerar biblioteken som `passport-azure-ad` använder.</span><span class="sxs-lookup"><span data-stu-id="bda13-131">This installs the libraries that `passport-azure-ad` uses.</span></span>

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a><span data-ttu-id="bda13-132">3: konfigurera din app att använda passport-nod-js-strategi</span><span class="sxs-lookup"><span data-stu-id="bda13-132">3: Set up your app to use the passport-node-js strategy</span></span>
<span data-ttu-id="bda13-133">Konfigurera Express-mellanprogrammet att använda autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="bda13-133">Set up the Express middleware to use the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="bda13-134">Du kan använda Passport för att utfärda inloggnings- och utloggningsförfrågningar, hantera användarens session och få information om användare, bland annat.</span><span class="sxs-lookup"><span data-stu-id="bda13-134">You use Passport to issue sign-in and sign-out requests, manage the user's session, and get information about the user, among other things.</span></span>

1.  <span data-ttu-id="bda13-135">Öppna filen Config.js i roten av projektet.</span><span class="sxs-lookup"><span data-stu-id="bda13-135">In the root of the project, open the Config.js file.</span></span> <span data-ttu-id="bda13-136">I den `exports.creds` ange din Apps konfigurationsvärden.</span><span class="sxs-lookup"><span data-stu-id="bda13-136">In the `exports.creds` section, enter your app's configuration values.</span></span>
  
  * <span data-ttu-id="bda13-137">`clientID`: **Program-Id** som har tilldelats din app i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bda13-137">`clientID`: The **Application Id** that's assigned to your app in the Azure portal.</span></span>
  * <span data-ttu-id="bda13-138">`returnURL`: **Omdirigerings-URI** som du angav på portalen.</span><span class="sxs-lookup"><span data-stu-id="bda13-138">`returnURL`: The **Redirect URI** that you entered in the portal.</span></span>
  * <span data-ttu-id="bda13-139">`clientSecret`: Den hemlighet som du genererade på portalen.</span><span class="sxs-lookup"><span data-stu-id="bda13-139">`clientSecret`: The secret that you generated in the portal.</span></span>

2.  <span data-ttu-id="bda13-140">Öppna filen App.js i roten av projektet.</span><span class="sxs-lookup"><span data-stu-id="bda13-140">In the root of the project, open the App.js file.</span></span> <span data-ttu-id="bda13-141">Att anropa OIDCStrategy stratey, som medföljer `passport-azure-ad`, Lägg till följande anrop:</span><span class="sxs-lookup"><span data-stu-id="bda13-141">To invoke the OIDCStrategy stratey, which comes with `passport-azure-ad`, add the following call:</span></span>

  ```JavaScript
  var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

  // Add some logging
  var log = bunyan.createLogger({
      name: 'Microsoft OIDC Example Web Application'
  });
  ```

3.  <span data-ttu-id="bda13-142">Använd strategin som du precis refererade till för att hantera dina inloggning begäranden:</span><span class="sxs-lookup"><span data-stu-id="bda13-142">To handle your sign-in requests, use the strategy you just referenced:</span></span>

  ```JavaScript
  // Use the OIDCStrategy within Passport (section 2)
  //
  //   Strategies in Passport require a `validate` function. The function accepts
  //   credentials (in this case, an OpenID identifier), and invokes a callback
  //   with a user object.
  passport.use( new OIDCStrategy({
      callbackURL: config.creds.returnURL,
      realm: config.creds.realm,
      clientID: config.creds.clientID,
      clientSecret: config.creds.clientSecret,
      oidcIssuer: config.creds.issuer,
      identityMetadata: config.creds.identityMetadata,
      responseType: config.creds.responseType,
      responseMode: config.creds.responseMode,
      skipUserProfile: config.creds.skipUserProfile
      scope: config.creds.scope
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
      log.info('Example: Email address we received was: ', profile.email);
      // Asynchronous verification, for effect...
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

<span data-ttu-id="bda13-143">Passport använder ett liknande mönster för alla sina strategier (Twitter, Facebook och så vidare).</span><span class="sxs-lookup"><span data-stu-id="bda13-143">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on).</span></span> <span data-ttu-id="bda13-144">Alla strategigenererare följer mönstret.</span><span class="sxs-lookup"><span data-stu-id="bda13-144">All strategy writers adhere to the pattern.</span></span> <span data-ttu-id="bda13-145">Skicka en strategi för en `function()` som använder en token och `done` som parametrar.</span><span class="sxs-lookup"><span data-stu-id="bda13-145">Pass the strategy a `function()` that uses a token and `done` as parameters.</span></span> <span data-ttu-id="bda13-146">Strategin som returneras när den gör allt arbete.</span><span class="sxs-lookup"><span data-stu-id="bda13-146">The strategy is returned after it does all its work.</span></span> <span data-ttu-id="bda13-147">Lagra användaren och token så att du inte behöver fråga efter den igen.</span><span class="sxs-lookup"><span data-stu-id="bda13-147">Store the user and stash the token so you don’t need to ask for it again.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="bda13-148">Föregående kod tar alla användare som kan autentisera till servern.</span><span class="sxs-lookup"><span data-stu-id="bda13-148">The preceding code takes any user that can authenticate to your server.</span></span> <span data-ttu-id="bda13-149">Detta kallas för automatisk registrering.</span><span class="sxs-lookup"><span data-stu-id="bda13-149">This is known as auto-registration.</span></span> <span data-ttu-id="bda13-150">På en produktionsserver skulle du vill låta alla i utan att behöva dem gå igenom den registreringsprocess som du väljer.</span><span class="sxs-lookup"><span data-stu-id="bda13-150">On a production server, you wouldn’t want to let anyone in without first having them go through a registration process that you choose.</span></span> <span data-ttu-id="bda13-151">Detta är oftast det mönster som du ser i konsumentappar.</span><span class="sxs-lookup"><span data-stu-id="bda13-151">This is usually the pattern that you see in consumer apps.</span></span> <span data-ttu-id="bda13-152">Appen kan tillåta dig att registrera med Facebook, men sedan du ombedd att ange ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="bda13-152">The app might allow you to register with Facebook, but then it asks you to enter additional information.</span></span> <span data-ttu-id="bda13-153">Om du inte använde ett kommandoradsprogram för den här självstudiekursen, kan du extrahera den e-postadressen från tokenobjektet som returneras.</span><span class="sxs-lookup"><span data-stu-id="bda13-153">If you weren't using a command-line program for this tutorial, you could extract the email from the token object that is returned.</span></span> <span data-ttu-id="bda13-154">Sedan kan du be användaren att ange ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="bda13-154">Then, you might ask the user to enter additional information.</span></span> <span data-ttu-id="bda13-155">Eftersom detta är en testserver kan du lägga till användaren direkt till den minnesintern databasen.</span><span class="sxs-lookup"><span data-stu-id="bda13-155">Because this is a test server, you add the user directly to the in-memory database.</span></span>
  > 
  > 

4.  <span data-ttu-id="bda13-156">Lägg till metoder som används för att hålla reda på användare som är inloggad, vilket krävs av Passport.</span><span class="sxs-lookup"><span data-stu-id="bda13-156">Add the methods that you use to keep track of users who are signed in, as required by Passport.</span></span> <span data-ttu-id="bda13-157">Detta inkluderar serialisering och avserialisering av användarens information:</span><span class="sxs-lookup"><span data-stu-id="bda13-157">This includes serializing and deserializing the user's information:</span></span>

  ```JavaScript

  // Passport session setup (section 2)

  //   To support persistent login sessions, Passport needs to be able to
  //   serialize users into, and deserialize users out of, the session. Typically,
  //   this is as simple as storing the user ID when serializing, and finding
  //   the user by ID when deserializing.
  passport.serializeUser(function(user, done) {
    done(null, user.email);
  });

  passport.deserializeUser(function(id, done) {
    findByEmail(id, function (err, user) {
      done(err, user);
    });
  });

  // Array to hold signed-in users
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

5.  <span data-ttu-id="bda13-158">Lägg till kod som läser in Express-motorn.</span><span class="sxs-lookup"><span data-stu-id="bda13-158">Add the code that loads the Express engine.</span></span> <span data-ttu-id="bda13-159">Du använder standard-/views och /routes mönster som Express tillhandahåller:</span><span class="sxs-lookup"><span data-stu-id="bda13-159">You use the default /views and /routes pattern that Express provides:</span></span>

  ```JavaScript

  // Set up Express (section 2)

  var app = express();

  app.configure(function() {
    app.set('views', __dirname + '/views');
    app.set('view engine', 'ejs');
    app.use(express.logger());
    app.use(express.methodOverride());
    app.use(cookieParser());
    app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
    app.use(bodyParser.urlencoded({ extended : true }));
    // Initialize Passport!  Also use passport.session() middleware, to support
    // persistent login sessions (recommended).
    app.use(passport.initialize());
    app.use(passport.session());
    app.use(app.router);
    app.use(express.static(__dirname + '/../../public'));
  });

  ```

6.  <span data-ttu-id="bda13-160">Lägg till POST dirigerar att leverera de faktiska inloggning förfrågningar till den `passport-azure-ad` motorn:</span><span class="sxs-lookup"><span data-stu-id="bda13-160">Add the POST routes that hand off the actual sign-in requests to the `passport-azure-ad` engine:</span></span>

  ```JavaScript

  // Auth routes (section 3)

  // GET /auth/openid
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. The first step in OpenID authentication involves redirecting
  //   the user to the user's OpenID provider. After authenticating, the OpenID
  //   provider redirects the user back to this application at
  //   /auth/openid/return.

  app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
      log.info('Authentication was called in the sample');
      res.redirect('/');
    });

  // GET /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called.
  //   In this example, it redirects the user to the home page.
  app.get('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });

  // POST /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called. 
  //   In this example, it redirects the user to the home page.

  app.post('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });
  ```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a><span data-ttu-id="bda13-161">4: använda Passport för att utfärda inloggnings- och utloggningsförfrågningar till Azure AD</span><span class="sxs-lookup"><span data-stu-id="bda13-161">4: Use Passport to issue sign-in and sign-out requests to Azure AD</span></span>
<span data-ttu-id="bda13-162">Appen har konfigurerats för att kommunicera med v2.0-slutpunkten med hjälp av autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="bda13-162">Your app is now set up to communicate with the v2.0 endpoint by using the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="bda13-163">Den `passport-azure-ad` strategi tar hand om alla detaljer om utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla användarsessionen.</span><span class="sxs-lookup"><span data-stu-id="bda13-163">The `passport-azure-ad` strategy takes care of all the details of crafting authentication messages, validating tokens from Azure AD, and maintaining the user session.</span></span> <span data-ttu-id="bda13-164">Återstår gör är att ge användarna ett sätt att logga in och logga ut och för att samla in mer information om den användare som har loggat in.</span><span class="sxs-lookup"><span data-stu-id="bda13-164">All that is left to do is to give your users a way to sign in and sign out, and to gather more information about the user who is signed in.</span></span>

1.  <span data-ttu-id="bda13-165">Lägg till den **standard**, **inloggning**, **konto**, och **logga ut** metoder i filen App.js:</span><span class="sxs-lookup"><span data-stu-id="bda13-165">Add the **default**, **login**, **account**, and **logout** methods to your App.js file:</span></span>

  ```JavaScript

  //Routes (section 4)

  app.get('/', function(req, res){
    res.render('index', { user: req.user });
  });

  app.get('/account', ensureAuthenticated, function(req, res){
    res.render('account', { user: req.user });
  });

  app.get('/login',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
      log.info('Login was called in the sample');
      res.redirect('/');
  });

  app.get('/logout', function(req, res){
    req.logout();
    res.redirect('/');
  });

  ```

  <span data-ttu-id="bda13-166">Här följer information:</span><span class="sxs-lookup"><span data-stu-id="bda13-166">Here are the details:</span></span>
    
    * <span data-ttu-id="bda13-167">Den `/` kommandot omdirigerar till vyn index.ejs.</span><span class="sxs-lookup"><span data-stu-id="bda13-167">The `/` route redirects to the index.ejs view.</span></span> <span data-ttu-id="bda13-168">Passerar användaren i begäran (om den finns).</span><span class="sxs-lookup"><span data-stu-id="bda13-168">It passes the user in the request (if it exists).</span></span>
    * <span data-ttu-id="bda13-169">Den `/account` vidarebefordra först *innebär att autentiseras* (du implementera som i följande kod).</span><span class="sxs-lookup"><span data-stu-id="bda13-169">The `/account` route first *ensures that you are authenticated* (you implement that in the following code).</span></span> <span data-ttu-id="bda13-170">Sedan, skickas användaren i begäran.</span><span class="sxs-lookup"><span data-stu-id="bda13-170">Then, it passes the user in the request.</span></span> <span data-ttu-id="bda13-171">Detta är så att du kan få mer information om användaren.</span><span class="sxs-lookup"><span data-stu-id="bda13-171">This is so you can get more information about the user.</span></span>
    * <span data-ttu-id="bda13-172">Den `/login` kommandot anropar din `azuread-openidconnect` autentiseraren från `passport-azuread`.</span><span class="sxs-lookup"><span data-stu-id="bda13-172">The `/login` route calls your `azuread-openidconnect` authenticator from `passport-azuread`.</span></span> <span data-ttu-id="bda13-173">Om inte fungerar som den dirigerar användaren till `/login`.</span><span class="sxs-lookup"><span data-stu-id="bda13-173">If that doesn't succeed, it redirects the user back to `/login`.</span></span>
    * <span data-ttu-id="bda13-174">Den `/logout` vägen anropar logout.ejs visa (och väg).</span><span class="sxs-lookup"><span data-stu-id="bda13-174">The `/logout` route calls the logout.ejs view (and route).</span></span> <span data-ttu-id="bda13-175">Detta tar bort cookies och returnerar sedan användaren till index.ejs.</span><span class="sxs-lookup"><span data-stu-id="bda13-175">This clears cookies, and then returns the user back to index.ejs.</span></span>

2.  <span data-ttu-id="bda13-176">Lägg till den **EnsureAuthenticated** metod som du använde tidigare i `/account`:</span><span class="sxs-lookup"><span data-stu-id="bda13-176">Add the **EnsureAuthenticated** method that you used earlier in `/account`:</span></span>

  ```JavaScript

  // Route middleware to ensure the user is authenticated (section 4)

  //   Use this route middleware on any resource that needs to be protected. If
  //   the request is authenticated (typically via a persistent login session),
  //   the request proceeds. Otherwise, the user is redirected to the
  //   sign-in page.
  function ensureAuthenticated(req, res, next) {
    if (req.isAuthenticated()) { return next(); }
    res.redirect('/login')
  }

  ```

3.  <span data-ttu-id="bda13-177">I App.js, skapar du servern:</span><span class="sxs-lookup"><span data-stu-id="bda13-177">In App.js, create the server:</span></span>

  ```JavaScript

  app.listen(3000);

  ```


## <a name="5-create-the-views-and-routes-in-express-that-you-show-your-user-on-the-website"></a><span data-ttu-id="bda13-178">5: skapa vyerna och vägarna i Express du visa din användare på webbplatsen</span><span class="sxs-lookup"><span data-stu-id="bda13-178">5: Create the views and routes in Express that you show your user on the website</span></span>
<span data-ttu-id="bda13-179">Lägga till vägarna och vyerna som visar information för användaren.</span><span class="sxs-lookup"><span data-stu-id="bda13-179">Add the routes and views that show information to the user.</span></span> <span data-ttu-id="bda13-180">Vägarna och vyerna hanterar även de `/logout` och `/login` vägar som du skapade.</span><span class="sxs-lookup"><span data-stu-id="bda13-180">The routes and views also handle the `/logout` and `/login` routes that you created.</span></span>

1. <span data-ttu-id="bda13-181">Skapa i rotkatalogen på `/routes/index.js` väg.</span><span class="sxs-lookup"><span data-stu-id="bda13-181">In the root directory, create the `/routes/index.js` route.</span></span>

  ```JavaScript

  /*
  * GET home page.
  */

  exports.index = function(req, res){
    res.render('index', { title: 'Express' });
  };
  ```

2.  <span data-ttu-id="bda13-182">Skapa i rotkatalogen på `/routes/user.js` väg.</span><span class="sxs-lookup"><span data-stu-id="bda13-182">In the root directory, create the `/routes/user.js` route.</span></span>

  ```JavaScript

  /*
  * GET users listing.
  */

  exports.list = function(req, res){
    res.send("respond with a resource");
  };
  ```

  <span data-ttu-id="bda13-183">`/routes/index.js`och `/routes/user.js` är enkla vägar skickar vidare begäran i vyer, inklusive användare, om sådan finns.</span><span class="sxs-lookup"><span data-stu-id="bda13-183">`/routes/index.js` and `/routes/user.js` are simple routes that pass along the request to your views, including the user, if present.</span></span>

3.  <span data-ttu-id="bda13-184">Skapa i rotkatalogen på `/views/index.ejs` vyn.</span><span class="sxs-lookup"><span data-stu-id="bda13-184">In the root directory, create the `/views/index.ejs` view.</span></span> <span data-ttu-id="bda13-185">Den här sidan anropar din **inloggning** och **logga ut** metoder.</span><span class="sxs-lookup"><span data-stu-id="bda13-185">This page calls your **login** and **logout** methods.</span></span> <span data-ttu-id="bda13-186">Du också använda den `/views/index.ejs` vyn för att avbilda kontoinformation.</span><span class="sxs-lookup"><span data-stu-id="bda13-186">You also use the `/views/index.ejs` view to capture account information.</span></span> <span data-ttu-id="bda13-187">Du kan använda villkorliga `if (!user)` som användaren som skickas via i begäran.</span><span class="sxs-lookup"><span data-stu-id="bda13-187">You can use the conditional `if (!user)` as the user being passed through in the request.</span></span> <span data-ttu-id="bda13-188">Det finns bevis att du har en inloggad användare.</span><span class="sxs-lookup"><span data-stu-id="bda13-188">It is evidence that you have a user signed in.</span></span>

  ```JavaScript
  <% if (!user) { %>
      <h2>Welcome! Please sign in.</h2>
      <a href="/login">Sign in</a>
  <% } else { %>
      <h2>Hello, <%= user.displayName %>.</h2>
      <a href="/account">Account info</a></br>
      <a href="/logout">Sign out</a>
  <% } %>
  ```

4.  <span data-ttu-id="bda13-189">Skapa i rotkatalogen på `/views/account.ejs` vyn.</span><span class="sxs-lookup"><span data-stu-id="bda13-189">In the root directory, create the `/views/account.ejs` view.</span></span> <span data-ttu-id="bda13-190">Den `/views/account.ejs` vy kan du visa ytterligare information som `passport-azuread` placerar i användarens begäran.</span><span class="sxs-lookup"><span data-stu-id="bda13-190">The `/views/account.ejs` view allows you to view additional information that `passport-azuread` puts in the user request.</span></span>

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
  <p>Full Claimes</p>
  <%- JSON.stringify(user) %>
  <p></p>
  <a href="/logout">Sign out</a>
  <% } %>
  ```

5.  <span data-ttu-id="bda13-191">Lägg till en layout.</span><span class="sxs-lookup"><span data-stu-id="bda13-191">Add a layout.</span></span> <span data-ttu-id="bda13-192">Skapa i rotkatalogen på `/views/layout.ejs` vyn.</span><span class="sxs-lookup"><span data-stu-id="bda13-192">In the root directory, create the `/views/layout.ejs` view.</span></span>

  ```HTML

  <!DOCTYPE html>
  <html>
      <head>
          <title>Passport-OpenID Example</title>
      </head>
      <body>
          <% if (!user) { %>
              <p>
              <a href="/">Home</a> |
              <a href="/login">Sign in</a>
              </p>
          <% } else { %>
              <p>
              <a href="/">Home</a> |
              <a href="/account">Account</a> |
              <a href="/logout">Sign out</a>
              </p>
          <% } %>
          <%- body %>
      </body>
  </html>
  ```

6.  <span data-ttu-id="bda13-193">Om du vill skapa och köra appen kör `node app.js`.</span><span class="sxs-lookup"><span data-stu-id="bda13-193">To build and run your app, run `node app.js`.</span></span> <span data-ttu-id="bda13-194">Gå sedan till `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="bda13-194">Then, go to `http://localhost:3000`.</span></span>

7.  <span data-ttu-id="bda13-195">Logga in med ett personligt microsoftkonto eller ett arbets- eller skolkonto konto.</span><span class="sxs-lookup"><span data-stu-id="bda13-195">Sign in with either a personal Microsoft account or a work or school account.</span></span> <span data-ttu-id="bda13-196">Observera att användarens identitet avspeglas i listan över /account.</span><span class="sxs-lookup"><span data-stu-id="bda13-196">Note that the user's identity is reflected in the /account list.</span></span> 

<span data-ttu-id="bda13-197">Nu har du en webbapp som skyddas med hjälp av standardprotokollen.</span><span class="sxs-lookup"><span data-stu-id="bda13-197">You now have a web app that is secured by using industry standard protocols.</span></span> <span data-ttu-id="bda13-198">Du kan autentisera användare i din app genom att använda sina personliga och arbetet eller skolan konton.</span><span class="sxs-lookup"><span data-stu-id="bda13-198">You can authenticate users in your app by using their personal and work or school accounts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bda13-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bda13-199">Next steps</span></span>
<span data-ttu-id="bda13-200">Referens tillhandahålls det slutförda exemplet (utan dina konfigurationsvärden) som [en .zip-fil](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="bda13-200">For reference, the completed sample (without your configuration values) is provided as [a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip).</span></span> <span data-ttu-id="bda13-201">Du kan också klona det från GitHub:</span><span class="sxs-lookup"><span data-stu-id="bda13-201">You also can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="bda13-202">Därefter kan du gå vidare till mer avancerade avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bda13-202">Next, you can move on to more advanced topics.</span></span> <span data-ttu-id="bda13-203">Du kanske vill prova:</span><span class="sxs-lookup"><span data-stu-id="bda13-203">You might want to try:</span></span>

[<span data-ttu-id="bda13-204">Skydda ett Node.js-webb-API med hjälp av v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="bda13-204">Secure a Node.js web API by using the v2.0 endpoint</span></span>](active-directory-v2-devquickstarts-node-api.md)

<span data-ttu-id="bda13-205">Här följer några ytterligare resurser:</span><span class="sxs-lookup"><span data-stu-id="bda13-205">Here are some additional resources:</span></span>

* [<span data-ttu-id="bda13-206">Utvecklarhandbok för Azure AD v2.0</span><span class="sxs-lookup"><span data-stu-id="bda13-206">Azure AD v2.0 developer guide</span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="bda13-207">Stacken spill ”azure-active-directory” tagg</span><span class="sxs-lookup"><span data-stu-id="bda13-207">Stack Overflow "azure-active-directory" tag</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a><span data-ttu-id="bda13-208">Hämta säkerhetsuppdateringar för våra produkter</span><span class="sxs-lookup"><span data-stu-id="bda13-208">Get security updates for our products</span></span>
<span data-ttu-id="bda13-209">Vi rekommenderar att du loggar som ska meddelas när säkerhetsincidenter.</span><span class="sxs-lookup"><span data-stu-id="bda13-209">We encourage you to sign up to be notified when security incidents occur.</span></span> <span data-ttu-id="bda13-210">På den [Microsoft tekniska säkerhetsmeddelanden](https://technet.microsoft.com/security/dd252948) kan prenumerera på rekommendationerna säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="bda13-210">On the [Microsoft Technical Security Notifications](https://technet.microsoft.com/security/dd252948) page, subscribe to Security Advisories Alerts.</span></span>

