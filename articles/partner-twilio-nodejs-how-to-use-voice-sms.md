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
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a><span data-ttu-id="1bf83-104">Med hjälp av Twilio för röst-, VoIP- och SMS-meddelanden i Azure</span><span class="sxs-lookup"><span data-stu-id="1bf83-104">Using Twilio for Voice, VoIP, and SMS Messaging in Azure</span></span>
<span data-ttu-id="1bf83-105">Den här guiden visar hur toobuild appar som kommunicerar med Twilio och node.js i Azure.</span><span class="sxs-lookup"><span data-stu-id="1bf83-105">This guide demonstrates how toobuild apps that communicate with Twilio and node.js on Azure.</span></span>

<a id="whatis"/>

## <a name="what-is-twilio"></a><span data-ttu-id="1bf83-106">Vad är Twilio?</span><span class="sxs-lookup"><span data-stu-id="1bf83-106">What is Twilio?</span></span>
<span data-ttu-id="1bf83-107">Twilio är en API-plattform som gör det lätt för utvecklare toomake och ta emot samtal, skicka och ta emot meddelanden och bädda in VoIP-anrop till webbläsarbaserad och egna mobila program.</span><span class="sxs-lookup"><span data-stu-id="1bf83-107">Twilio is an API platform that makes it easy for developers toomake and receive phone calls, send and receive text messages, and embed VoIP calling into browser-based and native mobile applications.</span></span> <span data-ttu-id="1bf83-108">Vi ska du kort gå igenom hur det fungerar innan du dyker.</span><span class="sxs-lookup"><span data-stu-id="1bf83-108">Let's briefly go over how this works before diving in.</span></span>

### <a name="receiving-calls-and-text-messages"></a><span data-ttu-id="1bf83-109">Ta emot samtal och textmeddelanden</span><span class="sxs-lookup"><span data-stu-id="1bf83-109">Receiving Calls and Text Messages</span></span>
<span data-ttu-id="1bf83-110">Twilio kan utvecklare för[köpa programmable telefonnummer] [ purchase_phone] som kan vara används tooboth skicka och ta emot samtal och textmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="1bf83-110">Twilio allows developers too[purchase programmable phone numbers][purchase_phone] which can be used tooboth send and receive calls and text messages.</span></span> <span data-ttu-id="1bf83-111">När flera Twilio tar emot ett inkommande samtal eller textmeddelande, skickar Twilio ditt webbprogram för en HTTP POST eller GET-begäran ber dig instruktioner för hur toohandle hello samtal eller textmeddelande.</span><span class="sxs-lookup"><span data-stu-id="1bf83-111">When a Twilio number receives an inbound call or text, Twilio will send your web application an HTTP POST or GET request, asking you for instructions on how toohandle hello call or text.</span></span> <span data-ttu-id="1bf83-112">Servern svarar Tootwilio's HTTP-begäran med [TwiML][twiml], en enkel uppsättning XML-taggar som innehåller instruktioner om hur toohandle samtal eller textmeddelande.</span><span class="sxs-lookup"><span data-stu-id="1bf83-112">Your server will respond tooTwilio's HTTP request with [TwiML][twiml], a simple set of XML tags that contain instructions on how toohandle a call or text.</span></span> <span data-ttu-id="1bf83-113">Exempel på TwiML ser vi bara en liten stund.</span><span class="sxs-lookup"><span data-stu-id="1bf83-113">We will see examples of TwiML in just a moment.</span></span>

