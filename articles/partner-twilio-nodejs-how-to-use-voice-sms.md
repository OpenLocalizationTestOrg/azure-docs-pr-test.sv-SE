---
title: "Med hjälp av Twilio för röst-, VoIP- och SMS-meddelanden i Azure"
description: "Lär dig att ringa ett telefonsamtal och skicka ett SMS-meddelande med Twilio-API-tjänsten på Azure. Kodexempel som skrivits i Node.js."
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
ms.openlocfilehash: 44ec97812130d41d75be98fc8e2d846b7cb5c913
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a><span data-ttu-id="a28ec-104">Med hjälp av Twilio för röst-, VoIP- och SMS-meddelanden i Azure</span><span class="sxs-lookup"><span data-stu-id="a28ec-104">Using Twilio for Voice, VoIP, and SMS Messaging in Azure</span></span>
<span data-ttu-id="a28ec-105">Den här guiden visar hur man skapar appar som kommunicerar med Twilio och node.js i Azure.</span><span class="sxs-lookup"><span data-stu-id="a28ec-105">This guide demonstrates how to build apps that communicate with Twilio and node.js on Azure.</span></span>

<a id="whatis"/>

## <a name="what-is-twilio"></a><span data-ttu-id="a28ec-106">Vad är Twilio?</span><span class="sxs-lookup"><span data-stu-id="a28ec-106">What is Twilio?</span></span>
<span data-ttu-id="a28ec-107">Twilio är en API-plattform som gör det enkelt för utvecklare att göra och ta emot samtal, skicka och ta emot meddelanden och bädda in VoIP-anrop till webbläsarbaserad och egna mobila program.</span><span class="sxs-lookup"><span data-stu-id="a28ec-107">Twilio is an API platform that makes it easy for developers to make and receive phone calls, send and receive text messages, and embed VoIP calling into browser-based and native mobile applications.</span></span> <span data-ttu-id="a28ec-108">Vi ska du kort gå igenom hur det fungerar innan du dyker.</span><span class="sxs-lookup"><span data-stu-id="a28ec-108">Let's briefly go over how this works before diving in.</span></span>

### <a name="receiving-calls-and-text-messages"></a><span data-ttu-id="a28ec-109">Ta emot samtal och textmeddelanden</span><span class="sxs-lookup"><span data-stu-id="a28ec-109">Receiving Calls and Text Messages</span></span>
<span data-ttu-id="a28ec-110">Twilio kan utvecklare [köpa programmable telefonnummer] [ purchase_phone] som kan användas både skicka och ta emot samtal och textmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="a28ec-110">Twilio allows developers to [purchase programmable phone numbers][purchase_phone] which can be used to both send and receive calls and text messages.</span></span> <span data-ttu-id="a28ec-111">När flera Twilio tar emot ett inkommande samtal eller textmeddelande, skickar Twilio ditt webbprogram för en HTTP POST eller GET-begäran ber dig instruktioner om hur du hanterar samtal eller textmeddelande.</span><span class="sxs-lookup"><span data-stu-id="a28ec-111">When a Twilio number receives an inbound call or text, Twilio will send your web application an HTTP POST or GET request, asking you for instructions on how to handle the call or text.</span></span> <span data-ttu-id="a28ec-112">Servern svarar på Twilio's HTTP-begäran med [TwiML][twiml], en enkel uppsättning XML-taggar som innehåller instruktioner om hur du hanterar ett samtal eller textmeddelande.</span><span class="sxs-lookup"><span data-stu-id="a28ec-112">Your server will respond to Twilio's HTTP request with [TwiML][twiml], a simple set of XML tags that contain instructions on how to handle a call or text.</span></span> <span data-ttu-id="a28ec-113">Exempel på TwiML ser vi bara en liten stund.</span><span class="sxs-lookup"><span data-stu-id="a28ec-113">We will see examples of TwiML in just a moment.</span></span>

