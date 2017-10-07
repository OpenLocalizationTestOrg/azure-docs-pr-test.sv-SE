---
title: "aaaUsing Twilio för röst-, VoIP- och SMS-meddelanden i Azure"
description: "Lär dig hur toomake ett telefonsamtal och skicka ett SMS-meddelandet med hello Twilio API-tjänsten på Azure. Kodexempel som skrivits i Node.js."
services: 
documentationcenter: nodejs
author: devinrader
manager: wpickett
editor: 
ms.assetid: f558cbbd-13d2-416f-b9b1-33a99c426af9
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/25/2014
ms.author: wpickett
ms.openlocfilehash: 6c44d60e217fcdf51e69fd2a8197f979afbb507a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Med hjälp av Twilio för röst-, VoIP- och SMS-meddelanden i Azure
Den här guiden visar hur toobuild appar som kommunicerar med Twilio och node.js i Azure.

<a id="whatis"/>

## <a name="what-is-twilio"></a>Vad är Twilio?
Twilio är en API-plattform som gör det lätt för utvecklare toomake och ta emot samtal, skicka och ta emot meddelanden och bädda in VoIP-anrop till webbläsarbaserad och egna mobila program. Vi ska du kort gå igenom hur det fungerar innan du dyker.

### <a name="receiving-calls-and-text-messages"></a>Ta emot samtal och textmeddelanden
Twilio kan utvecklare för[köpa programmable telefonnummer] [ purchase_phone] som kan vara används tooboth skicka och ta emot samtal och textmeddelanden. När flera Twilio tar emot ett inkommande samtal eller textmeddelande, skickar Twilio ditt webbprogram för en HTTP POST eller GET-begäran ber dig instruktioner för hur toohandle hello samtal eller textmeddelande. Servern svarar Tootwilio's HTTP-begäran med [TwiML][twiml], en enkel uppsättning XML-taggar som innehåller instruktioner om hur toohandle samtal eller textmeddelande. Exempel på TwiML ser vi bara en liten stund.

### <a name="making-calls-and-sending-text-messages"></a>Anrop och skicka textmeddelanden
Genom att göra HTTP-begäranden toohello Twilio-webbtjänstens API kan utvecklare skicka textmeddelanden eller initiera utgående telefonsamtal. För utgående samtal måste hello developer också ange en URL som returnerar TwiML instruktioner för hur toohandle hello utgående anropa när den är ansluten.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>Bädda in VoIP-funktioner i UI-kod (JavaScript, iOS eller Android)
Twilio ger en klientsidan SDK som kan göra alla skrivbord webbläsare, en iOS-app eller en Android-app till en VoIP-telefon. I den här artikeln ska vi fokusera på hur toouse VoIP anrop i hello webbläsare. I tillägg toohello *Twilio JavaScript SDK* körs i hello webbläsare, ett program för serversidan (vår node.js-program) måste vara används tooissue ”kapaciteten token”-toohello JavaScript-klienten. Du kan läsa mer om hur du använder VoIP med node.js [på hello Twilio dev blogg][voipnode].

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a>Registrera dig för Twilio (Microsoft rabatt)
Innan du använder Twilio-tjänster, måste du först [registrera dig för ett konto][signup]. Microsoft Azure-kunder får en särskild rabatt - [vara säker på att toosign här][signup]!

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a>Skapa och distribuera en node.js Azure-webbplats
Därefter behöver du toocreate en node.js-webbplats som körs på Azure. [hello officiella dokumentation för att göra detta finns här][azure_new_site]. På en hög nivå, kommer du att göra hello följande:

* Registrera dig för ett Azure-konto om du inte redan har en
* Med hjälp av hello Azure admin console toocreate en ny webbplats
* Att lägga till stöd för källa kontrollen (vi antar som du använde git)
* Skapa en fil `server.js` med en enkel node.js-webbapp
* Distribuera den här enkla program tooAzure

<a id="twiliomodule"/>

## <a name="configure-hello-twilio-module"></a>Konfigurera hello Twilio-modul
Därefter börjar vi toowrite en enkel node.js-program som använder hello Twilio-API. Innan vi kan behöver vi tooconfigure våra Twilio-autentiseringsuppgifter.

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Konfigurera Twilio-autentiseringsuppgifter i systemmiljövariabler
I ordning toomake autentiseras förfrågningar mot hello Twilio-serverdel behöver vi våra kontots SID och auth token, vilken funktion som hello användarnamn och lösenord som angetts för våra Twilio-konto. hello säkraste sättet tooconfigure dem för användning med hello nod modul i Azure är via systemmiljövariabler som du anger direkt i hello Azure-administrationskonsolen.

Välj din node.js-webbplats och på hello ”konfigurera” länk.  Om du bläddra nedåt för visas ett område där du kan ange konfigurationsegenskaper för programmet.  Ange autentiseringsuppgifterna för ditt Twilio-konto ([hittades på konsolen Twilio][twilio_console]) enligt – se till att tooname dem `TWILIO_ACCOUNT_SID` och `TWILIO_AUTH_TOKEN`respektive:

![Azure-administrationskonsolen][azure-admin-console]

När du har konfigurerat dessa variabler kan du starta om ditt program i hello Azure-konsolen.