### <a name="making-calls-and-sending-text-messages"></a><span data-ttu-id="1bf83-114">Anrop och skicka textmeddelanden</span><span class="sxs-lookup"><span data-stu-id="1bf83-114">Making Calls and Sending Text Messages</span></span>
<span data-ttu-id="1bf83-115">Genom att göra HTTP-begäranden toohello Twilio-webbtjänstens API kan utvecklare skicka textmeddelanden eller initiera utgående telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="1bf83-115">By making HTTP requests toohello Twilio web service API, developers can send text messages or initiate outbound phone calls.</span></span> <span data-ttu-id="1bf83-116">För utgående samtal måste hello developer också ange en URL som returnerar TwiML instruktioner för hur toohandle hello utgående anropa när den är ansluten.</span><span class="sxs-lookup"><span data-stu-id="1bf83-116">For outbound calls, hello developer must also specify a URL that returns TwiML instructions for how toohandle hello outbound call once it is connected.</span></span>

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a><span data-ttu-id="1bf83-117">Bädda in VoIP-funktioner i UI-kod (JavaScript, iOS eller Android)</span><span class="sxs-lookup"><span data-stu-id="1bf83-117">Embedding VoIP Capabilities in UI code (JavaScript, iOS, or Android)</span></span>
<span data-ttu-id="1bf83-118">Twilio ger en klientsidan SDK som kan göra alla skrivbord webbläsare, en iOS-app eller en Android-app till en VoIP-telefon.</span><span class="sxs-lookup"><span data-stu-id="1bf83-118">Twilio provides a client-side SDK which can turn any desktop web browser, iOS app, or Android app into a VoIP phone.</span></span> <span data-ttu-id="1bf83-119">I den här artikeln ska vi fokusera på hur toouse VoIP anrop i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="1bf83-119">In this article, we will focus on how toouse VoIP calling in hello browser.</span></span> <span data-ttu-id="1bf83-120">I tillägg toohello *Twilio JavaScript SDK* körs i hello webbläsare, ett program för serversidan (vår node.js-program) måste vara används tooissue ”kapaciteten token”-toohello JavaScript-klienten.</span><span class="sxs-lookup"><span data-stu-id="1bf83-120">In addition toohello *Twilio JavaScript SDK* running in hello browser, a server-side application (our node.js application) must be used tooissue a "capability token" toohello JavaScript client.</span></span> <span data-ttu-id="1bf83-121">Du kan läsa mer om hur du använder VoIP med node.js [på hello Twilio dev blogg][voipnode].</span><span class="sxs-lookup"><span data-stu-id="1bf83-121">You can read more about using VoIP with node.js [on hello Twilio dev blog][voipnode].</span></span>

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a><span data-ttu-id="1bf83-122">Registrera dig för Twilio (Microsoft rabatt)</span><span class="sxs-lookup"><span data-stu-id="1bf83-122">Sign Up For Twilio (Microsoft Discount)</span></span>
<span data-ttu-id="1bf83-123">Innan du använder Twilio-tjänster, måste du först [registrera dig för ett konto][signup].</span><span class="sxs-lookup"><span data-stu-id="1bf83-123">Before using Twilio services, you must first [sign up for an account][signup].</span></span> <span data-ttu-id="1bf83-124">Microsoft Azure-kunder får en särskild rabatt - [vara säker på att toosign här][signup]!</span><span class="sxs-lookup"><span data-stu-id="1bf83-124">Microsoft Azure customers receive a special discount - [be sure toosign up here][signup]!</span></span>

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a><span data-ttu-id="1bf83-125">Skapa och distribuera en node.js Azure-webbplats</span><span class="sxs-lookup"><span data-stu-id="1bf83-125">Create and Deploy a node.js Azure Website</span></span>
<span data-ttu-id="1bf83-126">Därefter behöver du toocreate en node.js-webbplats som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="1bf83-126">Next, you will need toocreate a node.js website running on Azure.</span></span> <span data-ttu-id="1bf83-127">[hello officiella dokumentation för att göra detta finns här][azure_new_site].</span><span class="sxs-lookup"><span data-stu-id="1bf83-127">[hello official documentation for doing this is located here][azure_new_site].</span></span> <span data-ttu-id="1bf83-128">På en hög nivå, kommer du att göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="1bf83-128">At a high level, you will be doing hello following:</span></span>

