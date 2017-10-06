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
# <a name="add-sign-in-tooa-nodejs-web-app"></a>Lägga till inloggning tooa Node.js-webbapp

> [!NOTE]
> Inte alla Azure Active Directory-scenarier och funktioner fungerar med hello v2.0-slutpunkten. toodetermine om du ska använda hello v2.0-slutpunkten eller hello v1.0 slutpunkt och Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
> 

I den här självstudiekursen kommer använda vi Passport toodo hello följande uppgifter:

* Logga in hello användare med hjälp av Azure Active Directory (AD Azure) i en webbapp och hello v2.0-slutpunkten.
* Visa information om hello användare.
* Logga hello användare utanför hello appen.

**Passport** är ett mellanprogram för autentisering för Node.js. Flexibel och modulära, Passport kan släppas diskret in i alla Express-baserade eller restify-webbappar. I Passport, en omfattande uppsättning strategier stöder autentisering med ett användarnamn och lösenord, Facebook, Twitter eller andra alternativ. Vi har utvecklat en strategi för Azure AD. I den här artikeln visar vi du hur tooinstall hello modulen och lägger sedan till hello Azure AD `passport-azure-ad` plugin-programmet.

## <a name="download"></a>Ladda ned
hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs). toofollow hello självstudier, kan du [hämta hello appens stomme som en .zip-fil](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) eller klona hello stommen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Du kan också få hello slutförts programmet hello slutet av den här kursen.

## <a name="1-register-an-app"></a>1: registrera en app
Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa [dessa detaljerade steg](active-directory-v2-app-registration.md) tooregister en app. Se till att du:

* Kopiera hello **program-Id** tilldelade tooyour app. Du behöver den för den här självstudiekursen.
* Lägg till hello **Web** plattform för din app.
* Kopiera hello **omdirigerings-URI** från hello-portalen. Du måste använda hello URI standardvärdet `urn:ietf:wg:oauth:2.0:oob`.

## <a name="2-add-prerequisities-tooyour-directory"></a>2: Lägg till prerequisities tooyour katalog
Ändra kataloger toogo tooyour rotmapp om du inte redan har det i en kommandotolk. Kör följande kommandon hello:

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

Dessutom kan vi använda `passport-azure-ad` i hello stommen i Snabbstart hello:

* `npm install passport-azure-ad`

Detta installerar hello bibliotek som `passport-azure-ad` använder.

## <a name="3-set-up-your-app-toouse-hello-passport-node-js-strategy"></a>3: Ställ in din app toouse hello passport-nod-js-strategi
Ställ in hello Express mellanprogram toouse hello autentiseringsprotokollet OpenID Connect. Använda Passport tooissue inloggning och utloggning begäranden, hantera hello användarens session och få information om hello användare, bland annat.

1.  Hello rot i hello projekt, öppna hello Config.js-fil. I hello `exports.creds` ange din Apps konfigurationsvärden.
  
  * `clientID`: hello **program-Id** som är tilldelade tooyour app i hello Azure-portalen.
  * `returnURL`: hello **omdirigerings-URI** som du angav i hello-portalen.
  * `clientSecret`: hello hemlighet som du skapade i hello-portalen.

2.  Öppna hello App.js filen i hello roten av hello-projekt. tooinvoke hello OIDCStrategy stratey som medföljer `passport-azure-ad`, Lägg till följande anrop hello:

  ```JavaScript
  var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

  // Add some logging
  var log = bunyan.createLogger({
      name: 'Microsoft OIDC Example Web Application'
  });
  ```

3.  toohandle din inloggning begäranden, Använd hello strategi som du precis refererade till:

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

Passport använder ett liknande mönster för alla sina strategier (Twitter, Facebook och så vidare). Alla strategigenererare följer toohello mönster. Skicka hello strategi en `function()` som använder en token och `done` som parametrar. hello strategi returneras när den gör allt arbete. Lagra hello användar- och stash hello token så inte behöver du tooask för den igen.

  > [!IMPORTANT]
  > hello tar föregående kod alla användare som kan autentisera tooyour server. Detta kallas för automatisk registrering. På en produktionsserver förmodligen du vill toolet vem som helst utan att behöva dem gå igenom den registreringsprocess som du väljer. Detta är vanligtvis hello mönster som du ser i konsumentappar. hello app kan tillåta dig tooregister med Facebook, men sedan du tillfrågas tooenter ytterligare information. Om du inte använde ett kommandoradsprogram för den här självstudiekursen, kan du extrahera hello e-post från token hello-objekt som returneras. Sedan kan du be hello tooenter ytterligare användarinformation. Eftersom detta är en testserver kan du lägga till hello användaren direkt toohello minnesinterna databasen.
  > 
  > 

4.  Lägger till hello metoder för att du använder tookeep reda på användare som är inloggad, vilket krävs av Passport. Detta inkluderar serialisering och avserialisering av hello användarinformation:

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

