---
title: "aaaGetting igång med Azure AD-inloggning och utloggning med Node.js | Microsoft Docs"
description: "Lär dig hur toobuild ett Node.js Express MVC web app som kan integreras med Azure AD för inloggning."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 81deecec-dbe2-4e75-8bc0-cf3788645f99
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 26481899c74741743b947bd891b65ff24ffc43c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-web-app-sign-in-and-sign-out-with-azure-ad"></a><span data-ttu-id="7fae7-103">Node.js-webbapp-inloggning och utloggning med Azure AD</span><span class="sxs-lookup"><span data-stu-id="7fae7-103">Node.js web app sign-in and sign-out with Azure AD</span></span>
<span data-ttu-id="7fae7-104">Här kan vi använda Passport:</span><span class="sxs-lookup"><span data-stu-id="7fae7-104">Here we use Passport to:</span></span>

* <span data-ttu-id="7fae7-105">Logga hello användaren i toohello app med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7fae7-105">Sign hello user in toohello app with Azure Active Directory (Azure AD).</span></span>
* <span data-ttu-id="7fae7-106">Visa information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="7fae7-106">Display information about hello user.</span></span>
* <span data-ttu-id="7fae7-107">Logga hello användare utanför hello appen.</span><span class="sxs-lookup"><span data-stu-id="7fae7-107">Sign hello user out of hello app.</span></span>

<span data-ttu-id="7fae7-108">Passport är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="7fae7-108">Passport is authentication middleware for Node.js.</span></span> <span data-ttu-id="7fae7-109">Flexibel och modulära, Passport diskret kan släppas i tooany Express-baserade eller restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="7fae7-109">Flexible and modular, Passport can be unobtrusively dropped in tooany Express-based or restify web application.</span></span> <span data-ttu-id="7fae7-110">En omfattande uppsättning strategier stöder autentisering som använder ett användarnamn och lösenord, Facebook, Twitter och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="7fae7-110">A comprehensive set of strategies support authentication that uses a username and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="7fae7-111">Vi har utvecklat en strategi för Microsoft Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7fae7-111">We have developed a strategy for Microsoft Azure Active Directory.</span></span> <span data-ttu-id="7fae7-112">Vi installerar den här modulen och lägger sedan till hello Microsoft Azure Active Directory `passport-azure-ad` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="7fae7-112">We install this module and then add hello Microsoft Azure Active Directory `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="7fae7-113">toodo detta, vidta hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7fae7-113">toodo this, take hello following steps:</span></span>

1. <span data-ttu-id="7fae7-114">Registrera en app.</span><span class="sxs-lookup"><span data-stu-id="7fae7-114">Register an app.</span></span>
2. <span data-ttu-id="7fae7-115">Konfigurera din app toouse hello `passport-azure-ad` strategi.</span><span class="sxs-lookup"><span data-stu-id="7fae7-115">Set up your app toouse hello `passport-azure-ad` strategy.</span></span>
3. <span data-ttu-id="7fae7-116">Använda Passport tooissue inloggning och utloggning begäranden tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="7fae7-116">Use Passport tooissue sign-in and sign-out requests tooAzure AD.</span></span>
4. <span data-ttu-id="7fae7-117">Skriva ut data om hello användare.</span><span class="sxs-lookup"><span data-stu-id="7fae7-117">Print data about hello user.</span></span>

