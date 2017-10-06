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
# <a name="azure-ad-b2c-add-sign-in-tooa-nodejs-web-app"></a>Azure AD B2C: Lägga till inloggning tooa Node.js-webbapp

**Passport** är ett mellanprogram för autentisering för Node.js. Modulbaserade Passport är mycket flexibelt och kan diskret installeras i alla Express-baserade webbappar eller Restify-webbappar. En omfattande uppsättning strategier stöder autentisering med användarnamn och lösenord, Facebook, Twitter och mycket mer.

Vi har utvecklat en strategi för Azure Active Directory (Azure AD). Du installerar den här modulen och lägger sedan till hello Azure AD `passport-azure-ad` plugin-programmet.

toodo, måste du:

1. Registrera ett program med hjälp av Azure AD.
2. Konfigurera din app toouse hello `passport-azure-ad` plugin-programmet.
3. Använda Passport tooissue inloggning och utloggning begäranden tooAzure AD.
4. Skriva ut användardata.

Hej koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS). toofollow längs kan du [hämta hello appens stomme som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip). Du kan också klona stommen hello:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS.git```

programmet hello slutförts tillhandahålls hello slutet av den här kursen.

## <a name="get-an-azure-ad-b2c-directory"></a>Skaffa en Azure AD B2C-katalog

Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.  En katalog är en behållare för alla användare, appar, grupper och mer. Om du inte redan har en B2C-katalog [skapar du en](active-directory-b2c-get-started.md) innan du fortsätter den här guiden.

## <a name="create-an-application"></a>Skapa ett program

Sedan måste toocreate en app i B2C-katalogen. Det ger Azure AD den information som behövs toocommunicate säkert med din app. Både hello-klientappen och webb-API representeras av ett enda **program-ID**eftersom de bildar en logisk app. toocreate en app, Följ [instruktionerna](active-directory-b2c-app-registration.md). Se till att:

- Inkludera en **webbapp**/**webb-API** i hello program.
- Ange `http://localhost:3000/auth/openid/return` som en **Reply URL**. Det är hello standard-URL för det här kodexemplet.
- Skapa en **programhemlighet** för programmet och kopiera den. Du behöver den senare. Observera att det här värdet måste toobe [ESC-XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) innan du använder den.
- Kopiera hello **program-ID** som är tilldelade tooyour app. Du behöver även det senare.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Skapa dina principer

I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md). Den här appen innehåller tre identitetsupplevelser: registrering, inloggning och inloggning med Facebook. Du behöver toocreate denna princip av varje typ, enligt beskrivningen i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Tänk på följande när du skapar dina tre principer:

- Välj hello **visningsnamn** och andra registreringsattribut i registreringsprincipen.
- Välj hello **visningsnamn** och **objekt-ID** programanspråken i varje princip. Du kan också välja andra anspråk.
- Kopiera hello **namn** på varje princip när du har skapat den. Det bör ha hello prefixet `b2c_1_`.  Du behöver principnamnen senare.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

När du har skapat dina tre principer du är klar toobuild din app.

