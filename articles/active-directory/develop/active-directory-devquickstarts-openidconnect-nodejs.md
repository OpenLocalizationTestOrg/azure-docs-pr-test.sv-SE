---
title: "Komma igång med Azure AD-inloggning och utloggning med Node.js | Microsoft Docs"
description: "Lär dig hur du skapar en Node.js Express MVC-webbapp som kan integreras med Azure AD för inloggning."
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
ms.openlocfilehash: 13317b016f9ff3955f376b858645c42668b0de42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="nodejs-web-app-sign-in-and-sign-out-with-azure-ad"></a><span data-ttu-id="5d488-103">Node.js-webbapp-inloggning och utloggning med Azure AD</span><span class="sxs-lookup"><span data-stu-id="5d488-103">Node.js web app sign-in and sign-out with Azure AD</span></span>
<span data-ttu-id="5d488-104">Här kan vi använda Passport:</span><span class="sxs-lookup"><span data-stu-id="5d488-104">Here we use Passport to:</span></span>

* <span data-ttu-id="5d488-105">Logga in användaren i appen med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5d488-105">Sign the user in to the app with Azure Active Directory (Azure AD).</span></span>
* <span data-ttu-id="5d488-106">Visa information om användaren.</span><span class="sxs-lookup"><span data-stu-id="5d488-106">Display information about the user.</span></span>
* <span data-ttu-id="5d488-107">Logga ut från appen användaren.</span><span class="sxs-lookup"><span data-stu-id="5d488-107">Sign the user out of the app.</span></span>

<span data-ttu-id="5d488-108">Passport är ett mellanprogram för autentisering för Node.js.</span><span class="sxs-lookup"><span data-stu-id="5d488-108">Passport is authentication middleware for Node.js.</span></span> <span data-ttu-id="5d488-109">Flexibel och modulära, Passport diskret kan släppas till alla Express-baserade eller restify-webbappar.</span><span class="sxs-lookup"><span data-stu-id="5d488-109">Flexible and modular, Passport can be unobtrusively dropped in to any Express-based or restify web application.</span></span> <span data-ttu-id="5d488-110">En omfattande uppsättning strategier stöder autentisering som använder ett användarnamn och lösenord, Facebook, Twitter och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="5d488-110">A comprehensive set of strategies support authentication that uses a username and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="5d488-111">Vi har utvecklat en strategi för Microsoft Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5d488-111">We have developed a strategy for Microsoft Azure Active Directory.</span></span> <span data-ttu-id="5d488-112">Vi installerar den här modulen och lägger sedan till Microsoft Azure Active Directory `passport-azure-ad` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="5d488-112">We install this module and then add the Microsoft Azure Active Directory `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="5d488-113">Om du vill göra det, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="5d488-113">To do this, take the following steps:</span></span>

1. <span data-ttu-id="5d488-114">Registrera en app.</span><span class="sxs-lookup"><span data-stu-id="5d488-114">Register an app.</span></span>
2. <span data-ttu-id="5d488-115">Konfigurera din app att använda den `passport-azure-ad` strategi.</span><span class="sxs-lookup"><span data-stu-id="5d488-115">Set up your app to use the `passport-azure-ad` strategy.</span></span>
3. <span data-ttu-id="5d488-116">Använda Passport för att utfärda inloggnings- och utloggningsförfrågningar till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5d488-116">Use Passport to issue sign-in and sign-out requests to Azure AD.</span></span>
4. <span data-ttu-id="5d488-117">Skriva ut data om användaren.</span><span class="sxs-lookup"><span data-stu-id="5d488-117">Print data about the user.</span></span>