### <a name="making-calls-and-sending-text-messages"></a><span data-ttu-id="a28ec-114">Anrop och skicka textmeddelanden</span><span class="sxs-lookup"><span data-stu-id="a28ec-114">Making Calls and Sending Text Messages</span></span>
<span data-ttu-id="a28ec-115">Genom att göra HTTP-begäranden till Twilio-webbtjänstens API kan utvecklare skicka textmeddelanden eller initiera utgående telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="a28ec-115">By making HTTP requests to the Twilio web service API, developers can send text messages or initiate outbound phone calls.</span></span> <span data-ttu-id="a28ec-116">För utgående samtal måste utvecklaren också ange en URL som returnerar TwiML instruktioner för hur du hanterar utgående samtal när den är ansluten.</span><span class="sxs-lookup"><span data-stu-id="a28ec-116">For outbound calls, the developer must also specify a URL that returns TwiML instructions for how to handle the outbound call once it is connected.</span></span>

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a><span data-ttu-id="a28ec-117">Bädda in VoIP-funktioner i UI-kod (JavaScript, iOS eller Android)</span><span class="sxs-lookup"><span data-stu-id="a28ec-117">Embedding VoIP Capabilities in UI code (JavaScript, iOS, or Android)</span></span>
<span data-ttu-id="a28ec-118">Twilio ger en klientsidan SDK som kan göra alla skrivbord webbläsare, en iOS-app eller en Android-app till en VoIP-telefon.</span><span class="sxs-lookup"><span data-stu-id="a28ec-118">Twilio provides a client-side SDK which can turn any desktop web browser, iOS app, or Android app into a VoIP phone.</span></span> <span data-ttu-id="a28ec-119">I den här artikeln fokuseras på hur du använder VoIP anrop i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="a28ec-119">In this article, we will focus on how to use VoIP calling in the browser.</span></span> <span data-ttu-id="a28ec-120">Förutom den *Twilio JavaScript SDK* körs i webbläsaren, ett program för serversidan (vår node.js-program) måste användas för att utfärda en ”kapaciteten token” till JavaScript-klienten.</span><span class="sxs-lookup"><span data-stu-id="a28ec-120">In addition to the *Twilio JavaScript SDK* running in the browser, a server-side application (our node.js application) must be used to issue a "capability token" to the JavaScript client.</span></span> <span data-ttu-id="a28ec-121">Du kan läsa mer om hur du använder VoIP med node.js [på Twilio dev bloggen][voipnode].</span><span class="sxs-lookup"><span data-stu-id="a28ec-121">You can read more about using VoIP with node.js [on the Twilio dev blog][voipnode].</span></span>

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a><span data-ttu-id="a28ec-122">Registrera dig för Twilio (Microsoft rabatt)</span><span class="sxs-lookup"><span data-stu-id="a28ec-122">Sign Up For Twilio (Microsoft Discount)</span></span>
<span data-ttu-id="a28ec-123">Innan du använder Twilio-tjänster, måste du först [registrera dig för ett konto][signup].</span><span class="sxs-lookup"><span data-stu-id="a28ec-123">Before using Twilio services, you must first [sign up for an account][signup].</span></span> <span data-ttu-id="a28ec-124">Microsoft Azure-kunder får en särskild rabatt - [måste du registrera dig här][signup]!</span><span class="sxs-lookup"><span data-stu-id="a28ec-124">Microsoft Azure customers receive a special discount - [be sure to sign up here][signup]!</span></span>

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a><span data-ttu-id="a28ec-125">Skapa och distribuera en node.js Azure-webbplats</span><span class="sxs-lookup"><span data-stu-id="a28ec-125">Create and Deploy a node.js Azure Website</span></span>
<span data-ttu-id="a28ec-126">Därefter behöver du skapa en node.js-webbplats som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="a28ec-126">Next, you will need to create a node.js website running on Azure.</span></span> <span data-ttu-id="a28ec-127">[Den officiella dokumentationen för att göra detta finns här][azure_new_site].</span><span class="sxs-lookup"><span data-stu-id="a28ec-127">[The official documentation for doing this is located here][azure_new_site].</span></span> <span data-ttu-id="a28ec-128">På en hög nivå, kommer du att göra följande:</span><span class="sxs-lookup"><span data-stu-id="a28ec-128">At a high level, you will be doing the following:</span></span>