Observera att den här artikeln inte beskriver hur toouse hello principer du nyss skapade. toolearn om hur principer fungerar i Azure AD B2C, börja med hello [.NET komma igång med webbappen kursen](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="add-prerequisites-tooyour-directory"></a>Lägg till krav tooyour katalog

Ändra kataloger tooyour rotmapp om du inte redan det från hello kommandorad. Kör följande kommandon hello:

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

Dessutom använde vi `passport-azure-ad` för vår förhandsgranskning i hello stommen i hello Snabbstart.

- `npm install passport-azure-ad`

Detta installerar hello bibliotek som `passport-azure-ad` beror på.

## <a name="set-up-your-app-toouse-hello-passport-nodejs-strategy"></a>Konfigurera din app toouse hello Passport-Node.js-strategin
Konfigurera hello Express mellanprogram toouse hello autentiseringsprotokollet OpenID Connect. Passport kommer att använda tooissue inloggning och utloggningsförfrågningar, hantera användarsessioner och få information om användare, bland andra åtgärder.

Öppna hello `config.js` filen i hello roten av projektet hello och ange din Apps konfigurationsvärden i hello `exports.creds` avsnitt.
- `clientID`: hello **program-ID** tilldelade tooyour app i portalen för registrering av hello.
- `returnURL`: hello **omdirigerings-URI** du angav i hello-portalen.
- `tenantName`: hello innehavarens namn för din app, till exempel **contoso.onmicrosoft.com**.

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Öppna hello `app.js` filen i projektet hello hello rot. Lägg till följande anrop tooinvoke hello hello `OIDCStrategy` strategin som medföljer `passport-azure-ad`.


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

Använd hello strategi som du precis refererade till toohandle inloggning begäranden.

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
Passport använder ett liknande mönster för alla dess strategier (inklusive Twitter och Facebook). Alla strategigenererare följer toothis mönster. När du tittar på strategin för hello kan du se att du skickar en `function()` som har en token och en `done` som hello parametrar. hello strategin kommer tillbaka tooyou när den har utfört allt sitt arbete. Lagra hello användaren och stash hello token så att du inte behöver tooask för den igen.

> [!IMPORTANT]
hello föregående kod körs på alla användare som autentiseras hello-servern. Det här är automatisk registrering. När du använder produktionsservrar vill du inte toolet användare om de inte har genomgått en registreringsprocess som du har ställt in. Det här mönstret är vanligt i konsumentappar. Dessa kan du tooregister med hjälp av Facebook men uppmanar sedan användaren toofill i ytterligare information. Om vårt program inte var ett exempelprogram, kan vi extrahera en e-postadress från token hello-objekt som returneras och sedan ber du hello användaren toofill i ytterligare information. Eftersom detta är en testserver lägger vi helt enkelt lägga till användare toohello minnesinterna databasen.

Lägga till hello-metoder som gör att du tookeep reda på användare som har loggat in, vilket krävs av Passport. Serialisering och avserialisering av användarinformation är ett par exempel på den här typen av metoder:

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

Lägg till hello kod tooload hello Express-motorn. I följande hello, ser du att vi använder hello standard `/views` och `/routes` Standardmönstret som Express tillhandahåller.

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

Lägg till hello `POST` dirigeringskommandona som lämnar hello faktiska inloggning begäranden toohello `passport-azure-ad` motorn:

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

## <a name="use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a>Använda Passport tooissue inloggning och utloggning begäranden tooAzure AD

Appen är nu korrekt konfigurerade toocommunicate med hello v2.0-slutpunkten med hjälp av autentiseringsprotokollet för hello OpenID Connect. `passport-azure-ad`har tagit hand om hello information utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla användarsessioner. Allt som fortfarande är toogive användarna en sätt toosign i och logga ut och toogather ytterligare information om användare som har loggat in.

Lägg först till hello standard, logga in, konto och utloggningsmetoderna tooyour `app.js` fil:

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

tooreview dessa metoder i detalj:
- Hej `/` kommandot omdirigerar toohello `index.ejs` vyn genom att skicka hello användaren i hello begäran (om den finns).
- Hej `/account` kontrollerar först att du har autentiserats (hello implementeringen för detta visas nedan). Därefter skickas användaren hello i hello begäran så att du kan få ytterligare information om hello användare.
- Hej `/login` väg anrop hello `azuread-openidconnect` autentiseraren från `passport-azure-ad`. Om det inte lyckas hello kommandot omdirigerar hello användaren tillbaka för`/login`.
- `/logout` anropar bara `logout.ejs` (och dess dirigeringskommando). Detta kommando rensar cookies och sedan returnerar hello användaren tillbaka för`index.ejs`.


För hello sista delen av `app.js`, lägga till hello `EnsureAuthenticated` metod som används i hello `/account` vägen.

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

Skapa slutligen hello-servern i `app.js`.

```JavaScript

app.listen(3000);

```


## <a name="create-hello-views-and-routes-in-express-toocall-your-policies"></a>Skapa hello vyer och dirigerar i Express toocall dina principer

Nu är din `app.js` klar. Du behöver bara tooadd hello vägarna och vyerna som gör att du toocall hello principer för inloggning och registrering. Dessa hanterar även hello `/logout` och `/login` vägar som du skapade.

Skapa hello `/routes/index.js` vägen under rotkatalogen hello.

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

Skapa hello `/routes/user.js` vägen under rotkatalogen hello.

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Dessa enkla vägar skickar vidare förfrågningar tooyour vyer. De omfattar hello användaren, om en sådan finns.

Skapa hello `/views/index.ejs` vyn under rotkatalogen hello. Det här är en enkel sida som anropar principer för in- och utloggning. Du kan också använda den toograb kontoinformation. Observera att du kan använda villkorlig hello `if (!user)` som hello användaren skickades via hello begäran tooprovide bevis hello användaren är inloggad.

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

Skapa hello `/views/account.ejs` vyn under rotkatalogen hello så att du kan visa ytterligare information som `passport-azure-ad` i hello användarens begäran.

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

Nu kan du skapa och köra din app.

Kör `node app.js` och navigera för`http://localhost:3000`


Registrering eller inloggning toohello appen med hjälp av e-post eller Facebook. Logga ut och logga in igen som en annan användare.

##<a name="next-steps"></a>Nästa steg

För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [har angetts som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip). Du kan också klona det från GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-nodejs.git```

Du kan nu gå vidare toomore avancerade alternativ. Du kan prova följande:

[Skydda ett webb-API med hjälp av hello B2C-modellen i Node.js](active-directory-b2c-devquickstarts-api-node.md)

<!--

For additional resources, check out:
You can now move on toomore advanced B2C topics. You might try:

[Call a Node.js web API from a Node.js web app]()

[Customizing hello your B2C App's UX >>]()

-->
