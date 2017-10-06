---
title: aaaAzure Active Directory v2.0 Node.js web app inloggning | Microsoft Docs
description: "Lär dig hur toobuild en Node.js webbapp som loggar in användare med hjälp av både ett personligt microsoftkonto och ett arbets- eller skolkonto konto."
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
ms.openlocfilehash: f8ce6e2b841c215cb14e82bcf444fe849634cc88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooa-nodejs-web-app"></a><span data-ttu-id="57ec2-103">Lägga till inloggning tooa Node.js-webbapp</span><span class="sxs-lookup"><span data-stu-id="57ec2-103">Add sign-in tooa Node.js web app</span></span>

> [!NOTE]
> <span data-ttu-id="57ec2-104">Inte alla Azure Active Directory-scenarier och funktioner fungerar med hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="57ec2-104">Not all Azure Active Directory scenarios and features work with hello v2.0 endpoint.</span></span> <span data-ttu-id="57ec2-105">toodetermine om du ska använda hello v2.0-slutpunkten eller hello v1.0 slutpunkt och Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="57ec2-105">toodetermine whether you should use hello v2.0 endpoint or hello v1.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 

<span data-ttu-id="57ec2-106">I den här självstudiekursen kommer använda vi Passport toodo hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="57ec2-106">In this tutorial, we use Passport toodo hello following tasks:</span></span>

* <span data-ttu-id="57ec2-107">Logga in hello användare med hjälp av Azure Active Directory (AD Azure) i en webbapp och hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="57ec2-107">In a web app, sign in hello user by using Azure Active Directory (Azure AD) and hello v2.0 endpoint.</span></span>
* <span data-ttu-id="57ec2-108">Visa information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="57ec2-108">Display information about hello user.</span></span>
* <span data-ttu-id="57ec2-109">Logga hello användare utanför hello appen.</span><span class="sxs-lookup"><span data-stu-id="57ec2-109">Sign hello user out of hello app.</span></span>

<span data-ttu-id="57ec2-110">**Passport** är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="57ec2-110">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="57ec2-111">Flexibel och modulära, Passport kan släppas diskret in i alla Express-baserade eller restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="57ec2-111">Flexible and modular, Passport can be unobtrusively dropped into any Express-based or restify web application.</span></span> <span data-ttu-id="57ec2-112">I Passport, en omfattande uppsättning strategier stöder autentisering med ett användarnamn och lösenord, Facebook, Twitter eller andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="57ec2-112">In Passport, a comprehensive set of strategies support authentication by using a username and password, Facebook, Twitter, or other options.</span></span> <span data-ttu-id="57ec2-113">Vi har utvecklat en strategi för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="57ec2-113">We have developed a strategy for Azure AD.</span></span> <span data-ttu-id="57ec2-114">I den här artikeln visar vi du hur tooinstall hello modulen och lägger sedan till hello Azure AD `passport-azure-ad` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="57ec2-114">In this article, we show you how tooinstall hello module, and then add hello Azure AD `passport-azure-ad` plug-in.</span></span>