<span data-ttu-id="5d488-118">Koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="5d488-118">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).</span></span>  <span data-ttu-id="5d488-119">Om du vill följa med kan du [ladda ned appens stomme som en .zip-fil](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) eller klona stommen:</span><span class="sxs-lookup"><span data-stu-id="5d488-119">To follow along, you can [download the app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="5d488-120">Det färdiga programmet har angetts i slutet av den här kursen samt.</span><span class="sxs-lookup"><span data-stu-id="5d488-120">The completed application is provided at the end of this tutorial as well.</span></span>

## <a name="step-1-register-an-app"></a><span data-ttu-id="5d488-121">Steg 1: Registrera en app</span><span class="sxs-lookup"><span data-stu-id="5d488-121">Step 1: Register an app</span></span>
1. <span data-ttu-id="5d488-122">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5d488-122">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="5d488-123">I menyn överst på sidan, Välj ditt konto.</span><span class="sxs-lookup"><span data-stu-id="5d488-123">In the menu at the top of the page, select your account.</span></span> <span data-ttu-id="5d488-124">Under den **Directory** Välj Active Directory-klient som du vill registrera ditt program.</span><span class="sxs-lookup"><span data-stu-id="5d488-124">Under the **Directory** list, choose the Active Directory tenant where you want to register your application.</span></span>

3. <span data-ttu-id="5d488-125">Välj **fler tjänster** på menyn till vänster på skärmen och väljer sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5d488-125">Select **More Services** in the menu on the left side of the screen, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="5d488-126">Välj **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5d488-126">Select **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="5d488-127">Följ anvisningarna för att skapa en **webbprogram** och/eller **WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="5d488-127">Follow the prompts to create a **Web Application** and/or **WebAPI**.</span></span>
  * <span data-ttu-id="5d488-128">Den **namn** beskriver ditt program till användare av programmet.</span><span class="sxs-lookup"><span data-stu-id="5d488-128">The **name** of the application describes your application to users.</span></span>

  * <span data-ttu-id="5d488-129">Den **inloggnings-URL** är den grundläggande Webbadressen för din app.</span><span class="sxs-lookup"><span data-stu-id="5d488-129">The **Sign-On URL** is the base URL of your app.</span></span>  <span data-ttu-id="5d488-130">Den stommen standardvärdet är ' http://localhost: 3000/auth/openid/returnera ''.</span><span class="sxs-lookup"><span data-stu-id="5d488-130">The skeleton's default is `http://localhost:3000/auth/openid/return`\`.</span></span>

6. <span data-ttu-id="5d488-131">När du registrerar tilldelar Azure AD appen ett unikt-ID.</span><span class="sxs-lookup"><span data-stu-id="5d488-131">After you register, Azure AD assigns your app a unique application ID.</span></span> <span data-ttu-id="5d488-132">Du behöver det här värdet i de följande avsnitten så kopiera den från appen på sidan.</span><span class="sxs-lookup"><span data-stu-id="5d488-132">You need this value in the following sections, so copy it from the application page.</span></span>
7. <span data-ttu-id="5d488-133">Från den **inställningar** -> **egenskaper** för programmet, uppdatera App-ID-URI.</span><span class="sxs-lookup"><span data-stu-id="5d488-133">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="5d488-134">Den **App-ID URI** är en unik identifierare för programmet.</span><span class="sxs-lookup"><span data-stu-id="5d488-134">The **App ID URI** is a unique identifier for your application.</span></span> <span data-ttu-id="5d488-135">Konventionen är att använda formatet `https://<tenant-domain>/<app-name>`, till exempel: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span><span class="sxs-lookup"><span data-stu-id="5d488-135">The convention is to use the format `https://<tenant-domain>/<app-name>`, for example: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span></span>

## <a name="step-2-add-prerequisites-to-your-directory"></a><span data-ttu-id="5d488-136">Steg 2: Lägg till förutsättningar för din katalog</span><span class="sxs-lookup"><span data-stu-id="5d488-136">Step 2: Add prerequisites to your directory</span></span>
1. <span data-ttu-id="5d488-137">Från kommandoraden ändrar du katalog till din rotmapp om du inte redan där, och kör sedan följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="5d488-137">From the command line, change directories to your root folder if you're not already there, and then run the following commands:</span></span>

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

2. <span data-ttu-id="5d488-138">Dessutom behöver du `passport-azure-ad`:</span><span class="sxs-lookup"><span data-stu-id="5d488-138">In addition, you need `passport-azure-ad`:</span></span>
    * `npm install passport-azure-ad`

<span data-ttu-id="5d488-139">Detta installerar biblioteken som `passport-azure-ad` beror på.</span><span class="sxs-lookup"><span data-stu-id="5d488-139">This installs the libraries that `passport-azure-ad` depends on.</span></span>

## <a name="step-3-set-up-your-app-to-use-the-passport-node-js-strategy"></a><span data-ttu-id="5d488-140">Steg 3: Konfigurera din app att använda passport-nod-js-strategi</span><span class="sxs-lookup"><span data-stu-id="5d488-140">Step 3: Set up your app to use the passport-node-js strategy</span></span>
<span data-ttu-id="5d488-141">Här kan konfigurera vi Express för att använda autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="5d488-141">Here, we configure Express to use the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="5d488-142">Passport används för att göra olika saker, bland annat problemet inloggning och utloggning förfrågningar, hantera användarens session och få information om användaren.</span><span class="sxs-lookup"><span data-stu-id="5d488-142">Passport is used to do various things, including issue sign-in and sign-out requests, manage the user's session, and get information about the user.</span></span>

1. <span data-ttu-id="5d488-143">Öppna först den `config.js` filen i roten av projektet och sedan ange din Apps konfigurationsvärden i den `exports.creds` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5d488-143">To begin, open the `config.js` file at the root of the project, and then enter your app's configuration values in the `exports.creds` section.</span></span>

  * <span data-ttu-id="5d488-144">Den `clientID` är den **program-Id** som har tilldelats din app i portalen för registrering.</span><span class="sxs-lookup"><span data-stu-id="5d488-144">The `clientID` is the **Application Id** that's assigned to your app in the registration portal.</span></span>

  * <span data-ttu-id="5d488-145">Den `returnURL` är den **omdirigerings-Uri** som du angav på portalen.</span><span class="sxs-lookup"><span data-stu-id="5d488-145">The `returnURL` is the **Redirect Uri** that you entered in the portal.</span></span>

  * <span data-ttu-id="5d488-146">Den `clientSecret` är den hemlighet som du genererade på portalen.</span><span class="sxs-lookup"><span data-stu-id="5d488-146">The `clientSecret` is the secret that you generated in the portal.</span></span>

2. <span data-ttu-id="5d488-147">Därefter öppnar den `app.js` filen i roten av projektet.</span><span class="sxs-lookup"><span data-stu-id="5d488-147">Next, open the `app.js` file in the root of the project.</span></span> <span data-ttu-id="5d488-148">Lägg till följande anrop för att anropa den `OIDCStrategy` strategin som medföljer `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="5d488-148">Then add the following call to invoke the `OIDCStrategy` strategy that comes with `passport-azure-ad`.</span></span>

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. <span data-ttu-id="5d488-149">Efter det att använda strategin vi precis refererade till för att hantera vår inloggning begäranden.</span><span class="sxs-lookup"><span data-stu-id="5d488-149">After that, use the strategy we just referenced to handle our sign-in requests.</span></span>

    ```JavaScript
    // Use the OIDCStrategy within Passport. (Section 2)
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
<span data-ttu-id="5d488-150">Passport använder ett liknande mönster för alla dess strategier (Twitter, Facebook och så vidare) som alla strategiskrivare följer.</span><span class="sxs-lookup"><span data-stu-id="5d488-150">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on) that all strategy writers adhere to.</span></span> <span data-ttu-id="5d488-151">Tittar på strategin ser du att vi skickar en funktion som har en token och klart som parametrar.</span><span class="sxs-lookup"><span data-stu-id="5d488-151">Looking at the strategy, you see that we pass it a function that has a token and a done as the parameters.</span></span> <span data-ttu-id="5d488-152">Strategin kommer tillbaka till oss när den har sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="5d488-152">The strategy comes back to us after it does its work.</span></span> <span data-ttu-id="5d488-153">Sedan vill vi lagra användaren och token så att vi inte behöver fråga efter den igen.</span><span class="sxs-lookup"><span data-stu-id="5d488-153">Then we want to store the user and stash the token so we don't need to ask for it again.</span></span>