5.  Lägg till hello-kod som läser in hello Express-motorn. Du använder hello standard /views och /routes mönster som Express tillhandahåller:

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

6.  Lägg till hello POST dirigerar att leverera hello faktiska inloggning begäranden toohello `passport-azure-ad` motorn:

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

## <a name="4-use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a>4: Använd Passport tooissue inloggning och utloggning begär tooAzure AD
Appen är nu konfigurera toocommunicate med hello v2.0-slutpunkten med hjälp av autentiseringsprotokollet för hello OpenID Connect. Hej `passport-azure-ad` strategi tar hand om alla hello information utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla hello användarsessioner. Alla lämnas toodo är toogive användarna en sätt toosign i och logga ut och toogather mer information om hello användare är inloggad.

1.  Lägg till hello **standard**, **inloggning**, **konto**, och **logga ut** metoder tooyour App.js fil:

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

  Här följer hello information:
    
    * Hej `/` kommandot omdirigerar toohello index.ejs vyn. Passerar hello användaren i hello begäran (om den finns).
    * Hej `/account` vidarebefordra först *innebär att autentiseras* (du implementera som i följande kod hello). Sedan överförs den hello användaren i hello-begäran. Detta är så att du kan få mer information om hello användare.
    * Hej `/login` kommandot anropar din `azuread-openidconnect` autentiseraren från `passport-azuread`. Om som inte lyckas omdirigerar den hello användaren tillbaka för`/login`.
    * Hej `/logout` väg anropar hello logout.ejs visa (och väg). Detta kommando rensar cookies och sedan returnerar hello tillbaka tooindex.ejs för användaren.

2.  Lägg till hello **EnsureAuthenticated** metod som du använde tidigare i `/account`:

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

3.  App.js, skapa hello server:

  ```JavaScript

  app.listen(3000);

  ```


## <a name="5-create-hello-views-and-routes-in-express-that-you-show-your-user-on-hello-website"></a>5: skapa hello vyerna och vägarna i Express du visa din användare på hello webbplats
Lägg till hello vägarna och vyerna som visar information om toohello användare. hello-vägarna och vyerna även hantera hello `/logout` och `/login` vägar som du skapade.

1. Skapa hello i hello rotkatalog `/routes/index.js` vägen.

  ```JavaScript

  /*
  * GET home page.
  */

  exports.index = function(req, res){
    res.render('index', { title: 'Express' });
  };
  ```

2.  Skapa hello i hello rotkatalog `/routes/user.js` vägen.

  ```JavaScript

  /*
  * GET users listing.
  */

  exports.list = function(req, res){
    res.send("respond with a resource");
  };
  ```

  `/routes/index.js`och `/routes/user.js` är enkla vägar skickar vidare hello begäran tooyour vyer, inklusive hello användaren, om sådan finns.

3.  Skapa hello i hello rotkatalog `/views/index.ejs` vyn. Den här sidan anropar din **inloggning** och **logga ut** metoder. Du också använda hello `/views/index.ejs` visa toocapture kontoinformation. Du kan använda villkorlig hello `if (!user)` som hello användare som skickas via hello-begäran. Det finns bevis att du har en inloggad användare.

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

4.  Skapa hello i hello rotkatalog `/views/account.ejs` vyn. Hej `/views/account.ejs` vy kan du tooview ytterligare information som `passport-azuread` placerar i hello användarens begäran.

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

5.  Lägg till en layout. Skapa hello i hello rotkatalog `/views/layout.ejs` vyn.

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

6.  toobuild och kör din app kör `node app.js`. Gå sedan för`http://localhost:3000`.

7.  Logga in med ett personligt microsoftkonto eller ett arbets- eller skolkonto konto. Observera att hello användaridentitet avspeglas i hello /account lista. 

Nu har du en webbapp som skyddas med hjälp av standardprotokollen. Du kan autentisera användare i din app genom att använda sina personliga och arbetet eller skolan konton.

## <a name="next-steps"></a>Nästa steg
För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) har angetts som [en .zip-fil](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip). Du kan också klona det från GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Därefter kan du gå vidare toomore avancerade alternativ. Du kanske vill tootry:

[Skydda ett Node.js-webb-API med hjälp av hello v2.0-slutpunkten](active-directory-v2-devquickstarts-node-api.md)

Här följer några ytterligare resurser:

* [Utvecklarhandbok för Azure AD v2.0](active-directory-appmodel-v2-overview.md)
* [Stacken spill ”azure-active-directory” tagg](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a>Hämta säkerhetsuppdateringar för våra produkter
Vi rekommenderar att du toosign in toobe meddelas när säkerhetsincidenter. På hello [Microsoft tekniska säkerhetsmeddelanden](https://technet.microsoft.com/security/dd252948) sidan, prenumerera tooSecurity rekommendationerna aviseringar.