* <span data-ttu-id="1bf83-129">Registrera dig för ett Azure-konto om du inte redan har en</span><span class="sxs-lookup"><span data-stu-id="1bf83-129">Signing up for an Azure account, if you don't have one already</span></span>
* <span data-ttu-id="1bf83-130">Med hjälp av hello Azure admin console toocreate en ny webbplats</span><span class="sxs-lookup"><span data-stu-id="1bf83-130">Using hello Azure admin console toocreate a new website</span></span>
* <span data-ttu-id="1bf83-131">Att lägga till stöd för källa kontrollen (vi antar som du använde git)</span><span class="sxs-lookup"><span data-stu-id="1bf83-131">Adding source control support (we will assume you used git)</span></span>
* <span data-ttu-id="1bf83-132">Skapa en fil `server.js` med en enkel node.js-webbapp</span><span class="sxs-lookup"><span data-stu-id="1bf83-132">Creating a file `server.js` with a simple node.js web application</span></span>
* <span data-ttu-id="1bf83-133">Distribuera den här enkla program tooAzure</span><span class="sxs-lookup"><span data-stu-id="1bf83-133">Deploying this simple application tooAzure</span></span>

<a id="twiliomodule"/>

## <a name="configure-hello-twilio-module"></a><span data-ttu-id="1bf83-134">Konfigurera hello Twilio-modul</span><span class="sxs-lookup"><span data-stu-id="1bf83-134">Configure hello Twilio Module</span></span>
<span data-ttu-id="1bf83-135">Därefter börjar vi toowrite en enkel node.js-program som använder hello Twilio-API.</span><span class="sxs-lookup"><span data-stu-id="1bf83-135">Next, we will begin toowrite a simple node.js application which makes use of hello Twilio API.</span></span> <span data-ttu-id="1bf83-136">Innan vi kan behöver vi tooconfigure våra Twilio-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1bf83-136">Before we begin, we need tooconfigure our Twilio account credentials.</span></span>

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a><span data-ttu-id="1bf83-137">Konfigurera Twilio-autentiseringsuppgifter i systemmiljövariabler</span><span class="sxs-lookup"><span data-stu-id="1bf83-137">Configuring Twilio Credentials in System Environment Variables</span></span>
<span data-ttu-id="1bf83-138">I ordning toomake autentiseras förfrågningar mot hello Twilio-serverdel behöver vi våra kontots SID och auth token, vilken funktion som hello användarnamn och lösenord som angetts för våra Twilio-konto.</span><span class="sxs-lookup"><span data-stu-id="1bf83-138">In order toomake authenticated requests against hello Twilio back end, we need our account SID and auth token, which function as hello username and password set for our Twilio account.</span></span> <span data-ttu-id="1bf83-139">hello säkraste sättet tooconfigure dem för användning med hello nod modul i Azure är via systemmiljövariabler som du anger direkt i hello Azure-administrationskonsolen.</span><span class="sxs-lookup"><span data-stu-id="1bf83-139">hello most secure way tooconfigure these for use with hello node module in Azure is via system environment variables, which you can set directly in hello Azure admin console.</span></span>

<span data-ttu-id="1bf83-140">Välj din node.js-webbplats och på hello ”konfigurera” länk.</span><span class="sxs-lookup"><span data-stu-id="1bf83-140">Select your node.js website, and click hello "CONFIGURE" link.</span></span>  <span data-ttu-id="1bf83-141">Om du bläddra nedåt för visas ett område där du kan ange konfigurationsegenskaper för programmet.</span><span class="sxs-lookup"><span data-stu-id="1bf83-141">If you scroll down a bit, you will see an area where you can set configuration properties for your application.</span></span>  <span data-ttu-id="1bf83-142">Ange autentiseringsuppgifterna för ditt Twilio-konto ([hittades på konsolen Twilio][twilio_console]) enligt – se till att tooname dem `TWILIO_ACCOUNT_SID` och `TWILIO_AUTH_TOKEN`respektive:</span><span class="sxs-lookup"><span data-stu-id="1bf83-142">Enter your Twilio account credentials ([found on your Twilio Console][twilio_console]) as shown - make sure tooname them `TWILIO_ACCOUNT_SID` and `TWILIO_AUTH_TOKEN`, respectively:</span></span>