> [!IMPORTANT]
<span data-ttu-id="5d488-154">Föregående kod tar alla användare som inträffar att autentisera till vår server.</span><span class="sxs-lookup"><span data-stu-id="5d488-154">The previous code takes any user that happens to authenticate to our server.</span></span> <span data-ttu-id="5d488-155">Detta kallas för automatisk registrering.</span><span class="sxs-lookup"><span data-stu-id="5d488-155">This is known as auto-registration.</span></span> <span data-ttu-id="5d488-156">Vi rekommenderar att du inte låta alla autentisera till en produktionsserver utan att behöva dem registreras via en process som du väljer.</span><span class="sxs-lookup"><span data-stu-id="5d488-156">We    recommend that you don't let anyone authenticate to a production server without first having them register via a process that you decide on.</span></span> <span data-ttu-id="5d488-157">Detta är oftast det mönster du ser i konsumentappar, så att du kan registrera med Facebook men be dig att ange ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="5d488-157">This is usually the pattern you see in consumer apps, which allow you to register with Facebook but then ask you to provide additional information.</span></span> <span data-ttu-id="5d488-158">Om det inte var ett exempelprogram kunnat vi extrahera användarens e-postadress från tokenobjektet som returneras och sedan bett användaren att fylla i ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="5d488-158">If this weren't a sample application, we could have extracted the user's email address from the token object that is returned and then asked the user to fill out additional information.</span></span> <span data-ttu-id="5d488-159">Eftersom detta är en testserver vi lägga till dem i InMemory-databasen.</span><span class="sxs-lookup"><span data-stu-id="5d488-159">Because this is a test server, we add them to the in-memory database.</span></span>


