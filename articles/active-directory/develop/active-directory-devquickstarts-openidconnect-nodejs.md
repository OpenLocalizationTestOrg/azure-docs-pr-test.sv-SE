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
# <a name="nodejs-web-app-sign-in-and-sign-out-with-azure-ad"></a>Node.js-webbapp-inloggning och utloggning med Azure AD
Här kan vi använda Passport:

* Logga hello användaren i toohello app med Azure Active Directory (AD Azure).
* Visa information om hello användare.
* Logga hello användare utanför hello appen.

Passport är ett mellanprogram för autentisering för Node.js. Flexibel och modulära, Passport diskret kan släppas i tooany Express-baserade eller restify-webbappar. En omfattande uppsättning strategier stöder autentisering som använder ett användarnamn och lösenord, Facebook, Twitter och mycket mer. Vi har utvecklat en strategi för Microsoft Azure Active Directory. Vi installerar den här modulen och lägger sedan till hello Microsoft Azure Active Directory `passport-azure-ad` plugin-programmet.

toodo detta, vidta hello följande steg:

1. Registrera en app.
2. Konfigurera din app toouse hello `passport-azure-ad` strategi.
3. Använda Passport tooissue inloggning och utloggning begäranden tooAzure AD.
4. Skriva ut data om hello användare.

hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).  toofollow längs kan du [hämta hello appens stomme som en .zip-fil](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) eller klona hello stommen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

programmet hello slutförts tillhandahålls hello slutet av den här kursen samt.

## <a name="step-1-register-an-app"></a>Steg 1: Registrera en app
1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. Hello menyn hello överst på hello sidan väljer du ditt konto. Under hello **Directory** Välj hello Active Directory-klient där du vill att tooregister ditt program.

3. Välj **fler tjänster** i hello-menyn på hello vänster sida av hello-skärmen och välj sedan **Azure Active Directory**.

4. Välj **App registreringar**, och välj sedan **Lägg till**.

5. Följ hello prompter toocreate en **webbprogram** och/eller **WebAPI**.
  * Hej **namn** av hello programmet beskriver toousers ditt program.

  * Hej **inloggnings-URL** är hello bas-URL för din app.  hello stommens standardvärdet är ' http://localhost: 3000/auth/openid/returnera ''.

6. När du registrerar tilldelar Azure AD appen ett unikt-ID. Du behöver det här värdet i hello följande avsnitt så kopiera den från hello appen på sidan.
7. Från hello **inställningar** -> **egenskaper** för programmet, uppdatera hello App-ID-URI. Hej **App-ID URI** är en unik identifierare för programmet. hello konventionen är toouse hello format `https://<tenant-domain>/<app-name>`, till exempel: `https://contoso.onmicrosoft.com/my-first-aad-app`.

## <a name="step-2-add-prerequisites-tooyour-directory"></a>Steg 2: Lägg till krav tooyour katalog
1. Från kommandoraden hello, ändra kataloger tooyour rotmapp om du inte redan där, och sedan kör hello följande kommandon:

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

2. Dessutom behöver du `passport-azure-ad`:
    * `npm install passport-azure-ad`

Detta installerar hello bibliotek som `passport-azure-ad` beror på.

## <a name="step-3-set-up-your-app-toouse-hello-passport-node-js-strategy"></a>Steg 3: Ställ in din app toouse hello passport-nod-js-strategi
Här kan konfigurera vi Express toouse hello autentiseringsprotokollet OpenID Connect.  Passport är används toodo olika saker, bland annat problemet inloggning och utloggning förfrågningar, hantera hello användarens session och få information om hello användare.

1. toobegin, öppna hello `config.js` filen hello roten i hello-projektet och sedan ange din Apps konfigurationsvärden i hello `exports.creds` avsnitt.

  * Hej `clientID` är hello **program-Id** som är tilldelade tooyour app i portalen för registrering av hello.

  * Hej `returnURL` är hello **omdirigerings-Uri** som du angav i hello-portalen.

  * Hej `clientSecret` är hello hemlighet som du skapade i hello-portalen.

2. Därefter öppnar hello `app.js` filen i projektet hello hello rot. Lägg till följande anrop tooinvoke hello hello `OIDCStrategy` strategin som medföljer `passport-azure-ad`.

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. Efter det att använda hello strategi vi bara refereras toohandle våra inloggning begäranden.

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
Passport använder ett liknande mönster för alla dess strategier (Twitter, Facebook och så vidare) som alla strategiskrivare följer. Granskar hello strategi kan se du att vi skickar en funktion som har en token och en klart som hello parametrar. hello strategin kommer tillbaka toous när den har sitt arbete. Vi vill sedan toostore hello användar- och stash hello token så vi inte behöver tooask för den igen.