![Azure-administrationskonsolen][azure-admin-console]

<span data-ttu-id="1bf83-144">När du har konfigurerat dessa variabler kan du starta om ditt program i hello Azure-konsolen.</span><span class="sxs-lookup"><span data-stu-id="1bf83-144">Once you have configured these variables, restart your application in hello Azure console.</span></span>

### <a name="declaring-hello-twilio-module-in-packagejson"></a><span data-ttu-id="1bf83-145">Deklarerar hello Twilio-modul i package.json</span><span class="sxs-lookup"><span data-stu-id="1bf83-145">Declaring hello Twilio module in package.json</span></span>
<span data-ttu-id="1bf83-146">Därefter måste toocreate en package.json toomanage våra nod modulen beroenden via [npm].</span><span class="sxs-lookup"><span data-stu-id="1bf83-146">Next, we need toocreate a package.json toomanage our node module dependencies via [npm].</span></span> <span data-ttu-id="1bf83-147">Vid hello samma nivå som hello `server.js` fil som du skapade i hello *Azure/node.js* självstudier, skapa en fil med namnet `package.json`.</span><span class="sxs-lookup"><span data-stu-id="1bf83-147">At hello same level as hello `server.js` file you created in hello *Azure/node.js* tutorial, create a file named `package.json`.</span></span>  <span data-ttu-id="1bf83-148">I den här filen, placera hello följande:</span><span class="sxs-lookup"><span data-stu-id="1bf83-148">Inside this file, place hello following:</span></span>

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

<span data-ttu-id="1bf83-149">Detta försäkrar hello twilio-modulen som en beroende samt hello populära [Express webbramverk] [ express] och hello EJS mall-motorn.</span><span class="sxs-lookup"><span data-stu-id="1bf83-149">This declares hello twilio module as a dependency, as well as hello popular [Express web framework][express] and hello EJS template engine.</span></span>  <span data-ttu-id="1bf83-150">OK, nu vi har allt klart - att skriva dags kod.</span><span class="sxs-lookup"><span data-stu-id="1bf83-150">Okay, now we're all set - let's write some code!</span></span>

<a id="makecall"/>

## <a name="make-an-outbound-call"></a><span data-ttu-id="1bf83-151">Göra en utgående anrop</span><span class="sxs-lookup"><span data-stu-id="1bf83-151">Make an Outbound Call</span></span>
<span data-ttu-id="1bf83-152">Nu ska vi skapa ett enkelt formulär som placerar anropet tooa flera vi väljer.</span><span class="sxs-lookup"><span data-stu-id="1bf83-152">Let's create a simple form that will place a call tooa number we choose.</span></span> <span data-ttu-id="1bf83-153">Öppna `server.js`, och ange hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="1bf83-153">Open up `server.js`, and enter hello following code.</span></span> <span data-ttu-id="1bf83-154">Observera står det ”CHANGE_ME” - placera hello namnet på ditt azure-webbplatsen där:</span><span class="sxs-lookup"><span data-stu-id="1bf83-154">Note where it says "CHANGE_ME" - put hello name of your azure website there:</span></span>

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

<span data-ttu-id="1bf83-155">Skapa sedan en katalog som kallas `views` – i den här katalogen, skapa en fil med namnet `index.ejs` med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="1bf83-155">Next, create a directory called `views` - inside this directory, create a file named `index.ejs` with hello following contents:</span></span>

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

