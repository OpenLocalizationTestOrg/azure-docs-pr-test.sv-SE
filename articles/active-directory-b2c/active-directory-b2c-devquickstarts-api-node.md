---
title: "Azure AD B2C: Säkra ett webb-API med Node.js | Microsoft Docs"
description: "Hur toobuild en Node.js webb-API accepterar som token från en B2C-klient"
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
ms.openlocfilehash: 47f5bae025a9ba2f486e36acef36aa37cfb43543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD B2C: Säkra ett webb-API med Node.js
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Med Azure Active Directory (Active AD) B2C kan du skydda ett webb-API med hjälp av OAuth 2.0-åtkomsttoken. Dessa token kan dina klientappar som använder Azure AD B2C tooauthenticate toohello API. Den här artikeln visar hur toocreate ett ”uppgiftslistan”-API som gör att användare tooadd och listan uppgifter. hello web API skyddas med Azure AD B2C och tillåter endast autentiserade användare toomanage sina att göra-lista.

> [!NOTE]
> Det här exemplet har skrivits toobe anslutna tooby vår [iOS B2C-exempelprogram](active-directory-b2c-devquickstarts-ios.md). Gör hello aktuella genomgången först och följ sedan det exemplet.
>
>

**Passport** är ett mellanprogram för autentisering för Node.js. Modulbaserade Passport är flexibelt och kan diskret installeras i alla Express-baserade webbappar eller Restify-webbappar. En omfattande uppsättning strategier stöder autentisering med användarnamn och lösenord, Facebook, Twitter och mycket mer. Vi har utvecklat en strategi för Azure Active Directory (Azure AD). Du installerar den här modulen och lägger sedan till hello Azure AD `passport-azure-ad` plugin-programmet.

toodo det här exemplet måste du:

1. Registrera ett program med Azure AD.
2. Konfigurera ditt program toouse Passport's `azure-ad-passport` plugin-programmet.
3. Konfigurera en klient programmet toocall hello ”uppgiftslistan” webb-API.

## <a name="get-an-azure-ad-b2c-directory"></a>Skaffa en Azure AD B2C-katalog
Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.  En katalog är en behållare för alla användare, appar, grupper och mer.  Om du inte redan har en [skapar du en B2C-katalog](active-directory-b2c-get-started.md) innan du fortsätter.

## <a name="create-an-application"></a>Skapa ett program
Sedan måste toocreate en app i B2C-katalogen som ger Azure AD information toosecurely måste kommunicera med din app. I det här fallet både hello-klientappen och webb-API som representeras av en enda **program-ID**eftersom de bildar en logisk app. toocreate en app, Följ [instruktionerna](active-directory-b2c-app-registration.md). Se till att:

* Inkludera en **webbapp/webb api** i hello program
* Ange `http://localhost/TodoListService` som en **Reply URL**. Det är hello standard-URL för det här kodexemplet.
* Skapa en **programhemlighet** för programmet och kopiera den. Du behöver dessa data senare. Observera att det här värdet måste toobe [ESC-XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) innan du använder den.
* Kopiera hello **program-ID** som är tilldelade tooyour app. Du behöver dessa data senare.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Skapa principer
I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md). Det här programmet innehåller två identitetsupplevelser: registrera sig och logga in. Du behöver toocreate en princip av varje typ, enligt beskrivningen i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).  Tänk på följande när du skapar dina tre principer:

* Välj hello **visningsnamn** och andra registreringsattribut i registreringsprincipen.
* Välj hello **visningsnamn** och **objekt-ID** programanspråken i varje princip.  Du kan också välja andra anspråk.
* Kopiera hello **namn** på varje princip när du har skapat den. Det bör ha hello prefixet `b2c_1_`.  Du behöver dem senare.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

När du har skapat dina tre principer kan du är klar toobuild din app.

toolearn om hur principer fungerar i Azure AD B2C, börja med hello [.NET komma igång med webbappen kursen](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-hello-code"></a>Hämta hello koden
Hej koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS). toobuild hello exempel som du går du [ladda ned stommen av ett projekt som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). Du kan också klona stommen hello:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

hello slutförts appen finns också [som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) eller på hello `complete` grenen av hello samma databas.

## <a name="download-nodejs-for-your-platform"></a>Ladda ned Node.js för plattformen
toosuccessfully använda det här exemplet måste du en fungerande installation av Node.js.