### <a name="declaring-hello-twilio-module-in-packagejson"></a>Deklarerar hello Twilio-modul i package.json
Därefter måste toocreate en package.json toomanage våra nod modulen beroenden via [npm]. Vid hello samma nivå som hello `server.js` fil som du skapade i hello *Azure/node.js* självstudier, skapa en fil med namnet `package.json`.  I den här filen, placera hello följande:

```json
{
  "name": "application-name",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node server"
  },
  "dependencies": {
    "body-parser": "^1.16.1",
    "ejs": "^2.5.5",
    "errorhandler": "^1.5.0",
    "express": "^4.14.1",
    "morgan": "^1.8.1",
    "twilio": "^2.11.1"
  }
}
```

Detta försäkrar hello twilio-modulen som en beroende samt hello populära [Express webbramverk] [ express] och hello EJS mall-motorn.  OK, nu vi har allt klart - att skriva dags kod.

<a id="makecall"/>

## <a name="make-an-outbound-call"></a>Göra en utgående anrop
Nu ska vi skapa ett enkelt formulär som placerar anropet tooa flera vi väljer. Öppna `server.js`, och ange hello följande kod. Observera står det ”CHANGE_ME” - placera hello namnet på ditt azure-webbplatsen där:

```javascript
// Module dependencies
const express = require('express');
const path = require('path');
const http = require('http');
const twilio = require('twilio');
const logger = require('morgan');
const bodyParser = require('body-parser');
const errorHandler = require('errorhandler');
const accountSid = process.env.TWILIO_ACCOUNT_SID;
const authToken = process.env.TWILIO_AUTH_TOKEN;
// Create Express web application
const app = express();

// Express configuration
app.set('port', process.env.PORT || 3000);
app.set('views', __dirname + '/views');
app.set('view engine', 'ejs');
app.use(logger('tiny'));
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())
app.use(express.static(path.join(__dirname, 'public')));

if (app.get('env') !== 'production') {
  app.use(errorHandler());
}

// Render an HTML user interface for hello application's home page
app.get('/', (request, response) => response.render('index'));

// Handle hello form POST tooplace a call
app.post('/call', (request, response) => {
  var client = twilio(accountSid, authToken);

  client.makeCall({
    // make a call toothis number
    to:request.body.number,

    // Change tooa Twilio number you bought - see:
    // https://www.twilio.com/console/phone-numbers/incoming
    from:'+15558675309',

    // A URL in our app which generates TwiML
    // Change "CHANGE_ME" tooyour app's name
    url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
  }, () => {
      // Go back toohello home page
      response.redirect('/');
  });
});

// Generate TwiML toohandle an outbound call
app.post('/outbound_call', (request, response) => {
  var twiml = new twilio.TwimlResponse();

  // Say a message toohello call's receiver
  twiml.say('hello - thanks for checking out Twilio and Azure', {
      voice:'woman'
  });

  response.set('Content-Type', 'text/xml');
  response.send(twiml.toString());
});

// Start server
app.listen(app.get('port'), function(){
  console.log(`Express server listening on port ${app.get('port')}`);
});
```

Skapa sedan en katalog som kallas `views` – i den här katalogen, skapa en fil med namnet `index.ejs` med hello följande innehåll:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Twilio Test</title>
  <style>
    input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
  </style>
</head>
<body>
  <h1>Twilio Test</h1>
  <form action="/call" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input type="submit" value="Call hello number above"/>
  </form>
</body>
</html>
```

Nu distribuera din webbplats tooAzure och öppna hemmet. Du bör kunna tooenter telefonnumret i hello textfält, och får ett samtal från ditt Twilio-nummer!

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a>Skicka ett SMS-meddelande
Nu ska vi ställer in ett användargränssnitt och hantering av logik toosend ett textmeddelande. Öppna `server.js`, och Lägg till följande kod efter senaste hello-anropet för hello`app.post`:

```javascript
app.post('/sms', (request, response) => {
  const client = twilio(accountSid, authToken);

  client.sendSms({
      // send a text toothis number
      to:request.body.number,

      // A Twilio number you bought - see:
      // https://www.twilio.com/console/phone-numbers/incoming
      from:'+15558675309',

      // hello body of hello text message
      body: request.body.message

  }, () => {
      // Go back toohello home page
      response.redirect('/');
  });
});
```

I `views/index.ejs`, lägga till en annan form under hello först en toosubmit en siffra och ett SMS:

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message toosend" name="message"/>
  <br/>
  <input type="submit" value="Send text toohello number above"/>
</form>
```

Omdistribuera ditt program tooAzure och nu bör du kunna toosubmit som formuläret och skicka dig själv (eller någon av dina närmaste vänner) ett textmeddelande!

<a id="nextsteps"/>

## <a name="next-steps"></a>Nästa steg
Nu har du lärt dig hello grunderna i node.js och Twilio toobuild appar som kommunicerar. Men de här exemplen scratch knappt hello yta vad du kan göra med Twilio och node.js. Mer information med node.js Twilio utcheckning hello följande resurser:

* [Officiell modulen docs][docs]
* [Självstudiekurs om VoIP med node.js-program][voipnode]
* [Votr - en realtid SMS röstning program med node.js och CouchDB (tre delar)][votr]
* [Par programmering i hello webbläsare med node.js][pair]

Vi hoppas att du gillar hackningsförsök node.js och Twilio i Azure!

[purchase_phone]: https://www.twilio.com/console/phone-numbers/search
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: app-service-web/app-service-web-get-started-nodejs.md
[twilio_console]: https://www.twilio.com/console
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png