> [!IMPORTANT]
hello föregående kod tar alla användare som händer tooauthenticate tooour server. Detta kallas för automatisk registrering. Vi rekommenderar att du inte låta alla autentisera tooa produktionsservern utan att behöva dem registreras via en process som du väljer. Detta är vanligtvis hello mönster du ser i konsumentappar som gör att du tooregister med Facebook men sedan ber du tooprovide ytterligare information. Om det inte var ett exempelprogram kunnat vi extrahera hello användarens e-postadress från token hello-objekt som returneras och sedan bett hello användaren toofill i ytterligare information. Eftersom detta är en testserver lägger vi lägga till dem toohello minnesinterna databasen.


4. Därefter lägger vi till hello-metoder som gör oss tootrack hello inloggade användare som krävs av Passport. De här metoderna omfattar serialisering och avserialisering av hello användarinformation.

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

5.  Därefter lägger vi till hello kod tooload hello Express-motorn. Använder här vi hello standard /views och /routes mönster som Express tillhandahåller.

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

6. Slutligen ska vi lägga till hello dirigerar att leverera hello faktiska inloggning begäranden toohello `passport-azure-ad` motorn:


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


## <a name="step-4-use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a>Steg 4: Använd Passport tooissue-inloggning och utloggning begäranden tooAzure AD
Appen är nu korrekt konfigurerade toocommunicate med hello slutpunkt med hjälp av autentiseringsprotokollet för hello OpenID Connect.  `passport-azure-ad`har tagit hand om alla hello information utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla användarsessioner. Allt som fortfarande ge användarna ett sätt toosign i och logga ut och samla in ytterligare information om hello inloggade användare.

1. Först ska vi lägga till hello standard, logga in, konto och utloggningsmetoderna tooour `app.js` fil:

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

2.  Nu ska vi se dessa i detalj:

  * Hej `/`kommandot omdirigerar toohello index.ejs vyn, skicka hello användaren i hello begäran (om den finns).
  * Hej `/account` vidarebefordra först *garanterar vi autentiseras* (vi implementera som i följande exempel hello), och sedan överför hello användare i hello begäran så att vi kan få ytterligare information om hello användare.
  * Hej `/login` väg anropar våra azuread openidconnect autentiseraren från `passport-azuread`. Om som inte lyckas omdirigerar hello tillbaka för/användarinloggning.
  * Hej `/logout` väg anropar bara hello logout.ejs (och väg), vilket raderar cookies och returnerar hello användaren tillbaka tooindex.ejs.

3. För hello sista delen av `app.js`, Lägg till hello **EnsureAuthenticated** metod som används i `/account`, enligt tidigare.

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

4. Nu ska vi skapa slutligen hello-servern i `app.js`:

```JavaScript

        app.listen(3000);

```


## <a name="step-5-toodisplay-our-user-in-hello-website-create-hello-views-and-routes-in-express"></a>Steg 5: toodisplay våra användare på hello webbplats, skapa hello vyerna och vägarna i Express
Nu `app.js` är klar. Vi bara behöver tooadd hello vägar och vyer som visar hello information vi hämta toohello användare, samt hantera hello `/logout` och `/login` vägar som vi skapade.

1. Skapa hello `/routes/index.js` vägen under rotkatalogen hello.

    ```JavaScript
                /*
                 * GET home page.
                 */

                exports.index = function(req, res){
                  res.render('index', { title: 'Express' });
                };
    ```

2. Skapa hello `/routes/user.js` vägen under rotkatalogen hello.

                ```JavaScript
                /*
                 * GET users listing.
                 */

                exports.list = function(req, res){
                  res.send("respond with a resource");
                };
                ```

 Dessa skickar vidare hello begäran tooour vyer, inklusive hello användare om den finns.

3. Skapa hello `/views/index.ejs` vyn under rotkatalogen hello. Detta är en enkel sida som anropar våra logga in och logga ut metoder och gör att vi toograb kontoinformation. Observera att vi kan använda hello villkorlig `if (!user)` eftersom hello användaren hello begäran som skickas via bevis som vi har en inloggad användare.

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

4. Skapa hello `/views/account.ejs` vyn under rotkatalogen hello så att vi kan visa ytterligare information som `passport-azuread` har satt i hello användarens begäran.

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

5. Vi behöver kontrollera sökningen bra genom att lägga till en layout. Skapa hello ' views/layout.ejs'-vyn under hello rotkatalogen /.

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

##<a name="next-steps"></a>Nästa steg
Slutligen skapar och kör din app. Kör `node app.js`, och sedan gå för`http://localhost:3000`.

Logga in med ett personligt microsoftkonto eller ett konto för arbetet eller skolan och Observera hur hello användaridentitet avspeglas i hello /account lista. Nu har du en webbapp som skyddas med standardprotokollen som kan autentisera användare med både sina personliga och arbete/skola konton.

För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [har angetts som en .zip-fil](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip). Du kan också klona det från GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

Du kan nu gå vidare till mer avancerade avsnitt. Du kanske vill tootry:

[Skydda ett webb-API med Azure AD](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