## <a name="download"></a><span data-ttu-id="57ec2-115">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="57ec2-115">Download</span></span>
<span data-ttu-id="57ec2-116">hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).</span><span class="sxs-lookup"><span data-stu-id="57ec2-116">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).</span></span> <span data-ttu-id="57ec2-117">toofollow hello självstudier, kan du [hämta hello appens stomme som en .zip-fil](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) eller klona hello stommen:</span><span class="sxs-lookup"><span data-stu-id="57ec2-117">toofollow hello tutorial, you can [download hello app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="57ec2-118">Du kan också få hello slutförts programmet hello slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="57ec2-118">You also can get hello completed application at hello end of this tutorial.</span></span>

## <a name="1-register-an-app"></a><span data-ttu-id="57ec2-119">1: registrera en app</span><span class="sxs-lookup"><span data-stu-id="57ec2-119">1: Register an app</span></span>
<span data-ttu-id="57ec2-120">Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa [dessa detaljerade steg](active-directory-v2-app-registration.md) tooregister en app.</span><span class="sxs-lookup"><span data-stu-id="57ec2-120">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow [these detailed steps](active-directory-v2-app-registration.md) tooregister an app.</span></span> <span data-ttu-id="57ec2-121">Se till att du:</span><span class="sxs-lookup"><span data-stu-id="57ec2-121">Make sure you:</span></span>

* <span data-ttu-id="57ec2-122">Kopiera hello **program-Id** tilldelade tooyour app.</span><span class="sxs-lookup"><span data-stu-id="57ec2-122">Copy hello **Application Id** assigned tooyour app.</span></span> <span data-ttu-id="57ec2-123">Du behöver den för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="57ec2-123">You need it for this tutorial.</span></span>
* <span data-ttu-id="57ec2-124">Lägg till hello **Web** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="57ec2-124">Add hello **Web** platform for your app.</span></span>
* <span data-ttu-id="57ec2-125">Kopiera hello **omdirigerings-URI** från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="57ec2-125">Copy hello **Redirect URI** from hello portal.</span></span> <span data-ttu-id="57ec2-126">Du måste använda hello URI standardvärdet `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="57ec2-126">You must use hello default URI value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="2-add-prerequisities-tooyour-directory"></a><span data-ttu-id="57ec2-127">2: Lägg till prerequisities tooyour katalog</span><span class="sxs-lookup"><span data-stu-id="57ec2-127">2: Add prerequisities tooyour directory</span></span>
<span data-ttu-id="57ec2-128">Ändra kataloger toogo tooyour rotmapp om du inte redan har det i en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="57ec2-128">At a command prompt, change directories toogo tooyour root folder, if you are not already there.</span></span> <span data-ttu-id="57ec2-129">Kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="57ec2-129">Run hello following commands:</span></span>

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

<span data-ttu-id="57ec2-130">Dessutom kan vi använda `passport-azure-ad` i hello stommen i Snabbstart hello:</span><span class="sxs-lookup"><span data-stu-id="57ec2-130">In addition, we use `passport-azure-ad` in hello skeleton of hello quickstart:</span></span>

* `npm install passport-azure-ad`

<span data-ttu-id="57ec2-131">Detta installerar hello bibliotek som `passport-azure-ad` använder.</span><span class="sxs-lookup"><span data-stu-id="57ec2-131">This installs hello libraries that `passport-azure-ad` uses.</span></span>

## <a name="3-set-up-your-app-toouse-hello-passport-node-js-strategy"></a><span data-ttu-id="57ec2-132">3: Ställ in din app toouse hello passport-nod-js-strategi</span><span class="sxs-lookup"><span data-stu-id="57ec2-132">3: Set up your app toouse hello passport-node-js strategy</span></span>
<span data-ttu-id="57ec2-133">Ställ in hello Express mellanprogram toouse hello autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="57ec2-133">Set up hello Express middleware toouse hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="57ec2-134">Använda Passport tooissue inloggning och utloggning begäranden, hantera hello användarens session och få information om hello användare, bland annat.</span><span class="sxs-lookup"><span data-stu-id="57ec2-134">You use Passport tooissue sign-in and sign-out requests, manage hello user's session, and get information about hello user, among other things.</span></span>

1.  <span data-ttu-id="57ec2-135">Hello rot i hello projekt, öppna hello Config.js-fil.</span><span class="sxs-lookup"><span data-stu-id="57ec2-135">In hello root of hello project, open hello Config.js file.</span></span> <span data-ttu-id="57ec2-136">I hello `exports.creds` ange din Apps konfigurationsvärden.</span><span class="sxs-lookup"><span data-stu-id="57ec2-136">In hello `exports.creds` section, enter your app's configuration values.</span></span>
  
  * <span data-ttu-id="57ec2-137">`clientID`: hello **program-Id** som är tilldelade tooyour app i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="57ec2-137">`clientID`: hello **Application Id** that's assigned tooyour app in hello Azure portal.</span></span>
  * <span data-ttu-id="57ec2-138">`returnURL`: hello **omdirigerings-URI** som du angav i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="57ec2-138">`returnURL`: hello **Redirect URI** that you entered in hello portal.</span></span>
  * <span data-ttu-id="57ec2-139">`clientSecret`: hello hemlighet som du skapade i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="57ec2-139">`clientSecret`: hello secret that you generated in hello portal.</span></span>

2.  <span data-ttu-id="57ec2-140">Öppna hello App.js filen i hello roten av hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="57ec2-140">In hello root of hello project, open hello App.js file.</span></span> <span data-ttu-id="57ec2-141">tooinvoke hello OIDCStrategy stratey som medföljer `passport-azure-ad`, Lägg till följande anrop hello:</span><span class="sxs-lookup"><span data-stu-id="57ec2-141">tooinvoke hello OIDCStrategy stratey, which comes with `passport-azure-ad`, add hello following call:</span></span>

  ```JavaScript
  var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

  // Add some logging
  var log = bunyan.createLogger({
      name: 'Microsoft OIDC Example Web Application'
  });
  ```

3.  <span data-ttu-id="57ec2-142">toohandle din inloggning begäranden, Använd hello strategi som du precis refererade till:</span><span class="sxs-lookup"><span data-stu-id="57ec2-142">toohandle your sign-in requests, use hello strategy you just referenced:</span></span>

  ```JavaScript
  // Use hello OIDCStrategy within Passport (section 2)
  //
  //   Strategies in Passport require a `validate` function. hello function accepts
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

<span data-ttu-id="57ec2-143">Passport använder ett liknande mönster för alla sina strategier (Twitter, Facebook och så vidare).</span><span class="sxs-lookup"><span data-stu-id="57ec2-143">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on).</span></span> <span data-ttu-id="57ec2-144">Alla strategigenererare följer toohello mönster.</span><span class="sxs-lookup"><span data-stu-id="57ec2-144">All strategy writers adhere toohello pattern.</span></span> <span data-ttu-id="57ec2-145">Skicka hello strategi en `function()` som använder en token och `done` som parametrar.</span><span class="sxs-lookup"><span data-stu-id="57ec2-145">Pass hello strategy a `function()` that uses a token and `done` as parameters.</span></span> <span data-ttu-id="57ec2-146">hello strategi returneras när den gör allt arbete.</span><span class="sxs-lookup"><span data-stu-id="57ec2-146">hello strategy is returned after it does all its work.</span></span> <span data-ttu-id="57ec2-147">Lagra hello användar- och stash hello token så inte behöver du tooask för den igen.</span><span class="sxs-lookup"><span data-stu-id="57ec2-147">Store hello user and stash hello token so you don’t need tooask for it again.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="57ec2-148">hello tar föregående kod alla användare som kan autentisera tooyour server.</span><span class="sxs-lookup"><span data-stu-id="57ec2-148">hello preceding code takes any user that can authenticate tooyour server.</span></span> <span data-ttu-id="57ec2-149">Detta kallas för automatisk registrering.</span><span class="sxs-lookup"><span data-stu-id="57ec2-149">This is known as auto-registration.</span></span> <span data-ttu-id="57ec2-150">På en produktionsserver förmodligen du vill toolet vem som helst utan att behöva dem gå igenom den registreringsprocess som du väljer.</span><span class="sxs-lookup"><span data-stu-id="57ec2-150">On a production server, you wouldn’t want toolet anyone in without first having them go through a registration process that you choose.</span></span> <span data-ttu-id="57ec2-151">Detta är vanligtvis hello mönster som du ser i konsumentappar.</span><span class="sxs-lookup"><span data-stu-id="57ec2-151">This is usually hello pattern that you see in consumer apps.</span></span> <span data-ttu-id="57ec2-152">hello app kan tillåta dig tooregister med Facebook, men sedan du tillfrågas tooenter ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="57ec2-152">hello app might allow you tooregister with Facebook, but then it asks you tooenter additional information.</span></span> <span data-ttu-id="57ec2-153">Om du inte använde ett kommandoradsprogram för den här självstudiekursen, kan du extrahera hello e-post från token hello-objekt som returneras.</span><span class="sxs-lookup"><span data-stu-id="57ec2-153">If you weren't using a command-line program for this tutorial, you could extract hello email from hello token object that is returned.</span></span> <span data-ttu-id="57ec2-154">Sedan kan du be hello tooenter ytterligare användarinformation.</span><span class="sxs-lookup"><span data-stu-id="57ec2-154">Then, you might ask hello user tooenter additional information.</span></span> <span data-ttu-id="57ec2-155">Eftersom detta är en testserver kan du lägga till hello användaren direkt toohello minnesinterna databasen.</span><span class="sxs-lookup"><span data-stu-id="57ec2-155">Because this is a test server, you add hello user directly toohello in-memory database.</span></span>
  > 
  > 