<span data-ttu-id="7fae7-118">hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="7fae7-118">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).</span></span>  <span data-ttu-id="7fae7-119">toofollow längs kan du [hämta hello appens stomme som en .zip-fil](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) eller klona hello stommen:</span><span class="sxs-lookup"><span data-stu-id="7fae7-119">toofollow along, you can [download hello app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="7fae7-120">programmet hello slutförts tillhandahålls hello slutet av den här kursen samt.</span><span class="sxs-lookup"><span data-stu-id="7fae7-120">hello completed application is provided at hello end of this tutorial as well.</span></span>

## <a name="step-1-register-an-app"></a><span data-ttu-id="7fae7-121">Steg 1: Registrera en app</span><span class="sxs-lookup"><span data-stu-id="7fae7-121">Step 1: Register an app</span></span>
1. <span data-ttu-id="7fae7-122">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7fae7-122">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="7fae7-123">Hello menyn hello överst på hello sidan väljer du ditt konto.</span><span class="sxs-lookup"><span data-stu-id="7fae7-123">In hello menu at hello top of hello page, select your account.</span></span> <span data-ttu-id="7fae7-124">Under hello **Directory** Välj hello Active Directory-klient där du vill att tooregister ditt program.</span><span class="sxs-lookup"><span data-stu-id="7fae7-124">Under hello **Directory** list, choose hello Active Directory tenant where you want tooregister your application.</span></span>

3. <span data-ttu-id="7fae7-125">Välj **fler tjänster** i hello-menyn på hello vänster sida av hello-skärmen och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="7fae7-125">Select **More Services** in hello menu on hello left side of hello screen, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="7fae7-126">Välj **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7fae7-126">Select **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="7fae7-127">Följ hello prompter toocreate en **webbprogram** och/eller **WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="7fae7-127">Follow hello prompts toocreate a **Web Application** and/or **WebAPI**.</span></span>
  * <span data-ttu-id="7fae7-128">Hej **namn** av hello programmet beskriver toousers ditt program.</span><span class="sxs-lookup"><span data-stu-id="7fae7-128">hello **name** of hello application describes your application toousers.</span></span>

  * <span data-ttu-id="7fae7-129">Hej **inloggnings-URL** är hello bas-URL för din app.</span><span class="sxs-lookup"><span data-stu-id="7fae7-129">hello **Sign-On URL** is hello base URL of your app.</span></span>  <span data-ttu-id="7fae7-130">hello stommens standardvärdet är ' http://localhost: 3000/auth/openid/returnera ''.</span><span class="sxs-lookup"><span data-stu-id="7fae7-130">hello skeleton's default is `http://localhost:3000/auth/openid/return`\`.</span></span>

6. <span data-ttu-id="7fae7-131">När du registrerar tilldelar Azure AD appen ett unikt-ID.</span><span class="sxs-lookup"><span data-stu-id="7fae7-131">After you register, Azure AD assigns your app a unique application ID.</span></span> <span data-ttu-id="7fae7-132">Du behöver det här värdet i hello följande avsnitt så kopiera den från hello appen på sidan.</span><span class="sxs-lookup"><span data-stu-id="7fae7-132">You need this value in hello following sections, so copy it from hello application page.</span></span>
7. <span data-ttu-id="7fae7-133">Från hello **inställningar** -> **egenskaper** för programmet, uppdatera hello App-ID-URI.</span><span class="sxs-lookup"><span data-stu-id="7fae7-133">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="7fae7-134">Hej **App-ID URI** är en unik identifierare för programmet.</span><span class="sxs-lookup"><span data-stu-id="7fae7-134">hello **App ID URI** is a unique identifier for your application.</span></span> <span data-ttu-id="7fae7-135">hello konventionen är toouse hello format `https://<tenant-domain>/<app-name>`, till exempel: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span><span class="sxs-lookup"><span data-stu-id="7fae7-135">hello convention is toouse hello format `https://<tenant-domain>/<app-name>`, for example: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span></span>

## <a name="step-2-add-prerequisites-tooyour-directory"></a><span data-ttu-id="7fae7-136">Steg 2: Lägg till krav tooyour katalog</span><span class="sxs-lookup"><span data-stu-id="7fae7-136">Step 2: Add prerequisites tooyour directory</span></span>
1. <span data-ttu-id="7fae7-137">Från kommandoraden hello, ändra kataloger tooyour rotmapp om du inte redan där, och sedan kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="7fae7-137">From hello command line, change directories tooyour root folder if you're not already there, and then run hello following commands:</span></span>

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

2. <span data-ttu-id="7fae7-138">Dessutom behöver du `passport-azure-ad`:</span><span class="sxs-lookup"><span data-stu-id="7fae7-138">In addition, you need `passport-azure-ad`:</span></span>
    * `npm install passport-azure-ad`

<span data-ttu-id="7fae7-139">Detta installerar hello bibliotek som `passport-azure-ad` beror på.</span><span class="sxs-lookup"><span data-stu-id="7fae7-139">This installs hello libraries that `passport-azure-ad` depends on.</span></span>

## <a name="step-3-set-up-your-app-toouse-hello-passport-node-js-strategy"></a><span data-ttu-id="7fae7-140">Steg 3: Ställ in din app toouse hello passport-nod-js-strategi</span><span class="sxs-lookup"><span data-stu-id="7fae7-140">Step 3: Set up your app toouse hello passport-node-js strategy</span></span>
<span data-ttu-id="7fae7-141">Här kan konfigurera vi Express toouse hello autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="7fae7-141">Here, we configure Express toouse hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="7fae7-142">Passport är används toodo olika saker, bland annat problemet inloggning och utloggning förfrågningar, hantera hello användarens session och få information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="7fae7-142">Passport is used toodo various things, including issue sign-in and sign-out requests, manage hello user's session, and get information about hello user.</span></span>

1. <span data-ttu-id="7fae7-143">toobegin, öppna hello `config.js` filen hello roten i hello-projektet och sedan ange din Apps konfigurationsvärden i hello `exports.creds` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7fae7-143">toobegin, open hello `config.js` file at hello root of hello project, and then enter your app's configuration values in hello `exports.creds` section.</span></span>

  * <span data-ttu-id="7fae7-144">Hej `clientID` är hello **program-Id** som är tilldelade tooyour app i portalen för registrering av hello.</span><span class="sxs-lookup"><span data-stu-id="7fae7-144">hello `clientID` is hello **Application Id** that's assigned tooyour app in hello registration portal.</span></span>

  * <span data-ttu-id="7fae7-145">Hej `returnURL` är hello **omdirigerings-Uri** som du angav i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="7fae7-145">hello `returnURL` is hello **Redirect Uri** that you entered in hello portal.</span></span>

  * <span data-ttu-id="7fae7-146">Hej `clientSecret` är hello hemlighet som du skapade i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="7fae7-146">hello `clientSecret` is hello secret that you generated in hello portal.</span></span>

2. <span data-ttu-id="7fae7-147">Därefter öppnar hello `app.js` filen i projektet hello hello rot.</span><span class="sxs-lookup"><span data-stu-id="7fae7-147">Next, open hello `app.js` file in hello root of hello project.</span></span> <span data-ttu-id="7fae7-148">Lägg till följande anrop tooinvoke hello hello `OIDCStrategy` strategin som medföljer `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="7fae7-148">Then add hello following call tooinvoke hello `OIDCStrategy` strategy that comes with `passport-azure-ad`.</span></span>

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. <span data-ttu-id="7fae7-149">Efter det att använda hello strategi vi bara refereras toohandle våra inloggning begäranden.</span><span class="sxs-lookup"><span data-stu-id="7fae7-149">After that, use hello strategy we just referenced toohandle our sign-in requests.</span></span>

    ```JavaScript
    // Use hello OIDCStrategy within Passport. (Section 2)
    //
    //   Strategies in passport require a `validate` function that accepts
    //   credentials (in this case, an OpenID identifier), and invokes a callback
    //   with a user object.
    passport.use(new OIDCStrategy({
        callbackURL: config.creds.returnURL,
        realm: config.creds.realm,
        clientID: config.creds.clientID,
        clientSecret: config.creds.clientSecret,
        oidcIssuer: config.creds.issuer,
        identityMetadata: config.creds.identityMetadata,
        skipUserProfile: config.creds.skipUserProfile,
        responseType: config.creds.responseType,
        responseMode: config.creds.responseMode
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
        if (!profile.email) {
        return done(new Error("No email found"), null);
        }
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
<span data-ttu-id="7fae7-150">Passport använder ett liknande mönster för alla dess strategier (Twitter, Facebook och så vidare) som alla strategiskrivare följer.</span><span class="sxs-lookup"><span data-stu-id="7fae7-150">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on) that all strategy writers adhere to.</span></span> <span data-ttu-id="7fae7-151">Granskar hello strategi kan se du att vi skickar en funktion som har en token och en klart som hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="7fae7-151">Looking at hello strategy, you see that we pass it a function that has a token and a done as hello parameters.</span></span> <span data-ttu-id="7fae7-152">hello strategin kommer tillbaka toous när den har sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="7fae7-152">hello strategy comes back toous after it does its work.</span></span> <span data-ttu-id="7fae7-153">Vi vill sedan toostore hello användar- och stash hello token så vi inte behöver tooask för den igen.</span><span class="sxs-lookup"><span data-stu-id="7fae7-153">Then we want toostore hello user and stash hello token so we don't need tooask for it again.</span></span>

> [!IMPORTANT]
<span data-ttu-id="7fae7-154">hello föregående kod tar alla användare som händer tooauthenticate tooour server.</span><span class="sxs-lookup"><span data-stu-id="7fae7-154">hello previous code takes any user that happens tooauthenticate tooour server.</span></span> <span data-ttu-id="7fae7-155">Detta kallas för automatisk registrering.</span><span class="sxs-lookup"><span data-stu-id="7fae7-155">This is known as auto-registration.</span></span> <span data-ttu-id="7fae7-156">Vi rekommenderar att du inte låta alla autentisera tooa produktionsservern utan att behöva dem registreras via en process som du väljer.</span><span class="sxs-lookup"><span data-stu-id="7fae7-156">We    recommend that you don't let anyone authenticate tooa production server without first having them register via a process that you decide on.</span></span> <span data-ttu-id="7fae7-157">Detta är vanligtvis hello mönster du ser i konsumentappar som gör att du tooregister med Facebook men sedan ber du tooprovide ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="7fae7-157">This is usually hello pattern you see in consumer apps, which allow you tooregister with Facebook but then ask you tooprovide additional information.</span></span> <span data-ttu-id="7fae7-158">Om det inte var ett exempelprogram kunnat vi extrahera hello användarens e-postadress från token hello-objekt som returneras och sedan bett hello användaren toofill i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="7fae7-158">If this weren't a sample application, we could have extracted hello user's email address from hello token object that is returned and then asked hello user toofill out additional information.</span></span> <span data-ttu-id="7fae7-159">Eftersom detta är en testserver lägger vi lägga till dem toohello minnesinterna databasen.</span><span class="sxs-lookup"><span data-stu-id="7fae7-159">Because this is a test server, we add them toohello in-memory database.</span></span>


4. <span data-ttu-id="7fae7-160">Därefter lägger vi till hello-metoder som gör oss tootrack hello inloggade användare som krävs av Passport.</span><span class="sxs-lookup"><span data-stu-id="7fae7-160">Next, let's add hello methods that enable us tootrack hello signed-in users as required by Passport.</span></span> <span data-ttu-id="7fae7-161">De här metoderna omfattar serialisering och avserialisering av hello användarinformation.</span><span class="sxs-lookup"><span data-stu-id="7fae7-161">These methods include serializing and deserializing hello user's information.</span></span>

    ```JavaScript

            // Passport session setup. (Section 2)

            //   toosupport persistent sign-in sessions, Passport needs toobe able to
            //   serialize users into hello session and deserialize them out of hello session. Typically,
            //   this is done simply by storing hello user ID when serializing and finding
            //   hello user by ID when deserializing.
            passport.serializeUser(function(user, done) {
            done(null, user.email);
            });

            passport.deserializeUser(function(id, done) {
            findByEmail(id, function (err, user) {
                done(err, user);
            });
            });

            // array toohold signed-in users
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

5.  <span data-ttu-id="7fae7-162">Därefter lägger vi till hello kod tooload hello Express-motorn.</span><span class="sxs-lookup"><span data-stu-id="7fae7-162">Next, let's add hello code tooload hello Express engine.</span></span> <span data-ttu-id="7fae7-163">Använder här vi hello standard /views och /routes mönster som Express tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="7fae7-163">Here we use hello default /views and /routes pattern that Express provides.</span></span>

    ```JavaScript

        // configure Express (section 2)

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

6. <span data-ttu-id="7fae7-164">Slutligen ska vi lägga till hello dirigerar att leverera hello faktiska inloggning begäranden toohello `passport-azure-ad` motorn:</span><span class="sxs-lookup"><span data-stu-id="7fae7-164">Finally, let's add hello routes that hand off hello actual sign-in requests toohello `passport-azure-ad` engine:</span></span>


       ```JavaScript

        // Our Auth routes (section 3)

        // GET /auth/openid
        //   Use passport.authenticate() as route middleware tooauthenticate the
        //   request. hello first step in OpenID authentication involves redirecting
        //   hello user tootheir OpenID provider. After authenticating, hello OpenID
        //   provider redirects hello user back toothis application at
        //   /auth/openid/return.
        app.get('/auth/openid',
        passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
        function(req, res) {
            log.info('Authentication was called in hello Sample');
            res.redirect('/');
        });

            // GET /auth/openid/return
            //   Use passport.authenticate() as route middleware tooauthenticate the
            //   request. If authentication fails, hello user is redirected back toothe
            //   sign-in page. Otherwise, hello primary route function is called,
            //   which, in this example, redirects hello user toohello home page.
            app.get('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });

            // POST /auth/openid/return
            //   Use passport.authenticate() as route middleware tooauthenticate the
            //   request. If authentication fails, hello user is redirected back toothe
            //   sign-in page. Otherwise, hello primary route function is called,
            //   which, in this example, redirects hello user toohello home page.
            app.post('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });
       ```


## <a name="step-4-use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a><span data-ttu-id="7fae7-165">Steg 4: Använd Passport tooissue-inloggning och utloggning begäranden tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="7fae7-165">Step 4: Use Passport tooissue sign-in and sign-out requests tooAzure AD</span></span>
<span data-ttu-id="7fae7-166">Appen är nu korrekt konfigurerade toocommunicate med hello slutpunkt med hjälp av autentiseringsprotokollet för hello OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="7fae7-166">Your app is now properly configured toocommunicate with hello endpoint by using hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="7fae7-167">`passport-azure-ad`har tagit hand om alla hello information utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla användarsessioner.</span><span class="sxs-lookup"><span data-stu-id="7fae7-167">`passport-azure-ad` has taken care of all hello details of crafting authentication messages, validating tokens from Azure AD, and maintaining user sessions.</span></span> <span data-ttu-id="7fae7-168">Allt som fortfarande ge användarna ett sätt toosign i och logga ut och samla in ytterligare information om hello inloggade användare.</span><span class="sxs-lookup"><span data-stu-id="7fae7-168">All that remains is giving your users a way toosign in and sign out, and gathering additional information about hello signed-in users.</span></span>

1. <span data-ttu-id="7fae7-169">Först ska vi lägga till hello standard, logga in, konto och utloggningsmetoderna tooour `app.js` fil:</span><span class="sxs-lookup"><span data-stu-id="7fae7-169">First, let's add hello default, sign-in, account, and sign-out methods tooour `app.js` file:</span></span>

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
            log.info('Login was called in hello Sample');
            res.redirect('/');
        });

        app.get('/logout', function(req, res){
          req.logout();
          res.redirect('/');
        });

    ```

2.  <span data-ttu-id="7fae7-170">Nu ska vi se dessa i detalj:</span><span class="sxs-lookup"><span data-stu-id="7fae7-170">Let's review these in detail:</span></span>

  * <span data-ttu-id="7fae7-171">Hej `/`kommandot omdirigerar toohello index.ejs vyn, skicka hello användaren i hello begäran (om den finns).</span><span class="sxs-lookup"><span data-stu-id="7fae7-171">hello `/`route redirects toohello index.ejs view, passing hello user in hello request (if it exists).</span></span>
  * <span data-ttu-id="7fae7-172">Hej `/account` vidarebefordra först *garanterar vi autentiseras* (vi implementera som i följande exempel hello), och sedan överför hello användare i hello begäran så att vi kan få ytterligare information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="7fae7-172">hello `/account` route first *ensures we are authenticated* (we implement that in hello following example), and then passes hello user in hello request so that we can get additional information about hello user.</span></span>
  * <span data-ttu-id="7fae7-173">Hej `/login` väg anropar våra azuread openidconnect autentiseraren från `passport-azuread`.</span><span class="sxs-lookup"><span data-stu-id="7fae7-173">hello `/login` route calls our azuread-openidconnect authenticator from `passport-azuread`.</span></span> <span data-ttu-id="7fae7-174">Om som inte lyckas omdirigerar hello tillbaka för/användarinloggning.</span><span class="sxs-lookup"><span data-stu-id="7fae7-174">If that doesn't succeed, it redirects hello user back too/login.</span></span>
  * <span data-ttu-id="7fae7-175">Hej `/logout` väg anropar bara hello logout.ejs (och väg), vilket raderar cookies och returnerar hello användaren tillbaka tooindex.ejs.</span><span class="sxs-lookup"><span data-stu-id="7fae7-175">hello `/logout` route simply calls hello logout.ejs (and route), which clears cookies and then returns hello user back tooindex.ejs.</span></span>

3. <span data-ttu-id="7fae7-176">För hello sista delen av `app.js`, Lägg till hello **EnsureAuthenticated** metod som används i `/account`, enligt tidigare.</span><span class="sxs-lookup"><span data-stu-id="7fae7-176">For hello last part of `app.js`, let's add hello **EnsureAuthenticated** method that is used in `/account`, as shown earlier.</span></span>

    ```JavaScript

        // Simple route middleware tooensure user is authenticated. (section 4)

        //   Use this route middleware on any resource that needs toobe protected. If
        //   hello request is authenticated (typically via a persistent sign-in session),
        //   hello request proceeds. Otherwise, hello user is redirected toothe
        //   sign-in page.
        function ensureAuthenticated(req, res, next) {
          if (req.isAuthenticated()) { return next(); }
          res.redirect('/login')
        }
    ```

4. <span data-ttu-id="7fae7-177">Nu ska vi skapa slutligen hello-servern i `app.js`:</span><span class="sxs-lookup"><span data-stu-id="7fae7-177">Finally, let's create hello server itself in `app.js`:</span></span>

```JavaScript

        app.listen(3000);

```


## <a name="step-5-toodisplay-our-user-in-hello-website-create-hello-views-and-routes-in-express"></a><span data-ttu-id="7fae7-178">Steg 5: toodisplay våra användare på hello webbplats, skapa hello vyerna och vägarna i Express</span><span class="sxs-lookup"><span data-stu-id="7fae7-178">Step 5: toodisplay our user in hello website, create hello views and routes in Express</span></span>
<span data-ttu-id="7fae7-179">Nu `app.js` är klar.</span><span class="sxs-lookup"><span data-stu-id="7fae7-179">Now `app.js` is complete.</span></span> <span data-ttu-id="7fae7-180">Vi bara behöver tooadd hello vägar och vyer som visar hello information vi hämta toohello användare, samt hantera hello `/logout` och `/login` vägar som vi skapade.</span><span class="sxs-lookup"><span data-stu-id="7fae7-180">We simply need tooadd hello routes and views that show hello information we get toohello user, as well as handle hello `/logout` and `/login` routes that we  created.</span></span>

1. <span data-ttu-id="7fae7-181">Skapa hello `/routes/index.js` vägen under rotkatalogen hello.</span><span class="sxs-lookup"><span data-stu-id="7fae7-181">Create hello `/routes/index.js` route under hello root directory.</span></span>

    ```JavaScript
                /*
                 * GET home page.
                 */

                exports.index = function(req, res){
                  res.render('index', { title: 'Express' });
                };
    ```

2. <span data-ttu-id="7fae7-182">Skapa hello `/routes/user.js` vägen under rotkatalogen hello.</span><span class="sxs-lookup"><span data-stu-id="7fae7-182">Create hello `/routes/user.js` route under hello root directory.</span></span>

                ```JavaScript
                /*
                 * GET users listing.
                 */

                exports.list = function(req, res){
                  res.send("respond with a resource");
                };
                ```

 <span data-ttu-id="7fae7-183">Dessa skickar vidare hello begäran tooour vyer, inklusive hello användare om den finns.</span><span class="sxs-lookup"><span data-stu-id="7fae7-183">These pass along hello request tooour views, including hello user if present.</span></span>

3. <span data-ttu-id="7fae7-184">Skapa hello `/views/index.ejs` vyn under rotkatalogen hello.</span><span class="sxs-lookup"><span data-stu-id="7fae7-184">Create hello `/views/index.ejs` view under hello root directory.</span></span> <span data-ttu-id="7fae7-185">Detta är en enkel sida som anropar våra logga in och logga ut metoder och gör att vi toograb kontoinformation.</span><span class="sxs-lookup"><span data-stu-id="7fae7-185">This is a simple page that calls our login and logout methods and enables us toograb account information.</span></span> <span data-ttu-id="7fae7-186">Observera att vi kan använda hello villkorlig `if (!user)` eftersom hello användaren hello begäran som skickas via bevis som vi har en inloggad användare.</span><span class="sxs-lookup"><span data-stu-id="7fae7-186">Notice that we can use hello conditional `if (!user)` as hello user being passed through in hello request is evidence we have a signed-in user.</span></span>

    ```JavaScript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
        <h2>Hello, <%= user.displayName %>.</h2>
        <a href="/account">Account Info</a></br>
        <a href="/logout">Log Out</a>
    <% } %>
    ```

4. <span data-ttu-id="7fae7-187">Skapa hello `/views/account.ejs` vyn under rotkatalogen hello så att vi kan visa ytterligare information som `passport-azuread` har satt i hello användarens begäran.</span><span class="sxs-lookup"><span data-stu-id="7fae7-187">Create hello `/views/account.ejs` view under hello root directory so that we can view additional information that `passport-azuread` has put in hello user request.</span></span>

    ```Javascript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
    <p>displayName: <%= user.displayName %></p>
    <p>givenName: <%= user.name.givenName %></p>
    <p>familyName: <%= user.name.familyName %></p>
    <p>UPN: <%= user._json.upn %></p>
    <p>Profile ID: <%= user.id %></p>
  ##Next steps  <p>Full Claimes</p>
    <%- JSON.stringify(user) %>
    <p></p>
    <a href="/logout">Log Out</a>
    <% } %>
    ```

5. <span data-ttu-id="7fae7-188">Vi behöver kontrollera sökningen bra genom att lägga till en layout.</span><span class="sxs-lookup"><span data-stu-id="7fae7-188">Let's make this look good by adding a layout.</span></span> <span data-ttu-id="7fae7-189">Skapa hello ' views/layout.ejs'-vyn under hello rotkatalogen /.</span><span class="sxs-lookup"><span data-stu-id="7fae7-189">Create hello '/views/layout.ejs' view under hello root directory.</span></span>

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
                <a href="/login">Log In</a>
                </p>
            <% } else { %>
                <p>
                <a href="/">Home</a> |
                <a href="/account">Account</a> |
                <a href="/logout">Log Out</a>
                </p>
            <% } %>
            <%- body %>
        </body>
    </html>
    ```