* <span data-ttu-id="a28ec-129">Registrera dig för ett Azure-konto om du inte redan har en</span><span class="sxs-lookup"><span data-stu-id="a28ec-129">Signing up for an Azure account, if you don't have one already</span></span>
* <span data-ttu-id="a28ec-130">Använder Azure-administratörskonsolen för att skapa en ny webbplats</span><span class="sxs-lookup"><span data-stu-id="a28ec-130">Using the Azure admin console to create a new website</span></span>
* <span data-ttu-id="a28ec-131">Att lägga till stöd för källa kontrollen (vi antar som du använde git)</span><span class="sxs-lookup"><span data-stu-id="a28ec-131">Adding source control support (we will assume you used git)</span></span>
* <span data-ttu-id="a28ec-132">Skapa en fil `server.js` med en enkel node.js-webbapp</span><span class="sxs-lookup"><span data-stu-id="a28ec-132">Creating a file `server.js` with a simple node.js web application</span></span>
* <span data-ttu-id="a28ec-133">Distribuera detta enkla program till Azure</span><span class="sxs-lookup"><span data-stu-id="a28ec-133">Deploying this simple application to Azure</span></span>

<a id="twiliomodule"/>

## <a name="configure-the-twilio-module"></a><span data-ttu-id="a28ec-134">Konfigurera Twilio-modul</span><span class="sxs-lookup"><span data-stu-id="a28ec-134">Configure the Twilio Module</span></span>
<span data-ttu-id="a28ec-135">Vi börjar sedan skriva en enkel node.js-program som använder Twilio-API.</span><span class="sxs-lookup"><span data-stu-id="a28ec-135">Next, we will begin to write a simple node.js application which makes use of the Twilio API.</span></span> <span data-ttu-id="a28ec-136">Innan vi kan behöver vi konfigurera våra Twilio-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a28ec-136">Before we begin, we need to configure our Twilio account credentials.</span></span>

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a><span data-ttu-id="a28ec-137">Konfigurera Twilio-autentiseringsuppgifter i systemmiljövariabler</span><span class="sxs-lookup"><span data-stu-id="a28ec-137">Configuring Twilio Credentials in System Environment Variables</span></span>
<span data-ttu-id="a28ec-138">Vi behöver för att göra autentiserade begäranden mot Twilio serverdelen våra kontots SID och auth token, vilken funktion som användarnamn och lösenord som angetts för våra Twilio-konto.</span><span class="sxs-lookup"><span data-stu-id="a28ec-138">In order to make authenticated requests against the Twilio back end, we need our account SID and auth token, which function as the username and password set for our Twilio account.</span></span> <span data-ttu-id="a28ec-139">Det är det säkraste sättet att konfigurera dem för användning med modulen noden i Azure via systemmiljövariabler som du anger direkt i Azure-administrationskonsolen.</span><span class="sxs-lookup"><span data-stu-id="a28ec-139">The most secure way to configure these for use with the node module in Azure is via system environment variables, which you can set directly in the Azure admin console.</span></span>

<span data-ttu-id="a28ec-140">Välj din node.js-webbplats och klicka på länken ”konfigurera”.</span><span class="sxs-lookup"><span data-stu-id="a28ec-140">Select your node.js website, and click the "CONFIGURE" link.</span></span>  <span data-ttu-id="a28ec-141">Om du bläddra nedåt för visas ett område där du kan ange konfigurationsegenskaper för programmet.</span><span class="sxs-lookup"><span data-stu-id="a28ec-141">If you scroll down a bit, you will see an area where you can set configuration properties for your application.</span></span>  <span data-ttu-id="a28ec-142">Ange autentiseringsuppgifterna för ditt Twilio-konto ([hittades på konsolen Twilio][twilio_console]) enligt – se till att ge dem `TWILIO_ACCOUNT_SID` och `TWILIO_AUTH_TOKEN`respektive:</span><span class="sxs-lookup"><span data-stu-id="a28ec-142">Enter your Twilio account credentials ([found on your Twilio Console][twilio_console]) as shown - make sure to name them `TWILIO_ACCOUNT_SID` and `TWILIO_AUTH_TOKEN`, respectively:</span></span>

![Azure-administrationskonsolen][azure-admin-console]

<span data-ttu-id="a28ec-144">När du har konfigurerat dessa variabler kan du starta om ditt program i Azure-konsolen.</span><span class="sxs-lookup"><span data-stu-id="a28ec-144">Once you have configured these variables, restart your application in the Azure console.</span></span>