Installera Node.js från [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>Installera MongoDB för din plattform
toosuccessfully använda det här exemplet måste du en fungerande installation av MongoDB. Vi använder MongoDB toomake REST-API beständiga över serverinstanser.

Installera MongoDB från [mongodb.org](http://www.mongodb.org).

> [!NOTE]
> Den här genomgången förutsätter att du använder hello installation och server standardslutpunkterna för MongoDB, som när detta skrivs hello är `mongodb://localhost`.
>
>

## <a name="install-hello-restify-modules-in-your-web-api"></a>Installera hello Restify-modulerna i ditt webb-API
Vi använder Restify toobuild REST-API. Restify är ett minimalt och flexibelt Node.js-program härlett från Express. Det har kraftfulla och stabila funktioner för att skapa REST API:er utöver Connect.

### <a name="install-restify"></a>Installera Restify
Från kommandoraden hello ändra katalogen för`azuread`. Om hello `azuread` katalogen inte finns, skapar du den.

`cd azuread` eller `mkdir azuread;`

Ange hello följande kommando:

`npm install restify`

Restify installeras med det här kommandot.

#### <a name="did-you-get-an-error"></a>Fick du ett felmeddelande?
I vissa operativsystem, när du använder `npm`, felmeddelande hello `Error: EPERM, chmod '/usr/local/bin/..'` och en förfrågan om att du kör hello kontot som administratör. Om det här problemet inträffar kan använda hello `sudo` kommandot toorun `npm` på en högre Privilegienivå.

#### <a name="did-you-get-a-dtrace-error"></a>Fick du ett felmeddelande om DTrace?
När du installerar Restify kan du stöta på något som ser ut som den här texten:

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

Restify erbjuder en kraftfull mekanism för att spåra REST-anrop med DTrace. Men många operativsystem har inte DTrace tillgängligt. Du kan ignorera dessa fel utan att oroa dig.

hello hello kommandots utdata ska se ut liknande toothis text:

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

## <a name="install-passport-in-your-web-api"></a>Installera Passport i ditt webb-API
Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där.

Installera Passport med hello följande kommando:

`npm install passport`

hello hello kommandots utdata ska vara liknande toothis text:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-tooyour-web-api"></a>Lägg till passport-azuread tooyour webb-API
Lägg till hello OAuth-strategin genom att använda `passport-azuread`, en uppsättning strategier som ansluter Azure AD med Passport. Använd den här strategin för ägar-token i hello REST API-exemplet.

> [!NOTE]
> Även om OAuth2 erbjuder ett ramverk där alla kända token-typer kan utfärdas, är det bara vissa token-typer som används ofta. hello-token som skyddar slutpunkter är ägar-token. Dessa typer av token är hello utfärdats oftast i OAuth2. Många olika implementeringar förutsätter att ägar-token hello enda typ av token som utfärdas.
>
>

Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där.

Installera hello Passport `passport-azure-ad` modul med hello följande kommando:

`npm install passport-azure-ad`

hello hello kommandots utdata ska vara liknande toothis text:

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

## <a name="add-mongodb-modules-tooyour-web-api"></a>Lägg till MongoDB-moduler tooyour webb-API
Det här exemplet använder MongoDB som datalager. Installera därför Mongoose, ett vanligt plugin-program för att hantera modeller och scheman.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Installera ytterligare moduler
Installera hello resterande moduler som krävs.

Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:

`cd azuread`

Installera hello moduler i din `node_modules` directory:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`

## <a name="create-a-serverjs-file-with-your-dependencies"></a>Skapa en server.js-fil med dina beroenden
Hej `server.js` filen innehåller hello merparten av hello funktioner för webb-API-server.

Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:

`cd azuread`

Skapa en `server.js`-fil i en textredigerare. Lägg till hello följande information:

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

Spara hello-filen. Tooit tillbaka senare.

## <a name="create-a-configjs-file-toostore-your-azure-ad-settings"></a>Skapa en config.js-fil toostore Azure AD-inställningar
Den här kodfilen skickar hello konfigurationsparametrar från din Azure AD-portalen toohello `Passport.js` fil. Du har skapat konfigurationsvärdena när du lade till hello webbportalen API toohello hello första delen av hello hanteringspaketen. Vi förklarar vilka tooput i hello värdena för dessa parametrar när du har kopierat hello kod.

Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:

`cd azuread`

Skapa en `config.js`-fil i en textredigerare. Lägg till hello följande information:

```Javascript
// Don't commit this file tooyour public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in hello portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // hello Client ID of hello application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add hello B2C tenant name in hello <tenant name> area
tenantName:'<tenant name>',
policyName:'b2c_1_<sign in policy name>' // This is hello policy you'll want toovalidate against in B2C. Usually this is your Sign-in policy (as users sign in toothis API)
passReqToCallback: false // This is a node.js construct that lets you pass hello req all hello way back tooany upstream caller. We turn this off as there is no upstream caller.
};

```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Värden som krävs
`clientID`: hello klient-ID för Web API-program.

`IdentityMetadata`: Det är där `passport-azure-ad` letar efter konfigurationsdata för hello identitetsleverantör. Det verkar också för hello nycklar toovalidate hello JSON web token.

`audience`: hello URI (uniform resource identifier) från hello-portalen som identifierar din anropande programmet.

`tenantName`: Ditt klientnamn (till exempel **contoso.onmicrosoft.com**).

`policyName`: hello principen som du vill toovalidate hello token som kommer tooyour server. Den här principen ska vara hello samma princip som du använder på hello klientprogrammet för inloggning.

> [!NOTE]
> För tillfället hello Använd samma principer för både klient och server-installationen. Om du redan har slutfört en genomgång och skapat dessa principer behöver du inte toodo så igen. Eftersom du har slutfört genomgången hello bör inte måste tooset in nya principer för klientgenomgångarna på hello platsen.
>
>

## <a name="add-configuration-tooyour-serverjs-file"></a>Lägg till konfigurationen tooyour server.js-fil
tooread hello värden från hello `config.js` fil du skapade, lägga till hello `.config` filen som en nödvändig resurs i ditt program och ange sedan hello globala variabler toothose i hello `config.js` dokumentet.

Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:

`cd azuread`

Öppna hello `server.js` fil i en textredigerare. Lägg till hello följande information:

```Javascript
var config = require('./config');
```
Lägg till ett nytt avsnitt för`server.js` som innehåller hello följande kod:

```Javascript
// We pass these options in toohello ODICBearerStrategy.

var options = {
    // hello URL of hello metadata document for your app. We put hello keys for token validation from hello URL found in hello jwks_uri tag of hello in hello metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Nu ska vi lägga till platshållare för hello användare som vi får från våra anropa program.

```Javascript
// array toohold logged in users and hello current logged in user (owner)
var users = [];
var owner = null;
```

Vi fortsätter med att skapa våra transaktionsloggfiler också.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a>Lägga till hello MongoDB-modellen och schemat genom att använda Mongoose
hello betalar förberett tidigare när du för ihop dessa tre filer i en REST API-tjänsten.

För den här genomgången Använd MongoDB toostore dina aktiviteter, enligt tidigare diskussion.

I hello `config.js` fil, kallade du databasen **tasklist**. Det här namnet har också vad du placerar hello slutet av hello `mongoose_auth_local` anslutnings-URL. Du behöver inte toocreate databasen i förväg i MongoDB. Hello databasen skapas åt dig på hello första gången du kör serverprogrammet.

När du har talat hello servern vilken MongoDB databasen toouse, behöver du toowrite vissa ytterligare kod toocreate hello modellen och schemat för serveraktiviteterna.

### <a name="expand-hello-model"></a>Expandera hello modellen
Den här schemamodellen är enkel. Du kan expandera den efter behov.

`owner`: Den som är tilldelad toohello aktivitet. Det här objektet är en **sträng**.  

`Text`: själva hello-aktiviteten. Det här objektet är en **sträng**.

`date`: hello datum hello uppgiften förfaller. Det här objektet är en **datetime**.

`completed`: Om hello uppgiften har slutförts. Det här objektet är en **boolesk**.

### <a name="create-hello-schema-in-hello-code"></a>Skapa hello schema i hello kod
Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:

`cd azuread`

Öppna hello `server.js` fil i en textredigerare. Lägg till följande information under hello konfigurationspost hello:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect tooMongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema toostore our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use hello schema tooregister a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Du först skapa hello schemat och skapas ett modellobjekt som du använder toostore data i hela hello code när du definierar din **vägar**.

## <a name="add-routes-for-your-rest-api-task-server"></a>Lägga till vägar för din REST API-aktivitetsserver
Nu när du har en databas modellen toowork med lägger du till hello vägar som du använder för REST API-servern.

### <a name="about-routes-in-restify"></a>Om vägar i Restify
I Restify fungerar vägar i hello samma sätt som när de använder hello Express-stacken. Du definierar vägar genom att använda hello URI som du förväntar dig hello klienten program toocall.

Ett typiskt mönster för en Restify-väg är:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep hello server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify och Express kan ge mycket mer ingående funktioner, till exempel att definiera programtyper och göra komplex routning över olika slutpunkter. För hello i den här kursen är behåller vi vägarna enkla.

#### <a name="add-default-routes-tooyour-server"></a>Lägg till standard vägar tooyour server
Du nu lägga till hello grundläggande CRUD-vägarna för **skapa** och **lista** för vårt REST-API. Andra vägar kan hittas i hello `complete` grenen av hello exempel.

Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:

`cd azuread`

Öppna hello `server.js` fil i en textredigerare. Nedan hello databasposter gjorde du senare lägga till hello följande information:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you tooset default headers
    // This headers comply with CORS and allow us toomongodbServer our response tooany origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it tooMongodb
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
            req.log.warn(err, 'createTask: unable toosave');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns hello list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you tooset default headers
    // This headers comply with CORS and allow us toomongodbServer our response tooany origin

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
            log.warn(err, "There is no tasks in hello database. Add one!");
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


#### <a name="add-error-handling-for-hello-routes"></a>Lägg till felhantering för vägarna hello
Lägga till vissa felhantering så att du kan kommunicera problem tillbaka toohello klienten så att den kan förstå.

Lägg till följande kod hello:

```Javascript
///--- Errors for communicating something interesting back toohello client
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


## <a name="create-your-server"></a>Skapa servern
Du har nu definierat databasen och placerat ut vägarna. hello sista för du toodo är tooadd hello serverinstansen som hanterar dina anrop.

Restify och Express ger anpassa för en REST API-server, men vi använder här hello mest grundläggande installationen.

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

// Allow 5 requests/second by IP, and burst too10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use hello common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping tooREST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-hello-routes-toohello-server-without-authentication"></a>Lägg till hello vägar toohello server (utan autentisering)
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
    consoleMessage += '\n Open your browser too%s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json tooget some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

```

## <a name="add-authentication-tooyour-rest-api-server"></a>Lägg till autentisering tooyour REST API-server
Nu när du har en aktiv REST API-server kan du använda den med Azure AD.

Från kommandoraden hello ändra katalogen för`azuread`, om den inte redan finns där:

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Använd hello oidc-Ägarstrategi som ingår i passport-azure-ad
> [!TIP]
> När du skriver att API: er, bör du alltid länka hello data toosomething unika från hello-token som hello användare inte kan förfalska. När hello-servern lagrar ToDo-objekt, sker det baserat på hello **oid** för hello användare i hello token (anropas via token.oid), som ska ingå i hello ”ägare”. Det här värdet garanterar att bara denna användaren kan komma åt sina egna ToDo-objekt. Exponeras aldrig i hello API för ”ägare”, så en extern användare kan begära andras ToDo-objekt, även om de autentiseras.
>
>

Använd sedan hello ägarstrategi som medföljer `passport-azure-ad`.

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
        log.info('verifying hello user');
        log.info(token, 'was hello token retreived');
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

Passport använder hello samma mönster för alla sina strategier. Du skickar en `function()` som har `token` och `done` som parametrar. hello strategin kommer tillbaka tooyou när den har utfört allt sitt arbete. Du bör och sedan lagra hello användaren och spara hello token så att du inte behöver tooask för den igen.

> [!IMPORTANT]
> hello koden ovan tar alla användare som tooauthenticate tooyour server. Denna process kallas autoregistrering. I produktionsservrar Låt inte i alla användare åtkomst hello API utan att behöva dem gå igenom registreringsprocessen. Den här processen är vanligtvis hello mönster du ser i konsumentappar som gör att du tooregister med hjälp av Facebook men sedan ber du toofill i ytterligare information. Om programmet inte var ett kommandoradsprogram, kunnat vi extrahera hello e-post från token hello-objekt som returneras och sedan bett användarna toofill i ytterligare information. Eftersom detta är ett exempel kan vi lägga till dem tooan minnesinterna databasen.
>
>

## <a name="run-your-server-application-tooverify-that-it-rejects-you"></a>Kör tooverify din server-program avvisar att det dig
Du kan använda `curl` toosee om du nu har OAuth2-skydd mot dina slutpunkter. hello rubriker som returneras bör vara tillräckligt med tootell att du inte på hello rätt sökväg.

Kontrollera att MongoDB-instansen körs:

    $sudo mongodb

Ändra toohello directory och kör hello server:

    $ cd azuread
    $ node server.js

I ett nytt terminalfönster kör `curl`

Prova med ett grundläggande INLÄGG:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Ett 401-fel är hello-svar som du vill ha. Anger det att hello Passport-lagret försöker tooredirect toohello auktorisera slutpunkt.

## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>Du har nu en REST API-tjänst som använder OAuth2
Du har implementerat en REST API med Restify och OAuth! Nu har du tillräckligt med kod så att du kan fortsätta toodevelop din tjänst och bygga på det här exemplet. Du har gjort så mycket du kan med den här servern utan att använda en OAuth2-kompatibel klient. För att nästa steg att använda ytterligare en genomgång som våra [ansluta tooa webb-API med hjälp av iOS med B2C](active-directory-b2c-devquickstarts-ios.md) genomgången.

## <a name="next-steps"></a>Nästa steg
Du kan nu flytta toomore avancerade ämnen, exempelvis:

[Ansluta tooa webb-API med hjälp av iOS med B2C](active-directory-b2c-devquickstarts-ios.md)