4.  <span data-ttu-id="57ec2-156">Lägger till hello metoder för att du använder tookeep reda på användare som är inloggad, vilket krävs av Passport.</span><span class="sxs-lookup"><span data-stu-id="57ec2-156">Add hello methods that you use tookeep track of users who are signed in, as required by Passport.</span></span> <span data-ttu-id="57ec2-157">Detta inkluderar serialisering och avserialisering av hello användarinformation:</span><span class="sxs-lookup"><span data-stu-id="57ec2-157">This includes serializing and deserializing hello user's information:</span></span>

  ```JavaScript

  // Passport session setup (section 2)

  //   toosupport persistent login sessions, Passport needs toobe able to
  //   serialize users into, and deserialize users out of, hello session. Typically,
  //   this is as simple as storing hello user ID when serializing, and finding
  //   hello user by ID when deserializing.
  passport.serializeUser(function(user, done) {
    done(null, user.email);
  });

  passport.deserializeUser(function(id, done) {
    findByEmail(id, function (err, user) {
      done(err, user);
    });
  });

  // Array toohold signed-in users
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

5.  <span data-ttu-id="57ec2-158">Lägg till hello-kod som läser in hello Express-motorn.</span><span class="sxs-lookup"><span data-stu-id="57ec2-158">Add hello code that loads hello Express engine.</span></span> <span data-ttu-id="57ec2-159">Du använder hello standard /views och /routes mönster som Express tillhandahåller:</span><span class="sxs-lookup"><span data-stu-id="57ec2-159">You use hello default /views and /routes pattern that Express provides:</span></span>

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
    // Initialize Passport!  Also use passport.session() middleware, toosupport
    // persistent login sessions (recommended).
    app.use(passport.initialize());
    app.use(passport.session());
    app.use(app.router);
    app.use(express.static(__dirname + '/../../public'));
  });

  ```