### <a name="declaring-the-twilio-module-in-packagejson"></a><span data-ttu-id="a28ec-145">Deklarerar Twilio-modulen i package.json</span><span class="sxs-lookup"><span data-stu-id="a28ec-145">Declaring the Twilio module in package.json</span></span>
<span data-ttu-id="a28ec-146">Därefter måste vi skapa en package.json om du vill hantera vår modulen beroenden för noden via [npm].</span><span class="sxs-lookup"><span data-stu-id="a28ec-146">Next, we need to create a package.json to manage our node module dependencies via [npm].</span></span> <span data-ttu-id="a28ec-147">På samma nivå som den `server.js` fil du skapade i den *Azure/node.js* självstudier, skapa en fil med namnet `package.json`.</span><span class="sxs-lookup"><span data-stu-id="a28ec-147">At the same level as the `server.js` file you created in the *Azure/node.js* tutorial, create a file named `package.json`.</span></span>  <span data-ttu-id="a28ec-148">I den här filen, placera på följande:</span><span class="sxs-lookup"><span data-stu-id="a28ec-148">Inside this file, place the following:</span></span>

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

<span data-ttu-id="a28ec-149">Detta försäkrar modulen twilio som ett beroende som den populära [Express webbramverk] [ express] och EJS mall-motorn.</span><span class="sxs-lookup"><span data-stu-id="a28ec-149">This declares the twilio module as a dependency, as well as the popular [Express web framework][express] and the EJS template engine.</span></span>  <span data-ttu-id="a28ec-150">OK, nu vi har allt klart - att skriva dags kod.</span><span class="sxs-lookup"><span data-stu-id="a28ec-150">Okay, now we're all set - let's write some code!</span></span>

<a id="makecall"/>

## <a name="make-an-outbound-call"></a><span data-ttu-id="a28ec-151">Göra en utgående anrop</span><span class="sxs-lookup"><span data-stu-id="a28ec-151">Make an Outbound Call</span></span>
<span data-ttu-id="a28ec-152">Nu ska vi skapa ett enkelt formulär som placerar ett anrop till ett antal vi väljer.</span><span class="sxs-lookup"><span data-stu-id="a28ec-152">Let's create a simple form that will place a call to a number we choose.</span></span> <span data-ttu-id="a28ec-153">Öppna `server.js`, och ange följande kod.</span><span class="sxs-lookup"><span data-stu-id="a28ec-153">Open up `server.js`, and enter the following code.</span></span> <span data-ttu-id="a28ec-154">Observera står det ”CHANGE_ME” - placera namnet på ditt azure-webbplatsen där:</span><span class="sxs-lookup"><span data-stu-id="a28ec-154">Note where it says "CHANGE_ME" - put the name of your azure website there:</span></span>

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

// Render an HTML user interface for the application's home page
app.get('/', (request, response) => response.render('index'));

// Handle the form POST to place a call
app.post('/call', (request, response) => {
  var client = twilio(accountSid, authToken);

  client.makeCall({
    // make a call to this number
    to:request.body.number,

    // Change to a Twilio number you bought - see:
    // https://www.twilio.com/console/phone-numbers/incoming
    from:'+15558675309',

    // A URL in our app which generates TwiML
    // Change "CHANGE_ME" to your app's name
    url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
  }, () => {
      // Go back to the home page
      response.redirect('/');
  });
});