<span data-ttu-id="1bf83-156">Nu distribuera din webbplats tooAzure och öppna hemmet.</span><span class="sxs-lookup"><span data-stu-id="1bf83-156">Now, deploy your website tooAzure and open your home.</span></span> <span data-ttu-id="1bf83-157">Du bör kunna tooenter telefonnumret i hello textfält, och får ett samtal från ditt Twilio-nummer!</span><span class="sxs-lookup"><span data-stu-id="1bf83-157">You should be able tooenter your phone number in hello text field, and receive a call from your Twilio number!</span></span>

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a><span data-ttu-id="1bf83-158">Skicka ett SMS-meddelande</span><span class="sxs-lookup"><span data-stu-id="1bf83-158">Send an SMS Message</span></span>
<span data-ttu-id="1bf83-159">Nu ska vi ställer in ett användargränssnitt och hantering av logik toosend ett textmeddelande.</span><span class="sxs-lookup"><span data-stu-id="1bf83-159">Now, let's set up a user interface and form handling logic toosend a text message.</span></span> <span data-ttu-id="1bf83-160">Öppna `server.js`, och Lägg till följande kod efter senaste hello-anropet för hello`app.post`:</span><span class="sxs-lookup"><span data-stu-id="1bf83-160">Open up `server.js`, and add hello following code after hello last call too`app.post`:</span></span>

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

<span data-ttu-id="1bf83-161">I `views/index.ejs`, lägga till en annan form under hello först en toosubmit en siffra och ett SMS:</span><span class="sxs-lookup"><span data-stu-id="1bf83-161">In `views/index.ejs`, add another form under hello first one toosubmit a number and a text message:</span></span>

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message toosend" name="message"/>
  <br/>
  <input type="submit" value="Send text toohello number above"/>
</form>
```

<span data-ttu-id="1bf83-162">Omdistribuera ditt program tooAzure och nu bör du kunna toosubmit som formuläret och skicka dig själv (eller någon av dina närmaste vänner) ett textmeddelande!</span><span class="sxs-lookup"><span data-stu-id="1bf83-162">Re-deploy your application tooAzure, and you should now be able toosubmit that form and send yourself (or any of your closest friends) a text message!</span></span>

<a id="nextsteps"/>

## <a name="next-steps"></a><span data-ttu-id="1bf83-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1bf83-163">Next Steps</span></span>
<span data-ttu-id="1bf83-164">Nu har du lärt dig hello grunderna i node.js och Twilio toobuild appar som kommunicerar.</span><span class="sxs-lookup"><span data-stu-id="1bf83-164">You have now learned hello basics of using node.js and Twilio toobuild apps that communicate.</span></span> <span data-ttu-id="1bf83-165">Men de här exemplen scratch knappt hello yta vad du kan göra med Twilio och node.js.</span><span class="sxs-lookup"><span data-stu-id="1bf83-165">But these examples barely scratch hello surface of what's possible with Twilio and node.js.</span></span> <span data-ttu-id="1bf83-166">Mer information med node.js Twilio utcheckning hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="1bf83-166">For more information using Twilio with node.js, check out hello following resources:</span></span>

* <span data-ttu-id="1bf83-167">[Officiell modulen docs][docs]</span><span class="sxs-lookup"><span data-stu-id="1bf83-167">[Official module docs][docs]</span></span>
* <span data-ttu-id="1bf83-168">[Självstudiekurs om VoIP med node.js-program][voipnode]</span><span class="sxs-lookup"><span data-stu-id="1bf83-168">[Tutorial on VoIP with node.js applications][voipnode]</span></span>
* <span data-ttu-id="1bf83-169">[Votr - en realtid SMS röstning program med node.js och CouchDB (tre delar)][votr]</span><span class="sxs-lookup"><span data-stu-id="1bf83-169">[Votr - a real-time SMS voting application with node.js and CouchDB (three parts)][votr]</span></span>
* <span data-ttu-id="1bf83-170">[Par programmering i hello webbläsare med node.js][pair]</span><span class="sxs-lookup"><span data-stu-id="1bf83-170">[Pair programming in hello browser with node.js][pair]</span></span>

<span data-ttu-id="1bf83-171">Vi hoppas att du gillar hackningsförsök node.js och Twilio i Azure!</span><span class="sxs-lookup"><span data-stu-id="1bf83-171">We hope you love hacking node.js and Twilio on Azure!</span></span>

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