6.  <span data-ttu-id="57ec2-160">Lägg till hello POST dirigerar att leverera hello faktiska inloggning begäranden toohello `passport-azure-ad` motorn:</span><span class="sxs-lookup"><span data-stu-id="57ec2-160">Add hello POST routes that hand off hello actual sign-in requests toohello `passport-azure-ad` engine:</span></span>

  ```JavaScript

  // Auth routes (section 3)

  // GET /auth/openid
  //   Use passport.authenticate() as route middleware tooauthenticate the
  //   request. hello first step in OpenID authentication involves redirecting
  //   hello user toohello user's OpenID provider. After authenticating, hello OpenID
  //   provider redirects hello user back toothis application at
  //   /auth/openid/return.

  app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
      log.info('Authentication was called in hello sample');
      res.redirect('/');
    });

  // GET /auth/openid/return
  //   Use passport.authenticate() as route middleware tooauthenticate the
  //   request. If authentication fails, hello user is redirected back toothe
  //   sign-in page. Otherwise, hello primary route function is called.
  //   In this example, it redirects hello user toohello home page.
  app.get('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });

  // POST /auth/openid/return
  //   Use passport.authenticate() as route middleware tooauthenticate the
  //   request. If authentication fails, hello user is redirected back toothe
  //   sign-in page. Otherwise, hello primary route function is called. 
  //   In this example, it redirects hello user toohello home page.

  app.post('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });
  ```

## <a name="4-use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a><span data-ttu-id="57ec2-161">4: Använd Passport tooissue inloggning och utloggning begär tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="57ec2-161">4: Use Passport tooissue sign-in and sign-out requests tooAzure AD</span></span>
<span data-ttu-id="57ec2-162">Appen är nu konfigurera toocommunicate med hello v2.0-slutpunkten med hjälp av autentiseringsprotokollet för hello OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="57ec2-162">Your app is now set up toocommunicate with hello v2.0 endpoint by using hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="57ec2-163">Hej `passport-azure-ad` strategi tar hand om alla hello information utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla hello användarsessioner.</span><span class="sxs-lookup"><span data-stu-id="57ec2-163">hello `passport-azure-ad` strategy takes care of all hello details of crafting authentication messages, validating tokens from Azure AD, and maintaining hello user session.</span></span> <span data-ttu-id="57ec2-164">Alla lämnas toodo är toogive användarna en sätt toosign i och logga ut och toogather mer information om hello användare är inloggad.</span><span class="sxs-lookup"><span data-stu-id="57ec2-164">All that is left toodo is toogive your users a way toosign in and sign out, and toogather more information about hello user who is signed in.</span></span>

