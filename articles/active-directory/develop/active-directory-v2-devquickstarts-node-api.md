---
title: aaaSecure Azure Active Directory v2.0 webb-API med Node.js | Microsoft Docs
description: "Lär dig hur toobuild en Node.js webb-API som accepterar token från en personligt Microsoft-konto och från arbets-eller skolkonton."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 0b572fc1-2aaf-4cb6-82de-63010fb1941d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 219e324cca11e107186b7e5f995589b9260af8a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-web-api-by-using-nodejs"></a>Säkra ett webb-API med Node.js
> [!NOTE]
> Inte alla Azure Active Directory-scenarier och funktioner fungerar med hello v2.0-slutpunkten. toodetermine om du ska använda hello v2.0-slutpunkten eller hello v1.0 slutpunkt och Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
> 
> 

När du använder hello Azure Active Directory (AD Azure) v2.0-slutpunkten kan du använda [OAuth 2.0](active-directory-v2-protocols.md) access token tooprotect ditt webb-API. Åtkomst-token, användare som har både en personliga Microsoft-konto och arbete eller skolkonton kan på ett säkert sätt komma åt ditt webb-API med OAuth 2.0.

*Passport* är ett mellanprogram för autentisering för Node.js. Flexibel och modulära, Passport kan släppas diskret in i alla Express-baserade eller restify-webbappar. I Passport, en omfattande uppsättning strategier stöder autentisering med ett användarnamn och lösenord, Facebook, Twitter eller andra alternativ. Vi har utvecklat en strategi för Azure AD. I den här artikeln visar vi du hur tooinstall hello modulen och lägger sedan till hello Azure AD `passport-azure-ad` plugin-programmet.

## <a name="download"></a>Ladda ned
hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs). toofollow hello självstudier, kan du [hämta hello appens stomme som en .zip-fil](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), eller klona hello stommen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Du kan också få hello slutförts programmet hello slutet av den här kursen.

## <a name="1-register-an-app"></a>1: registrera en app
Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa [dessa detaljerade steg](active-directory-v2-app-registration.md) tooregister en app. Se till att du:

* Kopiera hello **program-Id** tilldelade tooyour app. Du behöver det för den här kursen.
* Lägg till hello **Mobile** plattform för din app.
* Kopiera hello **omdirigerings-URI** från hello-portalen. Du måste använda hello URI standardvärdet `urn:ietf:wg:oauth:2.0:oob`.