4. <span data-ttu-id="5d488-160">Nu ska vi lägga till de metoder som gör det möjligt för oss att spåra inloggade användare som krävs av Passport.</span><span class="sxs-lookup"><span data-stu-id="5d488-160">Next, let's add the methods that enable us to track the signed-in users as required by Passport.</span></span> <span data-ttu-id="5d488-161">De här metoderna omfattar serialisering och avserialisering av användarens information.</span><span class="sxs-lookup"><span data-stu-id="5d488-161">These methods include serializing and deserializing the user's information.</span></span>

    ```JavaScript

            // Passport session setup. (Section 2)

            //   To support persistent sign-in sessions, Passport needs to be able to
            //   serialize users into the session and deserialize them out of the session. Typically,
            //   this is done simply by storing the user ID when serializing and finding
            //   the user by ID when deserializing.
            passport.serializeUser(function(user, done) {
            done(null, user.email);
            });

            passport.deserializeUser(function(id, done) {
            findByEmail(id, function (err, user) {
                done(err, user);
            });
            });

            // array to hold signed-in users
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

5.  <span data-ttu-id="5d488-162">Nu ska vi lägga till kod för att läsa in Express-motorn.</span><span class="sxs-lookup"><span data-stu-id="5d488-162">Next, let's add the code to load the Express engine.</span></span> <span data-ttu-id="5d488-163">Vi använder här standard /views /routes mönster som Express tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="5d488-163">Here we use the default /views and /routes pattern that Express provides.</span></span>

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
          // Initialize Passport!  Also use passport.session() middleware, to support
          // persistent login sessions (recommended).
          app.use(passport.initialize());
          app.use(passport.session());
          app.use(app.router);
          app.use(express.static(__dirname + '/../../public'));
        });

    ```