1.  <span data-ttu-id="57ec2-165">Lägg till hello **standard**, **inloggning**, **konto**, och **logga ut** metoder tooyour App.js fil:</span><span class="sxs-lookup"><span data-stu-id="57ec2-165">Add hello **default**, **login**, **account**, and **logout** methods tooyour App.js file:</span></span>

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
      log.info('Login was called in hello sample');
      res.redirect('/');
  });

  app.get('/logout', function(req, res){
    req.logout();
    res.redirect('/');
  });

  ```

  <span data-ttu-id="57ec2-166">Här följer hello information:</span><span class="sxs-lookup"><span data-stu-id="57ec2-166">Here are hello details:</span></span>
    
    * <span data-ttu-id="57ec2-167">Hej `/` kommandot omdirigerar toohello index.ejs vyn.</span><span class="sxs-lookup"><span data-stu-id="57ec2-167">hello `/` route redirects toohello index.ejs view.</span></span> <span data-ttu-id="57ec2-168">Passerar hello användaren i hello begäran (om den finns).</span><span class="sxs-lookup"><span data-stu-id="57ec2-168">It passes hello user in hello request (if it exists).</span></span>
    * <span data-ttu-id="57ec2-169">Hej `/account` vidarebefordra först *innebär att autentiseras* (du implementera som i följande kod hello).</span><span class="sxs-lookup"><span data-stu-id="57ec2-169">hello `/account` route first *ensures that you are authenticated* (you implement that in hello following code).</span></span> <span data-ttu-id="57ec2-170">Sedan överförs den hello användaren i hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="57ec2-170">Then, it passes hello user in hello request.</span></span> <span data-ttu-id="57ec2-171">Detta är så att du kan få mer information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="57ec2-171">This is so you can get more information about hello user.</span></span>
    * <span data-ttu-id="57ec2-172">Hej `/login` kommandot anropar din `azuread-openidconnect` autentiseraren från `passport-azuread`.</span><span class="sxs-lookup"><span data-stu-id="57ec2-172">hello `/login` route calls your `azuread-openidconnect` authenticator from `passport-azuread`.</span></span> <span data-ttu-id="57ec2-173">Om som inte lyckas omdirigerar den hello användaren tillbaka för`/login`.</span><span class="sxs-lookup"><span data-stu-id="57ec2-173">If that doesn't succeed, it redirects hello user back too`/login`.</span></span>
    * <span data-ttu-id="57ec2-174">Hej `/logout` väg anropar hello logout.ejs visa (och väg).</span><span class="sxs-lookup"><span data-stu-id="57ec2-174">hello `/logout` route calls hello logout.ejs view (and route).</span></span> <span data-ttu-id="57ec2-175">Detta kommando rensar cookies och sedan returnerar hello tillbaka tooindex.ejs för användaren.</span><span class="sxs-lookup"><span data-stu-id="57ec2-175">This clears cookies, and then returns hello user back tooindex.ejs.</span></span>

2.  <span data-ttu-id="57ec2-176">Lägg till hello **EnsureAuthenticated** metod som du använde tidigare i `/account`:</span><span class="sxs-lookup"><span data-stu-id="57ec2-176">Add hello **EnsureAuthenticated** method that you used earlier in `/account`:</span></span>

  ```JavaScript

  // Route middleware tooensure hello user is authenticated (section 4)

  //   Use this route middleware on any resource that needs toobe protected. If
  //   hello request is authenticated (typically via a persistent login session),
  //   hello request proceeds. Otherwise, hello user is redirected toothe
  //   sign-in page.
  function ensureAuthenticated(req, res, next) {
    if (req.isAuthenticated()) { return next(); }
    res.redirect('/login')
  }

  ```

3.  <span data-ttu-id="57ec2-177">App.js, skapa hello server:</span><span class="sxs-lookup"><span data-stu-id="57ec2-177">In App.js, create hello server:</span></span>

  ```JavaScript

  app.listen(3000);

  ```


## <a name="5-create-hello-views-and-routes-in-express-that-you-show-your-user-on-hello-website"></a><span data-ttu-id="57ec2-178">5: skapa hello vyerna och vägarna i Express du visa din användare på hello webbplats</span><span class="sxs-lookup"><span data-stu-id="57ec2-178">5: Create hello views and routes in Express that you show your user on hello website</span></span>
<span data-ttu-id="57ec2-179">Lägg till hello vägarna och vyerna som visar information om toohello användare.</span><span class="sxs-lookup"><span data-stu-id="57ec2-179">Add hello routes and views that show information toohello user.</span></span> <span data-ttu-id="57ec2-180">hello-vägarna och vyerna även hantera hello `/logout` och `/login` vägar som du skapade.</span><span class="sxs-lookup"><span data-stu-id="57ec2-180">hello routes and views also handle hello `/logout` and `/login` routes that you created.</span></span>

1. <span data-ttu-id="57ec2-181">Skapa hello i hello rotkatalog `/routes/index.js` vägen.</span><span class="sxs-lookup"><span data-stu-id="57ec2-181">In hello root directory, create hello `/routes/index.js` route.</span></span>

  ```JavaScript

  /*
  * GET home page.
  */

  exports.index = function(req, res){
    res.render('index', { title: 'Express' });
  };
  ```

2.  <span data-ttu-id="57ec2-182">Skapa hello i hello rotkatalog `/routes/user.js` vägen.</span><span class="sxs-lookup"><span data-stu-id="57ec2-182">In hello root directory, create hello `/routes/user.js` route.</span></span>

  ```JavaScript

  /*
  * GET users listing.
  */

  exports.list = function(req, res){
    res.send("respond with a resource");
  };
  ```

  <span data-ttu-id="57ec2-183">`/routes/index.js`och `/routes/user.js` är enkla vägar skickar vidare hello begäran tooyour vyer, inklusive hello användaren, om sådan finns.</span><span class="sxs-lookup"><span data-stu-id="57ec2-183">`/routes/index.js` and `/routes/user.js` are simple routes that pass along hello request tooyour views, including hello user, if present.</span></span>

3.  <span data-ttu-id="57ec2-184">Skapa hello i hello rotkatalog `/views/index.ejs` vyn.</span><span class="sxs-lookup"><span data-stu-id="57ec2-184">In hello root directory, create hello `/views/index.ejs` view.</span></span> <span data-ttu-id="57ec2-185">Den här sidan anropar din **inloggning** och **logga ut** metoder.</span><span class="sxs-lookup"><span data-stu-id="57ec2-185">This page calls your **login** and **logout** methods.</span></span> <span data-ttu-id="57ec2-186">Du också använda hello `/views/index.ejs` visa toocapture kontoinformation.</span><span class="sxs-lookup"><span data-stu-id="57ec2-186">You also use hello `/views/index.ejs` view toocapture account information.</span></span> <span data-ttu-id="57ec2-187">Du kan använda villkorlig hello `if (!user)` som hello användare som skickas via hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="57ec2-187">You can use hello conditional `if (!user)` as hello user being passed through in hello request.</span></span> <span data-ttu-id="57ec2-188">Det finns bevis att du har en inloggad användare.</span><span class="sxs-lookup"><span data-stu-id="57ec2-188">It is evidence that you have a user signed in.</span></span>

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

4.  <span data-ttu-id="57ec2-189">Skapa hello i hello rotkatalog `/views/account.ejs` vyn.</span><span class="sxs-lookup"><span data-stu-id="57ec2-189">In hello root directory, create hello `/views/account.ejs` view.</span></span> <span data-ttu-id="57ec2-190">Hej `/views/account.ejs` vy kan du tooview ytterligare information som `passport-azuread` placerar i hello användarens begäran.</span><span class="sxs-lookup"><span data-stu-id="57ec2-190">hello `/views/account.ejs` view allows you tooview additional information that `passport-azuread` puts in hello user request.</span></span>

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

5.  <span data-ttu-id="57ec2-191">Lägg till en layout.</span><span class="sxs-lookup"><span data-stu-id="57ec2-191">Add a layout.</span></span> <span data-ttu-id="57ec2-192">Skapa hello i hello rotkatalog `/views/layout.ejs` vyn.</span><span class="sxs-lookup"><span data-stu-id="57ec2-192">In hello root directory, create hello `/views/layout.ejs` view.</span></span>

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

6.  <span data-ttu-id="57ec2-193">toobuild och kör din app kör `node app.js`.</span><span class="sxs-lookup"><span data-stu-id="57ec2-193">toobuild and run your app, run `node app.js`.</span></span> <span data-ttu-id="57ec2-194">Gå sedan för`http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="57ec2-194">Then, go too`http://localhost:3000`.</span></span>