##<a name="next-steps"></a><span data-ttu-id="7fae7-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7fae7-190">Next steps</span></span>
<span data-ttu-id="7fae7-191">Slutligen skapar och kör din app.</span><span class="sxs-lookup"><span data-stu-id="7fae7-191">Finally, build and run your app.</span></span> <span data-ttu-id="7fae7-192">Kör `node app.js`, och sedan gå för`http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="7fae7-192">Run `node app.js`, and then go too`http://localhost:3000`.</span></span>

<span data-ttu-id="7fae7-193">Logga in med ett personligt microsoftkonto eller ett konto för arbetet eller skolan och Observera hur hello användaridentitet avspeglas i hello /account lista.</span><span class="sxs-lookup"><span data-stu-id="7fae7-193">Sign in with either a personal Microsoft account or a work or school account, and notice how hello user's identity is reflected in hello /account list.</span></span> <span data-ttu-id="7fae7-194">Nu har du en webbapp som skyddas med standardprotokollen som kan autentisera användare med både sina personliga och arbete/skola konton.</span><span class="sxs-lookup"><span data-stu-id="7fae7-194">You now have a web app that's secured with industry standard protocols that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="7fae7-195">För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [har angetts som en .zip-fil](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="7fae7-195">For reference, hello completed sample (without your configuration values) [is provided as a .zip file](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span></span> <span data-ttu-id="7fae7-196">Du kan också klona det från GitHub:</span><span class="sxs-lookup"><span data-stu-id="7fae7-196">Alternatively, you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="7fae7-197">Du kan nu gå vidare till mer avancerade avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7fae7-197">You can now move onto more advanced topics.</span></span> <span data-ttu-id="7fae7-198">Du kanske vill tootry:</span><span class="sxs-lookup"><span data-stu-id="7fae7-198">You might want tootry:</span></span>

[<span data-ttu-id="7fae7-199">Skydda ett webb-API med Azure AD</span><span class="sxs-lookup"><span data-stu-id="7fae7-199">Secure a Web API with Azure AD</span></span>](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