6. <span data-ttu-id="5d488-164">Slutligen ska vi lägga till dirigeringskommandona som lämnar de faktiska inloggning förfrågningar till den `passport-azure-ad` motorn:</span><span class="sxs-lookup"><span data-stu-id="5d488-164">Finally, let's add the routes that hand off the actual sign-in requests to the `passport-azure-ad` engine:</span></span>


       ```JavaScript

        // Our Auth routes (section 3)

        // GET /auth/openid
        //   Use passport.authenticate() as route middleware to authenticate the
        //   request. The first step in OpenID authentication involves redirecting
        //   the user to their OpenID provider. After authenticating, the OpenID
        //   provider redirects the user back to this application at
        //   /auth/openid/return.
        app.get('/auth/openid',
        passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
        function(req, res) {
            log.info('Authentication was called in the Sample');
            res.redirect('/');
        });

            // GET /auth/openid/return
            //   Use passport.authenticate() as route middleware to authenticate the
            //   request. If authentication fails, the user is redirected back to the
            //   sign-in page. Otherwise, the primary route function is called,
            //   which, in this example, redirects the user to the home page.
            app.get('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });

            // POST /auth/openid/return
            //   Use passport.authenticate() as route middleware to authenticate the
            //   request. If authentication fails, the user is redirected back to the
            //   sign-in page. Otherwise, the primary route function is called,
            //   which, in this example, redirects the user to the home page.
            app.post('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });
       ```


## <a name="step-4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a><span data-ttu-id="5d488-165">Steg 4: Använda Passport för att utfärda inloggnings- och utloggningsförfrågningar till Azure AD</span><span class="sxs-lookup"><span data-stu-id="5d488-165">Step 4: Use Passport to issue sign-in and sign-out requests to Azure AD</span></span>
<span data-ttu-id="5d488-166">Appen har nu konfigurerats korrekt för att kommunicera med slutpunkten med hjälp av autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="5d488-166">Your app is now properly configured to communicate with the endpoint by using the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="5d488-167">`passport-azure-ad`har tagit hand om alla detaljer om utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla användarsessioner.</span><span class="sxs-lookup"><span data-stu-id="5d488-167">`passport-azure-ad` has taken care of all the details of crafting authentication messages, validating tokens from Azure AD, and maintaining user sessions.</span></span> <span data-ttu-id="5d488-168">Allt som återstår är ge användarna ett sätt att logga in och logga ut och samla in ytterligare information om de inloggade användarna.</span><span class="sxs-lookup"><span data-stu-id="5d488-168">All that remains is giving your users a way to sign in and sign out, and gathering additional information about the signed-in users.</span></span>