7.  <span data-ttu-id="57ec2-195">Logga in med ett personligt microsoftkonto eller ett arbets- eller skolkonto konto.</span><span class="sxs-lookup"><span data-stu-id="57ec2-195">Sign in with either a personal Microsoft account or a work or school account.</span></span> <span data-ttu-id="57ec2-196">Observera att hello användaridentitet avspeglas i hello /account lista.</span><span class="sxs-lookup"><span data-stu-id="57ec2-196">Note that hello user's identity is reflected in hello /account list.</span></span> 

<span data-ttu-id="57ec2-197">Nu har du en webbapp som skyddas med hjälp av standardprotokollen.</span><span class="sxs-lookup"><span data-stu-id="57ec2-197">You now have a web app that is secured by using industry standard protocols.</span></span> <span data-ttu-id="57ec2-198">Du kan autentisera användare i din app genom att använda sina personliga och arbetet eller skolan konton.</span><span class="sxs-lookup"><span data-stu-id="57ec2-198">You can authenticate users in your app by using their personal and work or school accounts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57ec2-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="57ec2-199">Next steps</span></span>
<span data-ttu-id="57ec2-200">För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) har angetts som [en .zip-fil](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="57ec2-200">For reference, hello completed sample (without your configuration values) is provided as [a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip).</span></span> <span data-ttu-id="57ec2-201">Du kan också klona det från GitHub:</span><span class="sxs-lookup"><span data-stu-id="57ec2-201">You also can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="57ec2-202">Därefter kan du gå vidare toomore avancerade alternativ.</span><span class="sxs-lookup"><span data-stu-id="57ec2-202">Next, you can move on toomore advanced topics.</span></span> <span data-ttu-id="57ec2-203">Du kanske vill tootry:</span><span class="sxs-lookup"><span data-stu-id="57ec2-203">You might want tootry:</span></span>

[<span data-ttu-id="57ec2-204">Skydda ett Node.js-webb-API med hjälp av hello v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="57ec2-204">Secure a Node.js web API by using hello v2.0 endpoint</span></span>](active-directory-v2-devquickstarts-node-api.md)

<span data-ttu-id="57ec2-205">Här följer några ytterligare resurser:</span><span class="sxs-lookup"><span data-stu-id="57ec2-205">Here are some additional resources:</span></span>

* [<span data-ttu-id="57ec2-206">Utvecklarhandbok för Azure AD v2.0</span><span class="sxs-lookup"><span data-stu-id="57ec2-206">Azure AD v2.0 developer guide</span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="57ec2-207">Stacken spill ”azure-active-directory” tagg</span><span class="sxs-lookup"><span data-stu-id="57ec2-207">Stack Overflow "azure-active-directory" tag</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a><span data-ttu-id="57ec2-208">Hämta säkerhetsuppdateringar för våra produkter</span><span class="sxs-lookup"><span data-stu-id="57ec2-208">Get security updates for our products</span></span>
<span data-ttu-id="57ec2-209">Vi rekommenderar att du toosign in toobe meddelas när säkerhetsincidenter.</span><span class="sxs-lookup"><span data-stu-id="57ec2-209">We encourage you toosign up toobe notified when security incidents occur.</span></span> <span data-ttu-id="57ec2-210">På hello [Microsoft tekniska säkerhetsmeddelanden](https://technet.microsoft.com/security/dd252948) sidan, prenumerera tooSecurity rekommendationerna aviseringar.</span><span class="sxs-lookup"><span data-stu-id="57ec2-210">On hello [Microsoft Technical Security Notifications](https://technet.microsoft.com/security/dd252948) page, subscribe tooSecurity Advisories Alerts.</span></span>