// Generate TwiML to handle an outbound call
app.post('/outbound_call', (request, response) => {
  var twiml = new twilio.TwimlResponse();

  // Say a message to the call's receiver
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

<span data-ttu-id="a28ec-155">Skapa sedan en katalog som kallas `views` – i den här katalogen, skapa en fil med namnet `index.ejs` med följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="a28ec-155">Next, create a directory called `views` - inside this directory, create a file named `index.ejs` with the following contents:</span></span>

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
      <input type="submit" value="Call the number above"/>
  </form>
</body>
</html>
```

<span data-ttu-id="a28ec-156">Nu distribuera din webbplats till Azure och öppna hemmet.</span><span class="sxs-lookup"><span data-stu-id="a28ec-156">Now, deploy your website to Azure and open your home.</span></span> <span data-ttu-id="a28ec-157">Du ska kunna ange ditt telefonnummer i textfältet och får ett samtal från ditt Twilio-nummer!</span><span class="sxs-lookup"><span data-stu-id="a28ec-157">You should be able to enter your phone number in the text field, and receive a call from your Twilio number!</span></span>

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a><span data-ttu-id="a28ec-158">Skicka ett SMS-meddelande</span><span class="sxs-lookup"><span data-stu-id="a28ec-158">Send an SMS Message</span></span>
<span data-ttu-id="a28ec-159">Nu ska vi ställer in ett användargränssnitt och hantering av logik att skicka ett SMS.</span><span class="sxs-lookup"><span data-stu-id="a28ec-159">Now, let's set up a user interface and form handling logic to send a text message.</span></span> <span data-ttu-id="a28ec-160">Öppna `server.js`, och Lägg till följande kod efter det senaste anropet till `app.post`:</span><span class="sxs-lookup"><span data-stu-id="a28ec-160">Open up `server.js`, and add the following code after the last call to `app.post`:</span></span>

```javascript
app.post('/sms', (request, response) => {
  const client = twilio(accountSid, authToken);

  client.sendSms({
      // send a text to this number
      to:request.body.number,

      // A Twilio number you bought - see:
      // https://www.twilio.com/console/phone-numbers/incoming
      from:'+15558675309',

      // The body of the text message
      body: request.body.message

  }, () => {
      // Go back to the home page
      response.redirect('/');
  });
});
```

<span data-ttu-id="a28ec-161">I `views/index.ejs`, lägga till en annan form under den första som kan skicka en siffra och ett SMS:</span><span class="sxs-lookup"><span data-stu-id="a28ec-161">In `views/index.ejs`, add another form under the first one to submit a number and a text message:</span></span>

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message to send" name="message"/>
  <br/>
  <input type="submit" value="Send text to the number above"/>
</form>
```

<span data-ttu-id="a28ec-162">Distribuera programmet till Azure och du ska nu kunna skicka som formuläret och skicka dig själv (eller någon av dina närmaste vänner) ett textmeddelande!</span><span class="sxs-lookup"><span data-stu-id="a28ec-162">Re-deploy your application to Azure, and you should now be able to submit that form and send yourself (or any of your closest friends) a text message!</span></span>

<a id="nextsteps"/>

## <a name="next-steps"></a><span data-ttu-id="a28ec-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a28ec-163">Next Steps</span></span>
<span data-ttu-id="a28ec-164">Nu har du lärt dig hur du använder node.js och Twilio för att skapa appar som kommunicerar.</span><span class="sxs-lookup"><span data-stu-id="a28ec-164">You have now learned the basics of using node.js and Twilio to build apps that communicate.</span></span> <span data-ttu-id="a28ec-165">Men de här exemplen scratch knappt yta vad du kan göra med Twilio och node.js.</span><span class="sxs-lookup"><span data-stu-id="a28ec-165">But these examples barely scratch the surface of what's possible with Twilio and node.js.</span></span> <span data-ttu-id="a28ec-166">Mer information med node.js Twilio ta en titt i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="a28ec-166">For more information using Twilio with node.js, check out the following resources:</span></span>

* <span data-ttu-id="a28ec-167">[Officiell modulen docs][docs]</span><span class="sxs-lookup"><span data-stu-id="a28ec-167">[Official module docs][docs]</span></span>
* <span data-ttu-id="a28ec-168">[Självstudiekurs om VoIP med node.js-program][voipnode]</span><span class="sxs-lookup"><span data-stu-id="a28ec-168">[Tutorial on VoIP with node.js applications][voipnode]</span></span>
* <span data-ttu-id="a28ec-169">[Votr - en realtid SMS röstning program med node.js och CouchDB (tre delar)][votr]</span><span class="sxs-lookup"><span data-stu-id="a28ec-169">[Votr - a real-time SMS voting application with node.js and CouchDB (three parts)][votr]</span></span>
* <span data-ttu-id="a28ec-170">[Par programmering i webbläsaren med node.js][pair]</span><span class="sxs-lookup"><span data-stu-id="a28ec-170">[Pair programming in the browser with node.js][pair]</span></span>

<span data-ttu-id="a28ec-171">Vi hoppas att du gillar hackningsförsök node.js och Twilio i Azure!</span><span class="sxs-lookup"><span data-stu-id="a28ec-171">We hope you love hacking node.js and Twilio on Azure!</span></span>

[purchase_phone]: https://www.twilio.com/console/phone-numbers/search
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: app-service-web/app-service-web-get-started-nodejs.md
[twilio_console]: https://www.twilio.com/console
<span data-ttu-id="a28ec-172">[npm]: http://npmjs.org</span><span class="sxs-lookup"><span data-stu-id="a28ec-172">[npm]: http://npmjs.org</span></span>
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png