1. <span data-ttu-id="5d488-169">Först ska vi lägga till den standard, logga in, konto och utloggningsmetoderna i vår `app.js` fil:</span><span class="sxs-lookup"><span data-stu-id="5d488-169">First, let's add the default, sign-in, account, and sign-out methods to our `app.js` file:</span></span>

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
            log.info('Login was called in the Sample');
            res.redirect('/');
        });

        app.get('/logout', function(req, res){
          req.logout();
          res.redirect('/');
        });

    ```

2.  <span data-ttu-id="5d488-170">Nu ska vi se dessa i detalj:</span><span class="sxs-lookup"><span data-stu-id="5d488-170">Let's review these in detail:</span></span>

  * <span data-ttu-id="5d488-171">Den `/`kommandot omdirigerar till vyn index.ejs skicka användaren i begäran (om den finns).</span><span class="sxs-lookup"><span data-stu-id="5d488-171">The `/`route redirects to the index.ejs view, passing the user in the request (if it exists).</span></span>
  * <span data-ttu-id="5d488-172">Den `/account` vidarebefordra först *garanterar vi autentiseras* (vi implementera som i följande exempel), och därefter skickas användaren i begäran så att vi kan få ytterligare information om användaren.</span><span class="sxs-lookup"><span data-stu-id="5d488-172">The `/account` route first *ensures we are authenticated* (we implement that in the following example), and then passes the user in the request so that we can get additional information about the user.</span></span>
  * <span data-ttu-id="5d488-173">Den `/login` väg anropar våra azuread openidconnect autentiseraren från `passport-azuread`.</span><span class="sxs-lookup"><span data-stu-id="5d488-173">The `/login` route calls our azuread-openidconnect authenticator from `passport-azuread`.</span></span> <span data-ttu-id="5d488-174">Om som inte lyckas omdirigeras användaren till /login.</span><span class="sxs-lookup"><span data-stu-id="5d488-174">If that doesn't succeed, it redirects the user back to /login.</span></span>
  * <span data-ttu-id="5d488-175">Den `/logout` kommandot bara anropar logout.ejs (och väg) rensar cookies och returnerar sedan användaren till index.ejs.</span><span class="sxs-lookup"><span data-stu-id="5d488-175">The `/logout` route simply calls the logout.ejs (and route), which clears cookies and then returns the user back to index.ejs.</span></span>

3. <span data-ttu-id="5d488-176">För den sista delen av `app.js`, ska vi lägga till den **EnsureAuthenticated** metod som används i `/account`, enligt tidigare.</span><span class="sxs-lookup"><span data-stu-id="5d488-176">For the last part of `app.js`, let's add the **EnsureAuthenticated** method that is used in `/account`, as shown earlier.</span></span>

    ```JavaScript

        // Simple route middleware to ensure user is authenticated. (section 4)

        //   Use this route middleware on any resource that needs to be protected. If
        //   the request is authenticated (typically via a persistent sign-in session),
        //   the request proceeds. Otherwise, the user is redirected to the
        //   sign-in page.
        function ensureAuthenticated(req, res, next) {
          if (req.isAuthenticated()) { return next(); }
          res.redirect('/login')
        }
    ```

4. <span data-ttu-id="5d488-177">Slutligen ska vi skapar du själva servern i `app.js`:</span><span class="sxs-lookup"><span data-stu-id="5d488-177">Finally, let's create the server itself in `app.js`:</span></span>

```JavaScript

        app.listen(3000);

