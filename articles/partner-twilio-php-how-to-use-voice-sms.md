---
title: "aaaHow tooUse Twilio för röst- och SMS (PHP) | Microsoft Docs"
description: "Lär dig hur toomake ett telefonsamtal och skicka ett SMS-meddelandet med hello Twilio API-tjänsten på Azure. Kodexempel som skrivits i PHP."
documentationcenter: php
services: 
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 007f22e3-ac75-4868-8315-da000c2e0dd0
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 9354df8de694826a0ff7ea92620ec4d7e5c2fd70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-php"></a><span data-ttu-id="8c0ae-104">Hur tooUse Twilio för röst- och SMS-funktionerna i PHP</span><span class="sxs-lookup"><span data-stu-id="8c0ae-104">How tooUse Twilio for Voice and SMS Capabilities in PHP</span></span>
<span data-ttu-id="8c0ae-105">Den här guiden visar hur tooperform vanliga programmeringsuppgifter med hello Twilio API-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="8c0ae-106">hello-scenarier som tas upp inkluderar att ringa och ett kort meddelande (SMS Service)-meddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="8c0ae-107">Mer information om Twilio och använder röst- och SMS i dina program finns hello [nästa steg](#NextSteps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="8c0ae-108"><a id="WhatIs"></a>Vad är Twilio?</span><span class="sxs-lookup"><span data-stu-id="8c0ae-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="8c0ae-109">Twilio startar hello framtiden för kommunikation, aktivera utvecklare tooembed röst, VoIP, och meddelanden i program.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-109">Twilio is powering hello future of business communications, enabling developers tooembed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="8c0ae-110">De virtualisera alla infrastruktur som behövs i en molnbaserad, globala exponera den via hello Twilio kommunikation API-plattformen.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through hello Twilio communications API platform.</span></span> <span data-ttu-id="8c0ae-111">Programmen är enkla toobuild och skalbara.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-111">Applications are simple toobuild and scalable.</span></span> <span data-ttu-id="8c0ae-112">Få flexibilitet med lön-som-dig gå priser och dra nytta av molnet tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-112">Enjoy flexibility with pay-as-you go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="8c0ae-113">**Twilio röst** kan ditt program toomake och ta emot telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-113">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span> <span data-ttu-id="8c0ae-114">**Twilio SMS** gör att din ansökan toosend och ta emot textmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-114">**Twilio SMS** enables your application toosend and receive text messages.</span></span> <span data-ttu-id="8c0ae-115">**Twilio klienten** tillåter du toomake VoIP-anrop från telefon, surfplatta eller webbläsare och stöder WebRTC.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-115">**Twilio Client** allows you toomake VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="8c0ae-116"><a id="Pricing"></a>Priser för Twilio och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="8c0ae-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="8c0ae-117">Azure-kunder får en [specialerbjudande](http://www.twilio.com/azure): kostnadsfri $10 Twilio kredit när du uppgraderar ditt Twilio-konto.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-117">Azure customers receive a [special offer](http://www.twilio.com/azure): complimentary $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="8c0ae-118">Den här Twilio-kredit kan vara tillämpade tooany Twilio användning ($10 kredit motsvarande toosending upp till 1 000 SMS-meddelanden eller ta emot in too1000 inkommande röst minuter beroende på hello platsen för ditt mål för telefon och meddelandet eller samtal).</span><span class="sxs-lookup"><span data-stu-id="8c0ae-118">This Twilio Credit can be applied tooany Twilio usage ($10 credit equivalent toosending as many as 1,000 SMS messages or receiving up too1000 inbound Voice minutes, depending on hello location of your phone number and message or call destination).</span></span> <span data-ttu-id="8c0ae-119">Lösa in den här Twilio-kredit och kom igång på: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span><span class="sxs-lookup"><span data-stu-id="8c0ae-119">Redeem this Twilio credit and get started at: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span></span>

<span data-ttu-id="8c0ae-120">Twilio är en betalning per användning.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="8c0ae-121">Det finns inga avgifter för installation och du kan stänga ditt konto när som helst.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-121">There are no set-up fees and you can close your account at any time.</span></span> <span data-ttu-id="8c0ae-122">Du hittar mer information i [Twilio priser][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="8c0ae-122">You can find more details at [Twilio Pricing][twilio_pricing].</span></span>

## <span data-ttu-id="8c0ae-123"><a id="Concepts"></a>Begrepp</span><span class="sxs-lookup"><span data-stu-id="8c0ae-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="8c0ae-124">Hej Twilio API är en RESTful-API som tillhandahåller röst- och SMS-funktioner för program.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-124">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="8c0ae-125">Klientbibliotek är tillgängliga på flera språk. en lista, se [Twilio-API-bibliotek][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="8c0ae-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="8c0ae-126">Viktiga aspekter av hello Twilio API är Twilio verb och Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="8c0ae-126">Key aspects of hello Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="8c0ae-127"><a id="Verbs"></a>Twilio-verb</span><span class="sxs-lookup"><span data-stu-id="8c0ae-127"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="8c0ae-128">hello API väljer Twilio verb; till exempel hello  **&lt;säg&gt;**  verb instruerar Twilio tooaudibly leverera ett meddelande på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-128">hello API makes use of Twilio verbs; for example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span>

<span data-ttu-id="8c0ae-129">hello följer en lista över Twilio verb.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-129">hello following is a list of Twilio verbs.</span></span> <span data-ttu-id="8c0ae-130">Lär dig mer om hello andra verb och funktioner via [Markup Language Twilio dokumentationen](http://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="8c0ae-130">Learn about hello other verbs and capabilities via [Twilio Markup Language documentation](http://www.twilio.com/docs/api/twiml).</span></span>

* <span data-ttu-id="8c0ae-131">**&lt;Ring&gt;**: ansluter hello anroparen tooanother phone.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-131">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="8c0ae-132">**&lt;Samla in&gt;**: samlar in siffror som anges på hello telefon tangentbordet.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-132">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="8c0ae-133">**&lt;Koppla ned&gt;**: slutar ett anrop.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="8c0ae-134">**&lt;Spela upp&gt;**: spelar en ljudfil.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-134">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="8c0ae-135">**&lt;Pausa&gt;**: tyst väntar på ett angivet antal sekunder.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-135">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="8c0ae-136">**&lt;Posten&gt;**: registrerar hello anroparen röst och returnerar en URL för en fil som innehåller hello registrering.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-136">**&lt;Record&gt;**: Records hello caller's voice and returns a URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="8c0ae-137">**&lt;Omdirigera&gt;**: Överför kontroll över ett samtal eller SMS toohello TwiML på en annan URL.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-137">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="8c0ae-138">**&lt;Avvisa&gt;**: avvisar en inkommande anropa tooyour Twilio tal utan fakturering du</span><span class="sxs-lookup"><span data-stu-id="8c0ae-138">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you</span></span>
* <span data-ttu-id="8c0ae-139">**&lt;Säg&gt;**: konverterar text toospeech som görs på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-139">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="8c0ae-140">**&lt;SMS&gt;**: skickar ett SMS-meddelande.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-140">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="8c0ae-141"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="8c0ae-141"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="8c0ae-142">TwiML är en uppsättning XML-baserade instruktioner baserat på hello Twilio verb som informerar Twilio på hur tooprocess ett samtal eller SMS.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-142">TwiML is a set of XML-based instructions based on hello Twilio verbs that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="8c0ae-143">Exempelvis skulle hello följande TwiML omvandla hello text **Hello World** toospeech.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-143">As an example, hello following TwiML would convert hello text **Hello World** toospeech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

<span data-ttu-id="8c0ae-144">När programmet-anrop hello Twilio-API, är en av parametrarna för hello API hello URL som returnerar hello TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-144">When your application calls hello Twilio API, one of hello API parameters is hello URL that returns hello TwiML response.</span></span> <span data-ttu-id="8c0ae-145">För utveckling, kan du använda Twilio-tillhandahållna URL: er tooprovide hello TwiML svar som används av dina program.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-145">For development purposes, you can use Twilio-provided URLs tooprovide hello TwiML responses used by your applications.</span></span> <span data-ttu-id="8c0ae-146">Du kan också vara värd för dina egna URL: er tooproduce hello TwiML svar och ett annat alternativ är toouse hello **TwiMLResponse** objekt.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-146">You could also host your own URLs tooproduce hello TwiML responses, and another option is toouse hello **TwiMLResponse** object.</span></span>

<span data-ttu-id="8c0ae-147">Läs mer om Twilio verb, deras attribut och TwiML [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="8c0ae-147">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="8c0ae-148">Mer information om hello Twilio-API finns [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="8c0ae-148">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="8c0ae-149"><a id="CreateAccount"></a>Skapa ett Twilio-konto</span><span class="sxs-lookup"><span data-stu-id="8c0ae-149"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="8c0ae-150">När du är klar tooget Twilio-konto kan logga på [försök Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="8c0ae-150">When you're ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="8c0ae-151">Du kan börja med ett kostnadsfritt konto och uppgradera ditt konto senare.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-151">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="8c0ae-152">När du registrerar dig för ett Twilio-konto får du ett konto-ID och ett token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-152">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="8c0ae-153">Båda ska vara nödvändiga toomake Twilio API-anrop.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-153">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="8c0ae-154">tooprevent obehörig åtkomst till tooyour konto, skydda din token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-154">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="8c0ae-155">Ditt konto-ID och autentisering token kan visas på hello [Twilio kontosida][twilio_account]i hello fält med namnet **konto SID** och **AUTH TOKEN**respektive.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-155">Your account ID and authentication token are viewable at hello [Twilio account page][twilio_account], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="8c0ae-156"><a id="create_app"></a>Skapa en PHP-program</span><span class="sxs-lookup"><span data-stu-id="8c0ae-156"><a id="create_app"></a>Create a PHP Application</span></span>
<span data-ttu-id="8c0ae-157">Ett PHP-program som använder hello Twilio-tjänsten och körs i Azure är inte annorlunda än andra PHP-program som använder hello Twilio tjänst.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-157">A PHP application that uses hello Twilio service and is running in Azure is no different than any other PHP application that uses hello Twilio service.</span></span> <span data-ttu-id="8c0ae-158">När Twilio är REST-baserad och kan anropas från PHP på flera sätt, den här artikeln fokuserar på hur toouse Twilio tjänster med [Twilio-biblioteket för PHP från GitHub][twilio_php].</span><span class="sxs-lookup"><span data-stu-id="8c0ae-158">While Twilio services are REST-based and can be called from PHP in several ways, this article will focus on how toouse Twilio services with [Twilio library for PHP from GitHub][twilio_php].</span></span> <span data-ttu-id="8c0ae-159">Mer information om hur du använder hello Twilio-biblioteket för PHP finns [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].</span><span class="sxs-lookup"><span data-stu-id="8c0ae-159">For more information about using hello Twilio library for PHP, see [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].</span></span>

<span data-ttu-id="8c0ae-160">Detaljerade anvisningar för att skapa och distribuera en Twilio/PHP programmet tooAzure finns på [hur tooMake ett telefonsamtal med Twilio i ett PHP-program på Azure][howto_phonecall_php].</span><span class="sxs-lookup"><span data-stu-id="8c0ae-160">Detailed instructions for building and deploying a Twilio/PHP application tooAzure are available at [How tooMake a Phone Call Using Twilio in a PHP Application on Azure][howto_phonecall_php].</span></span>

## <span data-ttu-id="8c0ae-161"><a id="configure_app"></a>Konfigurera ditt program tooUse Twilio-bibliotek</span><span class="sxs-lookup"><span data-stu-id="8c0ae-161"><a id="configure_app"></a>Configure Your Application tooUse Twilio Libraries</span></span>
<span data-ttu-id="8c0ae-162">Du kan konfigurera biblioteket programmet toouse hello Twilio för PHP på två sätt:</span><span class="sxs-lookup"><span data-stu-id="8c0ae-162">You can configure your application toouse hello Twilio library for PHP in two ways:</span></span>

1. <span data-ttu-id="8c0ae-163">Hämta hello Twilio-biblioteket för PHP från GitHub ([https://github.com/twilio/twilio-php][twilio_php]) och Lägg till hello **Services** directory tooyour-program.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-163">Download hello Twilio library for PHP from GitHub ([https://github.com/twilio/twilio-php][twilio_php]) and add hello **Services** directory tooyour application.</span></span>
   
    <span data-ttu-id="8c0ae-164">ELLER</span><span class="sxs-lookup"><span data-stu-id="8c0ae-164">-OR-</span></span>
2. <span data-ttu-id="8c0ae-165">Installera hello Twilio-biblioteket för PHP som PÄRONTRÄD paket.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-165">Install hello Twilio library for PHP as a PEAR package.</span></span> <span data-ttu-id="8c0ae-166">Den kan installeras med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="8c0ae-166">It can be installed with hello following commands:</span></span>
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

<span data-ttu-id="8c0ae-167">När du har installerat hello Twilio-biblioteket för PHP, du kan sedan lägga till en **require_once** instruktionen hello överst i din PHP filer tooreference hello bibliotek:</span><span class="sxs-lookup"><span data-stu-id="8c0ae-167">Once you have installed hello Twilio library for PHP, you can then add a **require_once** statement at hello top of your PHP files tooreference hello library:</span></span>

        require_once 'Services/Twilio.php';

<span data-ttu-id="8c0ae-168">Mer information finns i [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].</span><span class="sxs-lookup"><span data-stu-id="8c0ae-168">For more information, see [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].</span></span>

## <span data-ttu-id="8c0ae-169"><a id="howto_make_call"></a>Så här: göra en utgående anrop</span><span class="sxs-lookup"><span data-stu-id="8c0ae-169"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="8c0ae-170">hello nedan visar hur en utgående toomake Ring upp med hello **Services_Twilio** klass.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-170">hello following shows how toomake an outgoing call using hello **Services_Twilio** class.</span></span> <span data-ttu-id="8c0ae-171">Den här koden används även en Twilio-tillhandahållna platsen tooreturn hello Twilio Markup Language (TwiML) svar.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-171">This code also uses a Twilio-provided site tooreturn hello Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="8c0ae-172">Ersätt värdena för hello **från** och **till** telefonnummer och se till att du kontrollerar hello **från** telefonnummer för Twilio-konto tidigare toorunning hello koden.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-172">Substitute your values for hello **From** and **To** phone numbers, and ensure that you verify hello **From** phone number for your Twilio account prior toorunning hello code.</span></span>

    // Include hello Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // hello number of hello phone initiating hello hello call.
    $from_number = "NNNNNNNNNNN";

    // hello number of hello phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use hello Twilio-provided site for hello TwiML response.
    $url = "http://twimlets.com/message";

    // hello phone message text.
    $message = "Hello world.";

    // Create hello call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make hello call.
    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

<span data-ttu-id="8c0ae-173">Som tidigare nämnts kan använder den här koden en Twilio-tillhandahållna platsen tooreturn hello TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-173">As mentioned, this code uses a Twilio-provided site tooreturn hello TwiML response.</span></span> <span data-ttu-id="8c0ae-174">Du kan i stället använda din egen webbplats tooprovide hello TwiML svar; Mer information finns i [hur tooProvide TwiML svar från din egen webbplats](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="8c0ae-174">You could instead use your own site tooprovide hello TwiML response; for more information, see [How tooProvide TwiML Responses from Your Own Web Site](#howto_provide_twiml_responses).</span></span>

* <span data-ttu-id="8c0ae-175">**Obs**: tootroubleshoot verifieringsfel för SSL-certifikat, se [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]</span><span class="sxs-lookup"><span data-stu-id="8c0ae-175">**Note**: tootroubleshoot SSL certificate validation errors, see [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]</span></span> 

## <span data-ttu-id="8c0ae-176"><a id="howto_send_sms"></a>Så här: skicka ett SMS-meddelande</span><span class="sxs-lookup"><span data-stu-id="8c0ae-176"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="8c0ae-177">hello nedan visar hur en SMS-meddelande med toosend hello **Services_Twilio** klass.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-177">hello following shows how toosend an SMS message using hello **Services_Twilio** class.</span></span> <span data-ttu-id="8c0ae-178">Hej **från** nummer tillhandahålls av Twilio för utvärderingsversionen konton toosend SMS-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-178">hello **From** number is provided by Twilio for trial accounts toosend SMS messages.</span></span> <span data-ttu-id="8c0ae-179">Hej **till** numret måste verifieras för Twilio-konto tidigare toorunning hello koden.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-179">hello **To** number must be verified for your Twilio account prior toorunning hello code.</span></span>

    // Include hello Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create hello call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send hello SMS message.
    try
    {
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <span data-ttu-id="8c0ae-180"><a id="howto_provide_twiml_responses"></a>Så här: Ange TwiML svar från din egen webbplats</span><span class="sxs-lookup"><span data-stu-id="8c0ae-180"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="8c0ae-181">När ditt program initierar en anropet toohello Twilio-API, skickar Twilio din begäran tooa URL-adress förväntades tooreturn TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-181">When your application initiates a call toohello Twilio API, Twilio will send your request tooa URL that is expected tooreturn a TwiML response.</span></span> <span data-ttu-id="8c0ae-182">hello-exemplet ovan använder hello Twilio-tillhandahållna URL [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="8c0ae-182">hello example above uses hello Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="8c0ae-183">(När TwiML är avsedd för användning av Twilio, kan du visa hello i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-183">(While TwiML is designed for use by Twilio, you can view hello it in your browser.</span></span> <span data-ttu-id="8c0ae-184">Klicka till exempel [http://twimlets.com/message] [ twimlet_message_url] toosee en tom `<Response>` element, klicka på ett annat exempel är [http://twimlets.com/message? Meddelande % 5B0 %5, D = Hello % 20World] [ twimlet_message_url_hello_world] toosee en `<Response>` element som innehåller en `<Say>` element.)</span><span class="sxs-lookup"><span data-stu-id="8c0ae-184">For example, click [http://twimlets.com/message][twimlet_message_url] toosee an empty `<Response>` element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] toosee a `<Response>` element that contains a `<Say>` element.)</span></span>

<span data-ttu-id="8c0ae-185">I stället för på hello angivna Twilio-URL, kan du skapa din egen webbplats som returnerar HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-185">Instead of relying on hello Twilio-provided URL, you can create your own site that returns HTTP responses.</span></span> <span data-ttu-id="8c0ae-186">Du kan skapa hello webbplats på alla språk som returnerar XML-svar; Det här avsnittet förutsätter att du använder PHP toocreate hello TwiML.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-186">You can create hello site in any language that returns XML responses; this topic assumes you'll be using PHP toocreate hello TwiML.</span></span>

<span data-ttu-id="8c0ae-187">Hej följande PHP sidan resulterar i ett TwiML-svar som säger **Hello World** i hello-anropet.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-187">hello following PHP page results in a TwiML response that says **Hello World** on hello call.</span></span>

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

<span data-ttu-id="8c0ae-188">Du kan se hello-exemplet ovan kan är hello TwiML svar bara ett XML-dokument.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-188">As you can see from hello example above, hello TwiML response is simply an XML document.</span></span> <span data-ttu-id="8c0ae-189">Hej Twilio-biblioteket för PHP innehåller klasser som ska generera TwiML för dig.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-189">hello Twilio library for PHP contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="8c0ae-190">hello exemplet nedan ger hello motsvarande svar som ovan, men använder hello **Services\_Twilio\_Twiml** klass i hello Twilio-biblioteket för PHP:</span><span class="sxs-lookup"><span data-stu-id="8c0ae-190">hello example below produces hello equivalent response as shown above, but uses hello **Services\_Twilio\_Twiml** class in hello Twilio library for PHP:</span></span>

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

<span data-ttu-id="8c0ae-191">Läs mer om TwiML [https://www.twilio.com/docs/api/twiml][twiml_reference].</span><span class="sxs-lookup"><span data-stu-id="8c0ae-191">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml][twiml_reference].</span></span> 

<span data-ttu-id="8c0ae-192">När du har PHP sidan Konfigurera tooprovide TwiML svar använder hello URL för hello PHP-sida som hello URL som skickades till hello `Services_Twilio->account->calls->create` metod.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-192">Once you have your PHP page set up tooprovide TwiML responses, use hello URL of hello PHP page as hello URL passed into hello  `Services_Twilio->account->calls->create`  method.</span></span> <span data-ttu-id="8c0ae-193">Till exempel om du har ett webbprogram med namnet **MyTwiML** distribuerade tooan Azure-värdtjänsten och hello hello PHP-sida heter **mytwiml.php**, hello URL kan skickas för **Services_ Twilio -> konto -> anrop -> Skapa** som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="8c0ae-193">For example, if you have a Web application named **MyTwiML** deployed tooan Azure hosted service, and hello name of hello PHP page is **mytwiml.php**, hello URL can be passed too **Services_Twilio->account->calls->create**  as shown in hello following example:</span></span>

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // hello phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

<span data-ttu-id="8c0ae-194">Mer information om hur du använder Twilio i Azure med PHP finns [hur tooMake ett telefonsamtal med Twilio i ett PHP-program på Azure][howto_phonecall_php].</span><span class="sxs-lookup"><span data-stu-id="8c0ae-194">For additional information about using Twilio in Azure with PHP, see [How tooMake a Phone Call Using Twilio in a PHP Application on Azure][howto_phonecall_php].</span></span>

## <span data-ttu-id="8c0ae-195"><a id="AdditionalServices"></a>Så här: använda ytterligare Twilio-tjänster</span><span class="sxs-lookup"><span data-stu-id="8c0ae-195"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="8c0ae-196">Dessutom kan toohello exemplen nedan, Twilio erbjuder webbaserade API: er som du använda tooleverage ytterligare Twilio-funktioner från din Azure-program.</span><span class="sxs-lookup"><span data-stu-id="8c0ae-196">In addition toohello examples shown here, Twilio offers web-based APIs that you can use tooleverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="8c0ae-197">Fullständig information finns i hello [Twilio-API-dokumentationen][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="8c0ae-197">For full details, see hello [Twilio API documentation][twilio_api_documentation].</span></span>

## <span data-ttu-id="8c0ae-198"><a id="NextSteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8c0ae-198"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="8c0ae-199">Nu när du har lärt dig hello grunderna i hello Twilio-tjänsten ska du följa dessa länkar toolearn mer:</span><span class="sxs-lookup"><span data-stu-id="8c0ae-199">Now that you've learned hello basics of hello Twilio service, follow these links toolearn more:</span></span>

* <span data-ttu-id="8c0ae-200">[Riktlinjer för Twilio-säkerhet][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="8c0ae-200">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="8c0ae-201">[Twilio ta och exempelkod][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="8c0ae-201">[Twilio HowTo's and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="8c0ae-202">[Twilio snabbstarten självstudier][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="8c0ae-202">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span> 
* <span data-ttu-id="8c0ae-203">[Twilio på GitHub][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="8c0ae-203">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="8c0ae-204">[Kontakta tooTwilio Support][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="8c0ae-204">[Talk tooTwilio Support][twilio_support]</span></span>

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_api_service]: https://api.twilio.com
[howto_phonecall_php]: partner-twilio-php-make-phone-call.md
[twilio_voice_request]: https://www.twilio.com/docs/api/twiml/twilio_request
[twilio_sms_request]: https://www.twilio.com/docs/api/twiml/sms/twilio_request
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
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