## <a name="2-install-nodejs"></a>2: Installera Node.js
toouse hello exemplet för den här självstudiekursen måste du [installera Node.js](http://nodejs.org).

## <a name="3-install-mongodb"></a>3: Installera MongoDB
toosuccessfully använda det här exemplet måste du [installera MongoDB](http://www.mongodb.org). I det här exemplet använder du MongoDB toomake REST-API beständiga över serverinstanser.

> [!NOTE]
> I den här artikeln förutsätter vi att du använder hello installation och server standardslutpunkterna för MongoDB: mongodb://localhost.
> 
> 

## <a name="4-install-hello-restify-modules-in-your-web-api"></a>4: Installera hello restify-modulerna i ditt webb-API
Vi använder Resitfy toobuild vårt REST-API. Restify är ett minimalt och flexibelt Node.js programramverk som härleds från Express. Restify har en kraftfull uppsättning funktioner som du kan använda toobuild REST API: er utöver Connect.

### <a name="install-restify"></a>Installera restify
1.  I Kommandotolken, ändra hello katalogen för**azuread**:

    `cd azuread`

    Om hello **azuread** katalogen inte finns, skapar du den:

    `mkdir azuread`

2.  Installera restify:

    `npm install restify`

    hello utdata från kommandot ska se ut så här:

    ```
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
    └── bunyan@0.22.0(mv@0.0.5)
    ```

#### <a name="did-you-get-an-error"></a>Fick du ett felmeddelande?
I vissa operativsystem när du använder hello `npm` kommandot kan du se meddelandet: `Error: EPERM, chmod '/usr/local/bin/..'`. hello fel följs av en begäran om du försöker köra hello kontot som administratör. Om detta inträffar kommandot hello `sudo` toorun `npm` på en högre Privilegienivå.

#### <a name="did-you-get-an-error-related-toodtrace"></a>Fick du ett fel som rör tooDTrace?
När du installerar restify kan det hända att det här meddelandet:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: two
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

Restify har en kraftfull mekanism tootrace REST-anrop med DTrace. DTrace är inte tillgänglig på många operativsystem. Du kan ignorera det här felmeddelandet.


## <a name="5-install-passportjs-in-your-web-api"></a>5: Installera Passport.js i ditt webb-API
1.  Kommandotolken hello ändra hello katalogen för**azuread**.

2.  Installera Passport.js:

    `npm install passport`

    hello hello kommandots utdata ska se ut så här:

    ```
     passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3
    ```

## <a name="6-add-passport-azure-ad-tooyour-web-api"></a>6: Lägg till passport-azure-ad tooyour webb-API
Lägg sedan till hello OAuth-strategin genom att använda passport-azuread. `passport-azuread`är en uppsättning strategier som ansluter Azure AD med Passport. Vi använder den här strategin för ägar-token i REST API-exemplet.

> [!NOTE]
> Även om OAuth 2.0 erbjuder ett ramverk där alla kända token-typer kan utfärdas, används ofta för vissa typer av token. Ägar-token är vanliga tooprotect slutpunkter. Ägar-token är hello mest utfärdat typ av token i OAuth 2.0. Många olika implementeringar OAuth 2.0 förutsätter att ägar-token hello enda typ av token som utfärdas.
> 
> 

1.  I Kommandotolken, ändra hello katalogen för**azuread**.

    `cd azuread`

2.  Installera hello Passport.js `passport-azure-ad` modulen:

    `npm install passport-azure-ad`

    hello hello kommandots utdata ska se ut så här:

    ```
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
    ```

## <a name="7-add-mongodb-modules-tooyour-web-api"></a>7: lägga till MongoDB-moduler tooyour webb-API
I det här exemplet använder vi MongoDB som våra datalagret. 

1.  Installera Mongoose ett vanligt plugin-programmet toomanage modeller och scheman: 

    `npm install mongoose`

2.  Installera hello databasdrivrutinen för MongoDB, som också kallas MongoDB:

    `npm install mongodb`

## <a name="8-install-additional-modules"></a>8: installera ytterligare moduler
Installera hello resterande moduler som krävs.

1.  I Kommandotolken, ändra hello katalogen för**azuread**:

    `cd azuread`

2.  Ange hello följande kommandon. hello kommandon installera hello följande moduler i katalogen node_modules:

    *   `npm install crypto`
    *   `npm install assert-plus`
    *   `npm install posix-getopt`
    *   `npm install util`
    *   `npm install path`
    *   `npm install connect`
    *   `npm install xml-crypto`
    *   `npm install xml2js`
    *   `npm install xmldom`
    *   `npm install async`
    *   `npm install request`
    *   `npm install underscore`
    *   `npm install grunt-contrib-jshint@0.1.1`
    *   `npm install grunt-contrib-nodeunit@0.1.2`
    *   `npm install grunt-contrib-watch@0.2.0`
    *   `npm install grunt@0.4.1`
    *   `npm install xtend@2.0.3`
    *   `npm install bunyan`
    *   `npm update`

## <a name="9-create-a-serverjs-file-for-your-dependencies"></a>9: skapa en Server.js-fil för dina beroenden
En Server.js-fil innehåller hello merparten av hello funktioner för web API-servern. Lägg till de flesta av din kod toothis-fil. Produktion ska refactor du hello funktionerna i mindre filer som för separata vägar och styrenheter. I den här artikeln använder vi Server.js för detta ändamål.

1.  I Kommandotolken, ändra hello katalogen för**azuread**:

    `cd azuread`

2.  Med hjälp av ett redigeringsprogram, skapa en Server.js-fil. Lägg till följande information toohello hello:

    ```Javascript
    'use strict';
    /**
    * Module dependencies.
    */
    var util = require('util');
    var assert = require('assert-plus');
    var mongoose = require('mongoose/');
    var bunyan = require('bunyan');
    var restify = require('restify');
    var config = require('./config');
    var passport = require('passport');
    var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
    ```

3.  Spara hello-filen. Du kommer tillbaka tooit inom kort.

## <a name="10-create-a-config-file-toostore-your-azure-ad-settings"></a>10: skapa en config-fil toostore Azure AD-inställningar
Den här kodfilen skickar hello konfigurationsparametrar från tooPassport.js din Azure AD-portalen. Du har skapat konfigurationsvärdena när du lade till hello webbportalen API toohello hello början av hello artikel. När du har kopierat hello koden förklarar vi vad tooput i hello värdena för dessa parametrar.

1.  I Kommandotolken, ändra hello katalogen för**azuread**:

    `cd azuread`

2.  Skapa en Config.js-fil i en textredigerare. Lägg till hello följande information:

    ```Javascript
    // Don't commit this file tooyour public repos. This config is for first-run.
    exports.creds = {
    mongoose_auth_local: 'mongodb://localhost/tasklist', // Your Mongo auth URI goes here.
    issuer: 'https://sts.windows.net/**<your application id>**/',
    audience: '<your redirect URI>',
    identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For Microsoft, you should never need toochange this.
    };

    ```



### <a name="required-values"></a>Värden som krävs

*   **IdentityMetadata**: det är där `passport-azure-ad` letar efter konfigurationsdata för hello identitetsprovider (IDP) och hello nycklar toovalidate hello JSON Web token (JWTs). Om du använder Azure AD, vill du förmodligen inte toochange detta.

*   **målgruppen**: din omdirigerings-URI från hello-portalen.

> [!NOTE]
> Distribuera dina nycklar med återkommande intervall. Var noga med att du alltid hämtar från hello ”openid_keys” URL och hello appen kan komma åt hello Internet.
> 
> 

## <a name="11-add-hello-configuration-tooyour-serverjs-file"></a>11: Lägg till hello configuration tooyour Server.js-fil
Programmet måste tooread hello värden från hello config-fil som du nyss skapade. Lägg till hello .config-filen som en nödvändig resurs i ditt program. Ange hello globala variabler toothose som finns i Config.js.

1.  Kommandotolken hello ändra hello katalogen för**azuread**:

    `cd azuread`

2.  Öppna Server.js i en textredigerare. Lägg till hello följande information:

    ```Javascript
    var config = require('./config');
    ```

3.  Lägg till ett nytt avsnitt tooServer.js:

    ```Javascript
    // Pass these options in toohello ODICBearerStrategy.
    var options = {
    // hello URL of hello metadata document for your app. Put hello keys for token validation from hello URL found in hello jwks_uri tag in hello metadata.
    identityMetadata: config.creds.identityMetadata,
    issuer: config.creds.issuer,
    audience: config.creds.audience
    };
    // Array toohold signed-in users and hello current signed-in user (owner).
    var users = [];
    var owner = null;
    // Your logger
    var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
    });
    ```

## <a name="12-add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a>12: lägga till hello MongoDB-modellen och schemat genom att använda Mongoose
Anslut sedan dessa tre filer i en REST API-tjänsten.

I den här artikeln använder vi MongoDB toostore våra uppgifter. Diskuterar vi detta i *steg 4*.

I hello Config.js fil som du skapade i steg 11 databasen kallas *tasklist*. Som var det du angav hello slutet av din mongoose_auth_local anslutnings-URL. Du behöver inte toocreate databasen i förväg i MongoDB. hello-databas skapas på hello först kör serverprogrammet (förutsatt att hello databas inte redan finns).

Du har redan nämnt hello server toouse vilken MongoDB-databas. Därefter behöver du toowrite vissa ytterligare kod toocreate hello modellen och schemat för din server-uppgifter.

### <a name="hello-model"></a>hello-modellen
Hej schemamodellen är väldigt enkla. Du kan expandera den om du behöver. 

hello har-schemat dessa värden:

*   **NAMNET**. hello person tilldelade toohello uppgift. Det här är en **sträng** värde.
*   **UPPGIFTEN**. hello namnet på hello aktiviteten. Det här är en **sträng** värde.
*   **DATUM**. hello datum hello uppgiften förfaller. Det här är en **datetime** värde.
*   **SLUTFÖRA**. Om hello-åtgärden har slutförts. Det här är en **booleskt** värde.

### <a name="create-hello-schema-in-hello-code"></a>Skapa hello schema i hello kod
1.  I Kommandotolken, ändra hello katalogen för**azuread**:

    `cd azuread`

2.  Öppna Server.js i redigeringsprogram. Lägg till hello följande information under hello konfigurationspost:

    ```Javascript
    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    // Connect tooMongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');
    ```

Den här koden ansluter toohello MongoDB-servern. Den returnerar även ett schemaobjekt.

#### <a name="using-hello-schema-create-your-model-in-hello-code"></a>Med hello schemat kan skapa din modell i hello kod
Lägg till följande kod hello under hello föregående kod:

```Javascript
// Create a basic schema toostore your tasks and users.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use hello schema tooregister a model.
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```

Som du ser från hello kod först skapar du schemat. Därefter skapar du ett modellobjekt. Du använder hello modellen objektet toostore data i hela hello code när du definierar din **vägar**.

## <a name="13-add-your-routes-for-your-task-rest-api-server"></a>13: lägga till din vägar för aktiviteten REST API-servern
Nu när du har en databas modellen toowork med lägger du till hello vägar du ska använda för REST API-servern.

### <a name="about-routes-in-restify"></a>Om vägar i restify
Vägar i restify fungerar exakt hello samma sätt som när du använder hello Express-stacken. Du definierar vägar genom att använda hello URI som du förväntar dig hello klienten program toocall. Vanligtvis kan du definiera vägarna i en separat fil. I den här självstudiekursen lägger vi vårt vägar i Server.js. För produktion rekommenderar vi att du strukturerar vägar till sina egna fil.

Ett typiskt mönster för en restify-väg är:

```Javascript
function createObject(req, res, next) {
// Do work on object.
_object.name = req.params.object; // Passed value is in req.params under object.
///...
return next(); // Keep hello server going.
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```


Detta är hello mönster på hello mest grundläggande nivån. Restify (och Express) ger mycket mer ingående funktioner, t.ex. hello möjlighet toodefine programtyper och komplex Routning över olika slutpunkter.

#### <a name="add-default-routes-tooyour-server"></a>Lägg till standard vägar tooyour server
Lägg till hello grundläggande CRUD-vägarna: **skapa**, **hämta**, **uppdatera**, och **ta bort**.

1.  I Kommandotolken, ändra hello katalogen för**azuread**:

    `cd azuread`

2.  Öppna Server.js i en textredigerare. Nedan hello databasposter du gjorde tidigare, lägga till hello följande information:

    ```Javascript
    /**
    *
    * APIs for your REST task server
    */
    // Create a task.
    function createTask(req, res, next) {
    // Resitify currently has a bug that doesn't allow you tooset default headers.
    // These headers comply with CORS, and allow you toouse MongoDB Server as your response tooany origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    // Create a new task model, fill it, and save it tooMongoDB.
    var _task = new Task();
    if (!req.params.task) {
    req.log.warn({
    params: p
    }, 'createTodo: missing task');
    next(new MissingTaskError());
    return;
    }
    _task.owner = owner;
    _task.task = req.params.task;
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
    // Delete a task by name.
    function removeTask(req, res, next) {
    Task.remove({
    task: req.params.task,
    owner: owner
    }, function(err) {
    if (err) {
    req.log.warn(err,
    'removeTask: unable toodelete %s',
    req.params.task);
    next(err);
    } else {
    log.info('Deleted task:', req.params.task);
    res.send(204);
    next();
    }
    });
    }
    // Delete all tasks.
    function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
    }
    // Get a specific task based on name.
    function getTask(req, res, next) {
    log.info('getTask was called for: ', owner);
    Task.find({
    owner: owner
    }, function(err, data) {
    if (err) {
    req.log.warn(err, 'get: unable tooread %s', owner);
    next(err);
    return;
    }
    res.json(data);
    });
    return next();
    }
    /// Returns hello list of TODOs that were loaded.
    function listTasks(req, res, next) {
    // Resitify currently has a bug that doesn't allow you tooset default headers.
    // These headers comply with CORS, and allow us toouse MongoDB Server as our response tooany origin.
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
    log.warn(err, "There are no tasks in hello database. Add one!");
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

### <a name="add-error-handling-for-hello-routes"></a>Lägg till felhantering för vägarna hello
Lägga till viss felhantering, så du kan kommunicera tillbaka toohello klienten om hello fel som inträffade.

Lägg till följande kod under hello-kod som du redan har skrivit hello:

```Javascript
///--- Errors for communicating something more information back toohello client.
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


## <a name="14-create-your-server"></a>14: skapa servern
Hej senaste sak toodo är tooadd server-instansen. Hej server-instans hanterar dina anrop.

Restify (och Express) har djupgående anpassning som du kan använda med en REST API-server. I den här kursen använder vi hello mest grundläggande installationen.

```Javascript
/**
* Your server
*/
var server = restify.createServer({
name: "Microsoft Azure Active Directory TODO Server",
version: "2.0.1"
});
// Ensure that you don't drop data on uploads.
server.pre(restify.pre.pause());
// Clean up imprecise paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());
// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());
// Set a per-request Bunyan logger (with requestid filled in).
server.use(restify.requestLogger());
// Allow 5 requests/second by IP address, and burst too10.
server.use(restify.throttle({
burst: 10,
rate: 5,
ip: true,
}));
// Use common commands, such as:
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
mapParams: true
}));
```
## <a name="15-add-hello-routes-without-authentication-for-now"></a>15: lägga till vägar hello (utan autentisering för tillfället)
```Javascript
/// Use CRUD tooadd hello real handlers.
/**
/*
/* Each of these handlers is protected by your Open ID Connect Bearer strategy. Invoke 'oidc-bearer'
/* in hello pasport.authenticate() method. Because REST is stateless, set 'session: false'. You 
/* don't need toomaintain session state. You can experiment with removing API protection.
/* toodo this, remove hello passport.authenticate() method:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser too%s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json tooget some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## <a name="16-run-hello-server"></a>16: kör hello-server
Det är en bra idé tootest servern innan du lägger till autentisering.

hello enklaste sättet tootest din server är att använda curl vid en kommandotolk. toodo detta, behöver du ett enkelt verktyg som du kan använda tooparse utdata som JSON. 

1.  Installera hello JSON-verktyget som vi använder i följande exempel hello:

    `$npm install -g jsontool`

    Detta installerar hello JSON-verktyget globalt.

2.  Kontrollera att MongoDB-instansen körs:

    `$sudo mongod`

3.  Ändra hello katalogen för**azuread**, och kör sedan curl:

    `$ cd azuread`
    `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 2.0OK
    Connection: close
    Content-Type: application/json
    Content-Length: 171
    Date: Tue, 14 Jul 2015 05:43:38 GMT
    [
    "GET /",
    "POST /tasks/:owner/:task",
    "POST /tasks (for JSON body)",
    "GET /tasks",
    "PUT /tasks/:owner",
    "GET /tasks/:owner",
    "DELETE /tasks/:owner/:task"
    ]
    ```

4.  tooadd en uppgift:

    `$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

    hello svaret bör vara:

    ```Shell
    HTTP/1.1 201 Created
    Connection: close
    Access-Control-Allow-Origin: *
    Access-Control-Allow-Headers: X-Requested-With
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 5
    Date: Tue, 04 Feb 2014 01:02:26 GMT
    Hello
    ```

5.  Lista över aktiviteter för Jens:

    `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Om dessa kommandon körs utan problem, är du redo tooadd OAuth toohello REST API-servern.

*Du har nu en REST API-server med MongoDB!*

## <a name="17-add-authentication-tooyour-rest-api-server"></a>17: Lägg till autentisering tooyour REST API-server
Nu när du har en aktiv REST-API, ställa in den toouse den med Azure AD.

I Kommandotolken, ändra hello katalogen för**azuread**:

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-thats-included-with-passport-azure-ad"></a>Använd hello oidc-ägarstrategi som ingår i passport-azure-ad
Hittills har du skapat en typisk REST TODO-server utan någon typ av auktorisering. Lägg till autentisering.

Ange först som du vill toouse Passport. Placera den här rättigheten efter tidigare serverkonfiguration:

```Javascript
// Start using Passport.js.

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [!TIP]
> När du skriver API: er är det en bra idé tooalways länk hello data toosomething unika från hello-token som hello användare inte kan förfalska. När den här servern lagrar TODO-objekt, lagrar dem baserat på hello användaren prenumerations-ID i hello token (anropas via token.sub). Du kan placera hello token.sub hello ”ägare” fältet. Detta säkerställer att bara den användaren kan komma åt hello användarens TODOs. Ingen annan kan komma åt hello TODOs som har angetts. Det finns inga exponering i hello API för ”ägare”. En extern användare kan begära TODOs för andra användare, även om de autentiseras.
> 
> 

Använd sedan hello öppna ID Connect ägarstrategi som medföljer `passport-azure-ad`. Placera det efter vad du klistrade in ovan:

```Javascript
/**
/*
/* Calling hello OIDCBearerStrategy and managing users.
/*
/* Because of hello Passport pattern, you need toomanage users and info tokens
/* with a FindorCreate() method. hello method must be provided by hello implementor.
/* In hello following code, you autoregister any user and implement a FindById().
/* It's a good idea toodo something more advanced.
**/
var findById = function(id, fn) {
for (var i = 0, len = users.length; i < len; i++) {
var user = users[i];
if (user.sub === id) {
log.info('Found user: ', user);
return fn(null, user);
}
}
return fn(null, null);
};
var oidcStrategy = new OIDCBearerStrategy(options,
function(token, done) {
log.info('verifying hello user');
log.info(token, 'was hello token retrieved');
findById(token.sub, function(err, user) {
if (err) {
return done(err);
}
if (!user) {
// "Auto-registration"
log.info('User was added automatically, because they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

Passport använder ett liknande mönster för alla sina strategier (Twitter, Facebook och så vidare). Alla strategigenererare följer toohello mönster. Skicka hello strategi en `function()` som använder en token och `done` som parametrar. hello strategi returneras när den gör allt arbete. Lagra hello användar- och stash hello token så inte behöver du tooask för den igen.

> [!IMPORTANT]
> hello tar föregående kod alla användare som kan autentisera tooyour server. Detta kallas för automatisk registrering. På en produktionsserver förmodligen du vill toolet vem som helst utan att behöva dem gå igenom den registreringsprocess som du väljer. Detta är vanligtvis hello mönster du ser i konsumentappar. hello app kan tillåta dig tooregister med Facebook, men sedan du tillfrågas tooenter ytterligare information. Om du inte använde ett kommandoradsprogram för den här självstudiekursen, kan du extrahera hello e-post från token hello-objekt som returneras. Sedan kan du be hello tooenter ytterligare användarinformation. Eftersom detta är en testserver kan du lägga till hello användaren direkt toohello minnesinterna databasen.
> 
> 

### <a name="protect-endpoints"></a>Skydda slutpunkter
Skydda slutpunkter genom att ange hello **passport.authenticate()** anrop med hello-protokollet som du vill toouse.

Du kan redigera din vägen i serverkoden för mer avancerade alternativ:

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again"></a>18: kör serverprogrammet igen
Använd curl igen toosee om du använder OAuth 2.0 skydd mot dina slutpunkter. Gör detta innan du kör någon av dina klient-SDK: er mot den här slutpunkten. hello rubriker som returneras bör berättar om dina autentisering fungerar.

1.  Kontrollera att MongoDB-isntance körs:

    `$sudo mongod`

2.  Ändra toohello **azuread** katalogen och Använd curl:

    `$ cd azuread`

    `$ node server.js`

3.  Prova med ett grundläggande INLÄGG:

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

Ett 401 svaret anger att hello Passport-lagret försöker tooredirect toohello auktorisera slutpunkt, vilket är exakt vad du vill.

*Du har nu en REST API-tjänst som använder OAuth 2.0!*

Du har gjort så mycket du kan med den här servern utan att använda en OAuth 2.0-kompatibel klient. För att behöver du tooreview en ytterligare vägledning.

## <a name="next-steps"></a>Nästa steg
För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) har angetts som [en .zip-fil](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip). Du kan också klona det från GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Du kan nu gå vidare toomore avancerade alternativ. Du kanske vill tootry [skydda en Node.js-webbapp med hello v2.0-slutpunkten](active-directory-v2-devquickstarts-node-web.md).

Här följer några ytterligare resurser:

* [Utvecklarhandbok för Azure AD v2.0](active-directory-appmodel-v2-overview.md)
* [Stacken spill ”azure-active-directory” tagg](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a>Hämta säkerhetsuppdateringar för våra produkter
Vi rekommenderar att du toosign in toobe meddelas när säkerhetsincidenter. På hello [Microsoft tekniska säkerhetsmeddelanden](https://technet.microsoft.com/security/dd252948) sidan, prenumerera tooSecurity rekommendationerna aviseringar.