```


## <a name="step-5-to-display-our-user-in-the-website-create-the-views-and-routes-in-express"></a><span data-ttu-id="5d488-178">Steg 5: Om du vill visa våra användare på webbplatsen, skapa vyerna och vägarna i Express</span><span class="sxs-lookup"><span data-stu-id="5d488-178">Step 5: To display our user in the website, create the views and routes in Express</span></span>
<span data-ttu-id="5d488-179">Nu `app.js` är klar.</span><span class="sxs-lookup"><span data-stu-id="5d488-179">Now `app.js` is complete.</span></span> <span data-ttu-id="5d488-180">Vi behöver bara lägga till vägarna och vyerna som visar den information som vi får användaren som hanterar den `/logout` och `/login` vägar som vi skapade.</span><span class="sxs-lookup"><span data-stu-id="5d488-180">We simply need to add the routes and views that show the information we get to the user, as well as handle the `/logout` and `/login` routes that we  created.</span></span>

1. <span data-ttu-id="5d488-181">Skapa `/routes/index.js`-vägen under rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="5d488-181">Create the `/routes/index.js` route under the root directory.</span></span>

    ```JavaScript
                /*
                 * GET home page.
                 */

                exports.index = function(req, res){
                  res.render('index', { title: 'Express' });
                };
    ```

2. <span data-ttu-id="5d488-182">Skapa `/routes/user.js`-vägen under rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="5d488-182">Create the `/routes/user.js` route under the root directory.</span></span>

                ```JavaScript
                /*
                 * GET users listing.
                 */

                exports.list = function(req, res){
                  res.send("respond with a resource");
                };
                ```

 <span data-ttu-id="5d488-183">Dessa skicka vidare förfrågan till vår vyer, inklusive användaren om den finns.</span><span class="sxs-lookup"><span data-stu-id="5d488-183">These pass along the request to our views, including the user if present.</span></span>

3. <span data-ttu-id="5d488-184">Skapa `/views/index.ejs`-vyn under rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="5d488-184">Create the `/views/index.ejs` view under the root directory.</span></span> <span data-ttu-id="5d488-185">Det här är en enkel sida som anropar våra logga in och logga ut metoder och ger oss möjlighet att hämta kontoinformation.</span><span class="sxs-lookup"><span data-stu-id="5d488-185">This is a simple page that calls our login and logout methods and enables us to grab account information.</span></span> <span data-ttu-id="5d488-186">Observera att du kan använda villkorliga `if (!user)` som användaren som skickas via i begäran är bevis som vi har en inloggad användare.</span><span class="sxs-lookup"><span data-stu-id="5d488-186">Notice that we can use the conditional `if (!user)` as the user being passed through in the request is evidence we have a signed-in user.</span></span>

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

4. <span data-ttu-id="5d488-187">Skapa den `/views/account.ejs` vyn under rotkatalogen så att vi kan visa ytterligare information som `passport-azuread` har satt i användarens begäran.</span><span class="sxs-lookup"><span data-stu-id="5d488-187">Create the `/views/account.ejs` view under the root directory so that we can view additional information that `passport-azuread` has put in the user request.</span></span>

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

5. <span data-ttu-id="5d488-188">Vi behöver kontrollera sökningen bra genom att lägga till en layout.</span><span class="sxs-lookup"><span data-stu-id="5d488-188">Let's make this look good by adding a layout.</span></span> <span data-ttu-id="5d488-189">Skapa den ' / views/layout.ejs' vyn under rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="5d488-189">Create the '/views/layout.ejs' view under the root directory.</span></span>

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

##<a name="next-steps"></a><span data-ttu-id="5d488-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5d488-190">Next steps</span></span>
<span data-ttu-id="5d488-191">Slutligen skapar och kör din app.</span><span class="sxs-lookup"><span data-stu-id="5d488-191">Finally, build and run your app.</span></span> <span data-ttu-id="5d488-192">Kör `node app.js`, gå sedan till `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="5d488-192">Run `node app.js`, and then go to `http://localhost:3000`.</span></span>

<span data-ttu-id="5d488-193">Logga in med ett personligt microsoftkonto eller ett konto för arbetet eller skolan och hur användarens identitet visas i listan över /account.</span><span class="sxs-lookup"><span data-stu-id="5d488-193">Sign in with either a personal Microsoft account or a work or school account, and notice how the user's identity is reflected in the /account list.</span></span> <span data-ttu-id="5d488-194">Nu har du en webbapp som skyddas med standardprotokollen som kan autentisera användare med både sina personliga och arbete/skola konton.</span><span class="sxs-lookup"><span data-stu-id="5d488-194">You now have a web app that's secured with industry standard protocols that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="5d488-195">Det färdiga exemplet (utan dina konfigurationsvärden) är tillgängligt som referens i form av en [ZIP-fil](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="5d488-195">For reference, the completed sample (without your configuration values) [is provided as a .zip file](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span></span> <span data-ttu-id="5d488-196">Du kan också klona det från GitHub:</span><span class="sxs-lookup"><span data-stu-id="5d488-196">Alternatively, you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="5d488-197">Du kan nu gå vidare till mer avancerade avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5d488-197">You can now move onto more advanced topics.</span></span> <span data-ttu-id="5d488-198">Du kanske vill prova:</span><span class="sxs-lookup"><span data-stu-id="5d488-198">You might want to try:</span></span>

[<span data-ttu-id="5d488-199">Skydda ett webb-API med Azure AD</span><span class="sxs-lookup"><span data-stu-id="5d488-199">Secure a Web API with Azure AD</span></span>](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
