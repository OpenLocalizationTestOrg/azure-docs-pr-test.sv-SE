---
title: "aaaHow tooUse Twilio för röst- och SMS (Ruby) | Microsoft Docs"
description: "Lär dig hur toomake ett telefonsamtal och skicka ett SMS-meddelandet med hello Twilio API-tjänsten på Azure. Kodexempel som skrivits i Ruby."
services: 
documentationcenter: ruby
author: devinrader
manager: twilio
editor: 
ms.assetid: 60e512f6-fa47-47c0-aedc-f19bb72a1158
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 11/25/2014
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: aca5ccb4663ff03c9c1f39c848469415f06dfb45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-ruby"></a><span data-ttu-id="20fa5-104">Hur tooUse Twilio för röst- och SMS-funktioner i Ruby</span><span class="sxs-lookup"><span data-stu-id="20fa5-104">How tooUse Twilio for Voice and SMS Capabilities in Ruby</span></span>
<span data-ttu-id="20fa5-105">Den här guiden visar hur tooperform vanliga programmeringsuppgifter med hello Twilio API-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="20fa5-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="20fa5-106">hello-scenarier som tas upp inkluderar att ringa och ett kort meddelande (SMS Service)-meddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="20fa5-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="20fa5-107">Mer information om Twilio och använder röst- och SMS i dina program finns hello [nästa steg](#NextSteps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="20fa5-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="20fa5-108"><a id="WhatIs"></a>Vad är Twilio?</span><span class="sxs-lookup"><span data-stu-id="20fa5-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="20fa5-109">Twilio är en telefoni webbtjänst API som kan du använda din befintliga web språk och kunskaper toobuild röst och SMS-program.</span><span class="sxs-lookup"><span data-stu-id="20fa5-109">Twilio is a telephony web-service API that lets you use your existing web languages and skills toobuild voice and SMS applications.</span></span> <span data-ttu-id="20fa5-110">Twilio är en från tredje part (inte en funktion i Azure och inte en Microsoft-produkt).</span><span class="sxs-lookup"><span data-stu-id="20fa5-110">Twilio is a third-party service (not an Azure feature and not a Microsoft product).</span></span>

<span data-ttu-id="20fa5-111">**Twilio röst** kan ditt program toomake och ta emot telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="20fa5-111">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span> <span data-ttu-id="20fa5-112">**Twilio SMS** kan ditt program toomake och ta emot SMS-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="20fa5-112">**Twilio SMS** allows your applications toomake and receive SMS messages.</span></span> <span data-ttu-id="20fa5-113">**Twilio klienten** gör att dina program tooenable röstkommunikation via befintliga Internet-anslutningar, inklusive mobila anslutningar.</span><span class="sxs-lookup"><span data-stu-id="20fa5-113">**Twilio Client** allows your applications tooenable voice communication using existing Internet connections, including mobile connections.</span></span>

## <span data-ttu-id="20fa5-114"><a id="Pricing"></a>Priser för Twilio och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="20fa5-114"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="20fa5-115">Information om priser Twilio finns på [Twilio priser][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="20fa5-115">Information about Twilio pricing is available at [Twilio Pricing][twilio_pricing].</span></span> <span data-ttu-id="20fa5-116">Azure-kunder får en [specialerbjudande][special_offer]: en kredit på 1000 texter eller 1000 inkommande minuter.</span><span class="sxs-lookup"><span data-stu-id="20fa5-116">Azure customers receive a [special offer][special_offer]: a free credit of 1000 texts or 1000 inbound minutes.</span></span> <span data-ttu-id="20fa5-117">toosign för detta erbjudande eller få mer information, besök [http://ahoy.twilio.com/azure][special_offer].</span><span class="sxs-lookup"><span data-stu-id="20fa5-117">toosign up for this offer or get more information, please visit [http://ahoy.twilio.com/azure][special_offer].</span></span>  

## <span data-ttu-id="20fa5-118"><a id="Concepts"></a>Begrepp</span><span class="sxs-lookup"><span data-stu-id="20fa5-118"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="20fa5-119">Hej Twilio API är en RESTful-API som tillhandahåller röst- och SMS-funktioner för program.</span><span class="sxs-lookup"><span data-stu-id="20fa5-119">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="20fa5-120">Klientbibliotek är tillgängliga på flera språk. en lista, se [Twilio-API-bibliotek][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="20fa5-120">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

### <span data-ttu-id="20fa5-121"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="20fa5-121"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="20fa5-122">TwiML är en uppsättning XML-baserade instruktioner som talar om för Twilio på hur tooprocess ett samtal eller SMS.</span><span class="sxs-lookup"><span data-stu-id="20fa5-122">TwiML is a set of XML-based instructions that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="20fa5-123">Exempelvis skulle hello följande TwiML omvandla hello text **Hello World** toospeech.</span><span class="sxs-lookup"><span data-stu-id="20fa5-123">As an example, hello following TwiML would convert hello text **Hello World** toospeech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

<span data-ttu-id="20fa5-124">Alla TwiML dokument har `<Response>` som deras rotelementet.</span><span class="sxs-lookup"><span data-stu-id="20fa5-124">All TwiML documents have `<Response>` as their root element.</span></span> <span data-ttu-id="20fa5-125">Därifrån kan använda du Twilio verb toodefine hello beteendet för ditt program.</span><span class="sxs-lookup"><span data-stu-id="20fa5-125">From there, you use Twilio Verbs toodefine hello behavior of your application.</span></span>

### <span data-ttu-id="20fa5-126"><a id="Verbs"></a>TwiML verb</span><span class="sxs-lookup"><span data-stu-id="20fa5-126"><a id="Verbs"></a>TwiML Verbs</span></span>
<span data-ttu-id="20fa5-127">Twilio-verb är XML-taggar som talar om Twilio vad för**gör**.</span><span class="sxs-lookup"><span data-stu-id="20fa5-127">Twilio Verbs are XML tags that tell Twilio what too**do**.</span></span> <span data-ttu-id="20fa5-128">Till exempel hello  **&lt;säg&gt;**  verb instruerar Twilio tooaudibly leverera ett meddelande på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="20fa5-128">For example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span> 

<span data-ttu-id="20fa5-129">hello följer en lista över Twilio verb.</span><span class="sxs-lookup"><span data-stu-id="20fa5-129">hello following is a list of Twilio verbs.</span></span>

* <span data-ttu-id="20fa5-130">**&lt;Ring&gt;**: ansluter hello anroparen tooanother phone.</span><span class="sxs-lookup"><span data-stu-id="20fa5-130">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="20fa5-131">**&lt;Samla in&gt;**: samlar in siffror som anges på hello telefon tangentbordet.</span><span class="sxs-lookup"><span data-stu-id="20fa5-131">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="20fa5-132">**&lt;Koppla ned&gt;**: slutar ett anrop.</span><span class="sxs-lookup"><span data-stu-id="20fa5-132">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="20fa5-133">**&lt;Spela upp&gt;**: spelar en ljudfil.</span><span class="sxs-lookup"><span data-stu-id="20fa5-133">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="20fa5-134">**&lt;Pausa&gt;**: tyst väntar på ett angivet antal sekunder.</span><span class="sxs-lookup"><span data-stu-id="20fa5-134">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="20fa5-135">**&lt;Posten&gt;**: registrerar hello anroparen röst och returnerar en URL för en fil som innehåller hello registrering.</span><span class="sxs-lookup"><span data-stu-id="20fa5-135">**&lt;Record&gt;**: Records hello caller's voice and returns a URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="20fa5-136">**&lt;Omdirigera&gt;**: Överför kontroll över ett samtal eller SMS toohello TwiML på en annan URL.</span><span class="sxs-lookup"><span data-stu-id="20fa5-136">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="20fa5-137">**&lt;Avvisa&gt;**: avvisar en inkommande anropa tooyour Twilio tal utan fakturering du</span><span class="sxs-lookup"><span data-stu-id="20fa5-137">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you</span></span>
* <span data-ttu-id="20fa5-138">**&lt;Säg&gt;**: konverterar text toospeech som görs på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="20fa5-138">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="20fa5-139">**&lt;SMS&gt;**: skickar ett SMS-meddelande.</span><span class="sxs-lookup"><span data-stu-id="20fa5-139">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

<span data-ttu-id="20fa5-140">Läs mer om Twilio verb, deras attribut och TwiML [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="20fa5-140">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="20fa5-141">Mer information om hello Twilio-API finns [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="20fa5-141">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="20fa5-142"><a id="CreateAccount"></a>Skapa ett Twilio-konto</span><span class="sxs-lookup"><span data-stu-id="20fa5-142"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="20fa5-143">När du är klar tooget Twilio-konto kan logga på [försök Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="20fa5-143">When you're ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="20fa5-144">Du kan börja med ett kostnadsfritt konto och uppgradera ditt konto senare.</span><span class="sxs-lookup"><span data-stu-id="20fa5-144">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="20fa5-145">När du registrerar dig för ett Twilio-konto får du ett kostnadsfritt telefonnummer för ditt program.</span><span class="sxs-lookup"><span data-stu-id="20fa5-145">When you sign up for a Twilio account, you'll get a free phone number for your application.</span></span> <span data-ttu-id="20fa5-146">Du får även ett SID-konto och en token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="20fa5-146">You'll also receive an account SID and an auth token.</span></span> <span data-ttu-id="20fa5-147">Båda ska vara nödvändiga toomake Twilio API-anrop.</span><span class="sxs-lookup"><span data-stu-id="20fa5-147">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="20fa5-148">tooprevent obehörig åtkomst till tooyour konto, skydda din token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="20fa5-148">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="20fa5-149">Ditt konto SID och auth token kan visas på hello [Twilio kontosida][twilio_account]i hello fält med namnet **konto SID** och **AUTH TOKEN** , respektive.</span><span class="sxs-lookup"><span data-stu-id="20fa5-149">Your account SID and auth token are viewable at hello [Twilio account page][twilio_account], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

### <span data-ttu-id="20fa5-150"><a id="VerifyPhoneNumbers"></a>Kontrollera telefonnummer</span><span class="sxs-lookup"><span data-stu-id="20fa5-150"><a id="VerifyPhoneNumbers"></a>Verify Phone Numbers</span></span>
<span data-ttu-id="20fa5-151">I tillägg toohello tal som du får från Twilio, kan du också kontrollera siffror som du hanterar (d.v.s. din mobiltelefon eller home telefonnumret) för användning i dina program.</span><span class="sxs-lookup"><span data-stu-id="20fa5-151">In addition toohello number you are given by Twilio, you can also verify numbers that you control (i.e. your cell phone or home phone number) for use in your applications.</span></span> 

<span data-ttu-id="20fa5-152">Mer information om hur tooverify ett telefonnummer finns [hantera siffror][verify_phone].</span><span class="sxs-lookup"><span data-stu-id="20fa5-152">For information on how tooverify a phone number, see [Manage Numbers][verify_phone].</span></span>

## <span data-ttu-id="20fa5-153"><a id="create_app"></a>Skapa ett Ruby-program</span><span class="sxs-lookup"><span data-stu-id="20fa5-153"><a id="create_app"></a>Create a Ruby Application</span></span>
<span data-ttu-id="20fa5-154">Ett Ruby-program som använder hello Twilio-tjänst och körs i Azure är inte annorlunda än andra Ruby program som använder hello Twilio-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="20fa5-154">A Ruby application that uses hello Twilio service and is running in Azure is no different than any other Ruby application that uses hello Twilio service.</span></span> <span data-ttu-id="20fa5-155">Medan Twilio tjänsterna RESTful och kan anropas från Ruby på flera sätt, den här artikeln fokuserar på hur toouse Twilio tjänster med [Twilio hjälpbibliotek för Ruby][twilio_ruby].</span><span class="sxs-lookup"><span data-stu-id="20fa5-155">While Twilio services are RESTful and can be called from Ruby in several ways, this article will focus on how toouse Twilio services with [Twilio helper library for Ruby][twilio_ruby].</span></span>

<span data-ttu-id="20fa5-156">Första, [ställa in en ny Azure Linux VM] [ azure_vm_setup] tooact som värd för ditt nya Ruby webbprogram.</span><span class="sxs-lookup"><span data-stu-id="20fa5-156">First, [set-up a new Azure Linux VM][azure_vm_setup] tooact as a host for your new Ruby web application.</span></span> <span data-ttu-id="20fa5-157">Ignorera hello steg som involverar hello skapandet av en spår app bara ställa in hello VM.</span><span class="sxs-lookup"><span data-stu-id="20fa5-157">Ignore hello steps involving hello creation of a Rails app, just set-up hello VM.</span></span> <span data-ttu-id="20fa5-158">Kontrollera att du skapar en slutpunkt med en extern port 80 och en intern port 5000.</span><span class="sxs-lookup"><span data-stu-id="20fa5-158">Make sure you create an Endpoint with an external port of 80 and an internal port of 5000.</span></span>

<span data-ttu-id="20fa5-159">I hello exemplen nedan, kommer vi att använda [Sinatra][sinatra], ett väldigt enkelt webbramverk för Ruby.</span><span class="sxs-lookup"><span data-stu-id="20fa5-159">In hello examples below, we will be using [Sinatra][sinatra], a very simple web framework for Ruby.</span></span> <span data-ttu-id="20fa5-160">Men du kan visserligen använda hello Twilio hjälpbibliotek för Ruby med alla andra webbramverk, inklusive Ruby spår.</span><span class="sxs-lookup"><span data-stu-id="20fa5-160">But you can certainly use hello Twilio helper library for Ruby with any other web framework, including Ruby on Rails.</span></span>

<span data-ttu-id="20fa5-161">SSH till den nya virtuella datorn och skapar en katalog för din nya app.</span><span class="sxs-lookup"><span data-stu-id="20fa5-161">SSH into your new VM and create a directory for your new app.</span></span> <span data-ttu-id="20fa5-162">Skapa en fil med namnet Gemfile och kopiera hello följande kod till den i katalogen:</span><span class="sxs-lookup"><span data-stu-id="20fa5-162">Inside that directory, create a file called Gemfile and copy hello following code into it:</span></span>

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

<span data-ttu-id="20fa5-163">På kommandoraden för hello kör `bundle install`.</span><span class="sxs-lookup"><span data-stu-id="20fa5-163">On hello command line run `bundle install`.</span></span> <span data-ttu-id="20fa5-164">Detta installerar hello ovanstående beroenden.</span><span class="sxs-lookup"><span data-stu-id="20fa5-164">This will install hello dependencies above.</span></span> <span data-ttu-id="20fa5-165">Sedan skapar en fil med namnet `web.rb`.</span><span class="sxs-lookup"><span data-stu-id="20fa5-165">Next create a file called `web.rb`.</span></span> <span data-ttu-id="20fa5-166">Det här är där hello-koden för ditt webbprogram finns.</span><span class="sxs-lookup"><span data-stu-id="20fa5-166">This will be where hello code for your web app lives.</span></span> <span data-ttu-id="20fa5-167">Klistra in följande kod till den hello:</span><span class="sxs-lookup"><span data-stu-id="20fa5-167">Paste hello following code into it:</span></span>

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

<span data-ttu-id="20fa5-168">Nu bör du kunna hello kör hello kommandot `ruby web.rb -p 5000`.</span><span class="sxs-lookup"><span data-stu-id="20fa5-168">At this point you should be able hello run hello command `ruby web.rb -p 5000`.</span></span> <span data-ttu-id="20fa5-169">Detta kommer rotationsrutan upp en liten webbserver på port 5000.</span><span class="sxs-lookup"><span data-stu-id="20fa5-169">This will spin-up a small web server on port 5000.</span></span> <span data-ttu-id="20fa5-170">Du bör vara kan toobrowse toothis app i webbläsaren genom att besöka hello URL: en inställning för din virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="20fa5-170">You should be able toobrowse toothis app in your browser by visiting hello URL you set-up for your Azure VM.</span></span> <span data-ttu-id="20fa5-171">När du kan komma åt ditt webbprogram i hello webbläsare, är du redo toostart bygga en Twilio-app.</span><span class="sxs-lookup"><span data-stu-id="20fa5-171">Once you can reach your web app in hello browser, you're ready toostart building a Twilio app.</span></span>

## <span data-ttu-id="20fa5-172"><a id="configure_app"></a>Konfigurera ditt program tooUse Twilio</span><span class="sxs-lookup"><span data-stu-id="20fa5-172"><a id="configure_app"></a>Configure Your Application tooUse Twilio</span></span>
<span data-ttu-id="20fa5-173">Du kan konfigurera biblioteket web app toouse hello Twilio genom att uppdatera din `Gemfile` tooinclude raden:</span><span class="sxs-lookup"><span data-stu-id="20fa5-173">You can configure your web app toouse hello Twilio library by updating your `Gemfile` tooinclude this line:</span></span>

    gem 'twilio-ruby'

<span data-ttu-id="20fa5-174">Kör på kommandoraden hello `bundle install`.</span><span class="sxs-lookup"><span data-stu-id="20fa5-174">On hello command line, run `bundle install`.</span></span> <span data-ttu-id="20fa5-175">Öppna `web.rb` och den här raden hello överst, inklusive:</span><span class="sxs-lookup"><span data-stu-id="20fa5-175">Now open `web.rb` and including this line at hello top:</span></span>

    require 'twilio-ruby'

<span data-ttu-id="20fa5-176">Du är nu allt klart toouse hello Twilio hjälpbibliotek för Ruby i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="20fa5-176">You're now all set toouse hello Twilio helper library for Ruby in your web app.</span></span>

## <span data-ttu-id="20fa5-177"><a id="howto_make_call"></a>Så här: göra en utgående anrop</span><span class="sxs-lookup"><span data-stu-id="20fa5-177"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="20fa5-178">hello följande visar hur en utgående toomake anropa.</span><span class="sxs-lookup"><span data-stu-id="20fa5-178">hello following shows how toomake an outgoing call.</span></span> <span data-ttu-id="20fa5-179">Viktiga begrepp omfattar med hello Twilio hjälpbibliotek för Ruby toomake REST API-anrop och återges TwiML.</span><span class="sxs-lookup"><span data-stu-id="20fa5-179">Key concepts include using hello Twilio helper library for Ruby toomake REST API calls and rendering TwiML.</span></span> <span data-ttu-id="20fa5-180">Ersätt värdena för hello **från** och **till** telefonnummer och se till att du kontrollerar hello **från** telefonnummer för Twilio-konto tidigare toorunning hello koden.</span><span class="sxs-lookup"><span data-stu-id="20fa5-180">Substitute your values for hello **From** and **To** phone numbers, and ensure that you verify hello **From** phone number for your Twilio account prior toorunning hello code.</span></span>

<span data-ttu-id="20fa5-181">Lägg till den här funktionen för`web.md`:</span><span class="sxs-lookup"><span data-stu-id="20fa5-181">Add this function too`web.md`:</span></span>

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # hello number of hello phone initiating hello hello call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # hello number of hello phone receiving call.
    too= "NNNNNNNNNNN";

    # Use hello Twilio-provided site for hello TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";

    get '/make_call' do
      # Create hello call client.
      client = Twilio::REST::Client.new(sid, token);

      # Make hello call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end

<span data-ttu-id="20fa5-182">Om du öppna upp `http://yourdomain.cloudapp.net/make_call` i en webbläsare som utlöser hello anropet toohello Twilio API toomake hello telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="20fa5-182">If you open-up `http://yourdomain.cloudapp.net/make_call` in a browser, that will trigger hello call toohello Twilio API toomake hello phone call.</span></span> <span data-ttu-id="20fa5-183">Hej först två parametrar i `client.account.calls.create` är ganska självförklarande: hello nummer hello anropet är `from` och hello nummer hello anropet är `to`.</span><span class="sxs-lookup"><span data-stu-id="20fa5-183">hello first two parameters in `client.account.calls.create` are fairly self-explanatory: hello number hello call is `from` and hello number hello call is `to`.</span></span> 

<span data-ttu-id="20fa5-184">Hej tredje parametern (`url`) är hello-URL att Twilio begär tooget instruktioner om vilken toodo när hello anrop är ansluten.</span><span class="sxs-lookup"><span data-stu-id="20fa5-184">hello third parameter (`url`) is hello URL that Twilio requests tooget instructions on what toodo once hello call is connected.</span></span> <span data-ttu-id="20fa5-185">I det här fallet vi ställa in en URL (`http://yourdomain.cloudapp.net`) som returnerar ett enkelt TwiML dokument och använder hello `<Say>` verb toodo vissa text till tal- och säg ”Hello apa” toohello personen som tar emot hello anropa.</span><span class="sxs-lookup"><span data-stu-id="20fa5-185">In this case we set-up a URL (`http://yourdomain.cloudapp.net`) that returns a simple TwiML document and uses hello `<Say>` verb toodo some text-to-speech and say "Hello Monkey" toohello person recieving hello call.</span></span>

## <span data-ttu-id="20fa5-186"><a id="howto_recieve_sms"></a>Så här: ta emot ett SMS-meddelande</span><span class="sxs-lookup"><span data-stu-id="20fa5-186"><a id="howto_recieve_sms"></a>How to: Recieve an SMS message</span></span>
<span data-ttu-id="20fa5-187">I föregående exempel för hello vi initierade en **utgående** telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="20fa5-187">In hello previous example we initiated an **outgoing** phone call.</span></span> <span data-ttu-id="20fa5-188">Den här gången, ska vi använda hello telefonnumret som Twilio angav under registreringen tooprocess en **inkommande** SMS-meddelande.</span><span class="sxs-lookup"><span data-stu-id="20fa5-188">This time, let's use hello phone number that Twilio gave us during sign-up tooprocess an **incoming** SMS message.</span></span>

<span data-ttu-id="20fa5-189">Första, logga in på tooyour [Twilio instrumentpanelen][twilio_account].</span><span class="sxs-lookup"><span data-stu-id="20fa5-189">First, log-in tooyour [Twilio dashboard][twilio_account].</span></span> <span data-ttu-id="20fa5-190">Klicka på ”siffror” i hello översta nav och klicka sedan på hello Twilio-nummer som du har angett.</span><span class="sxs-lookup"><span data-stu-id="20fa5-190">Click on "Numbers" in hello top nav and then click on hello Twilio number you were provided.</span></span> <span data-ttu-id="20fa5-191">Ser du två URL: er som du kan konfigurera.</span><span class="sxs-lookup"><span data-stu-id="20fa5-191">You'll see two URLs that you can configure.</span></span> <span data-ttu-id="20fa5-192">En röst begärd URL och ett SMS URL-begäran.</span><span class="sxs-lookup"><span data-stu-id="20fa5-192">A Voice Request URL and an SMS Request URL.</span></span> <span data-ttu-id="20fa5-193">Dessa är hello URL: er som görs Twilio anrop när ett telefonsamtal eller ett SMS skickas tooyour nummer.</span><span class="sxs-lookup"><span data-stu-id="20fa5-193">These are hello URLs that Twilio calls whenever a phone call is made or an SMS is sent tooyour number.</span></span> <span data-ttu-id="20fa5-194">hello URL: er kallas även ”web hook”.</span><span class="sxs-lookup"><span data-stu-id="20fa5-194">hello URLs are also known as "web hooks".</span></span>

<span data-ttu-id="20fa5-195">Vi vill gärna tooprocess inkommande SMS-meddelanden, så vi uppdatera hello URL för`http://yourdomain.cloudapp.net/sms_url`.</span><span class="sxs-lookup"><span data-stu-id="20fa5-195">We would like tooprocess incoming SMS messages, so let's update hello URL too`http://yourdomain.cloudapp.net/sms_url`.</span></span> <span data-ttu-id="20fa5-196">Gå vidare och klicka på Spara ändringar på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="20fa5-196">Go ahead and click Save Changes at hello bottom of hello page.</span></span> <span data-ttu-id="20fa5-197">Nu tillbaka `web.rb` vi programmet våra program toohandle detta:</span><span class="sxs-lookup"><span data-stu-id="20fa5-197">Now, back in `web.rb` let's program our application toohandle this:</span></span>

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for hello ping! Twilio and Azure rock!</Message>
       </Response>"
    end

<span data-ttu-id="20fa5-198">Efter att hello ändringen, se till att toore-start ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="20fa5-198">After making hello change, make sure toore-start your web app.</span></span> <span data-ttu-id="20fa5-199">Nu ta ut din telefon och skicka ett SMS tooyour Twilio nummer.</span><span class="sxs-lookup"><span data-stu-id="20fa5-199">Now, take out your phone and send an SMS tooyour Twilio number.</span></span> <span data-ttu-id="20fa5-200">Du får omedelbart ett SMS-svar som säger ”Hey, Tack för hello ping!</span><span class="sxs-lookup"><span data-stu-id="20fa5-200">You should promptly get an SMS response that says "Hey, thanks for hello ping!</span></span> <span data-ttu-id="20fa5-201">Twilio och Azure Berg ”!.</span><span class="sxs-lookup"><span data-stu-id="20fa5-201">Twilio and Azure rock!".</span></span>

## <span data-ttu-id="20fa5-202"><a id="additional_services"></a>Så här: använda ytterligare Twilio-tjänster</span><span class="sxs-lookup"><span data-stu-id="20fa5-202"><a id="additional_services"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="20fa5-203">Dessutom kan toohello exemplen nedan, Twilio erbjuder webbaserade API: er som du använda tooleverage ytterligare Twilio-funktioner från din Azure-program.</span><span class="sxs-lookup"><span data-stu-id="20fa5-203">In addition toohello examples shown here, Twilio offers web-based APIs that you can use tooleverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="20fa5-204">Fullständig information finns i hello [Twilio-API-dokumentationen][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="20fa5-204">For full details, see hello [Twilio API documentation][twilio_api_documentation].</span></span>

### <span data-ttu-id="20fa5-205"><a id="NextSteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="20fa5-205"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="20fa5-206">Nu när du har lärt dig hello grunderna i hello Twilio-tjänsten ska du följa dessa länkar toolearn mer:</span><span class="sxs-lookup"><span data-stu-id="20fa5-206">Now that you've learned hello basics of hello Twilio service, follow these links toolearn more:</span></span>

* <span data-ttu-id="20fa5-207">[Riktlinjer för Twilio-säkerhet][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="20fa5-207">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="20fa5-208">[Twilio HowTos och exempelkod][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="20fa5-208">[Twilio HowTos and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="20fa5-209">[Twilio snabbstarten självstudier][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="20fa5-209">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span> 
* <span data-ttu-id="20fa5-210">[Twilio på GitHub][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="20fa5-210">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="20fa5-211">[Kontakta tooTwilio Support][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="20fa5-211">[Talk tooTwilio Support][twilio_support]</span></span>

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
