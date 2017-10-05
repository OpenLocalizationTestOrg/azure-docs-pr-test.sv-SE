---
title: "Hur du använder Twilio för röst- och SMS (PHP) | Microsoft Docs"
description: "Lär dig att ringa ett telefonsamtal och skicka ett SMS-meddelande med Twilio-API-tjänsten på Azure. Kodexempel som skrivits i PHP."
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
ms.openlocfilehash: bd50eac7390e8639f77894689388e6926cdb619c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a><span data-ttu-id="9a6cd-104">Hur du använder Twilio för röst- och SMS-funktionerna i PHP</span><span class="sxs-lookup"><span data-stu-id="9a6cd-104">How to Use Twilio for Voice and SMS Capabilities in PHP</span></span>
<span data-ttu-id="9a6cd-105">Den här guiden visar hur du utför vanliga programmeringsuppgifter med Twilio-API-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-105">This guide demonstrates how to perform common programming tasks with the Twilio API service on Azure.</span></span> <span data-ttu-id="9a6cd-106">Scenarier som tas upp inkluderar att ringa och ett kort meddelande (SMS Service)-meddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-106">The scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="9a6cd-107">Mer information om Twilio och använder röst- och SMS i dina program finns i [nästa steg](#NextSteps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-107">For more information on Twilio and using voice and SMS in your applications, see the [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="9a6cd-108"><a id="WhatIs"></a>Vad är Twilio?</span><span class="sxs-lookup"><span data-stu-id="9a6cd-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="9a6cd-109">Twilio startar framtiden för kommunikation med, så att utvecklare kan bädda in röst, VoIP och meddelanden i program.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-109">Twilio is powering the future of business communications, enabling developers to embed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="9a6cd-110">De virtualisera alla infrastruktur som behövs i en molnbaserad, globala miljö, exponera den via Twilio kommunikation API-plattformen.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through the Twilio communications API platform.</span></span> <span data-ttu-id="9a6cd-111">Programmen är enkla att skapa och skalbara.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-111">Applications are simple to build and scalable.</span></span> <span data-ttu-id="9a6cd-112">Få flexibilitet med lön-som-dig gå priser och dra nytta av molnet tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-112">Enjoy flexibility with pay-as-you go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="9a6cd-113">**Twilio röst** gör att dina program att göra och ta emot telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-113">**Twilio Voice** allows your applications to make and receive phone calls.</span></span> <span data-ttu-id="9a6cd-114">**Twilio SMS** gör att programmet kan skicka och ta emot textmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-114">**Twilio SMS** enables your application to send and receive text messages.</span></span> <span data-ttu-id="9a6cd-115">**Twilio klienten** kan du se VoIP-anrop från telefon, surfplatta eller webbläsare och stöder WebRTC.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-115">**Twilio Client** allows you to make VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="9a6cd-116"><a id="Pricing"></a>Priser för Twilio och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="9a6cd-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="9a6cd-117">Azure-kunder får en [specialerbjudande](http://www.twilio.com/azure): kostnadsfri $10 Twilio kredit när du uppgraderar ditt Twilio-konto.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-117">Azure customers receive a [special offer](http://www.twilio.com/azure): complimentary $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="9a6cd-118">Den här Twilio-kredit kan tillämpas på alla Twilio-användning ($10 kredit motsvarar upp till 1 000 SMS-meddelanden skickas eller tas emot upp till 1 000 inkommande röst minuter beroende på platsen för ditt mål för telefon och meddelandet eller samtal).</span><span class="sxs-lookup"><span data-stu-id="9a6cd-118">This Twilio Credit can be applied to any Twilio usage ($10 credit equivalent to sending as many as 1,000 SMS messages or receiving up to 1000 inbound Voice minutes, depending on the location of your phone number and message or call destination).</span></span> <span data-ttu-id="9a6cd-119">Lösa in den här Twilio-kredit och kom igång på: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span><span class="sxs-lookup"><span data-stu-id="9a6cd-119">Redeem this Twilio credit and get started at: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span></span>

<span data-ttu-id="9a6cd-120">Twilio är en betalning per användning.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="9a6cd-121">Det finns inga avgifter för installation och du kan stänga ditt konto när som helst.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-121">There are no set-up fees and you can close your account at any time.</span></span> <span data-ttu-id="9a6cd-122">Du hittar mer information i [Twilio priser][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="9a6cd-122">You can find more details at [Twilio Pricing][twilio_pricing].</span></span>

## <span data-ttu-id="9a6cd-123"><a id="Concepts"></a>Begrepp</span><span class="sxs-lookup"><span data-stu-id="9a6cd-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="9a6cd-124">Twilio-API är en RESTful-API som tillhandahåller röst- och SMS-funktioner för program.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-124">The Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="9a6cd-125">Klientbibliotek är tillgängliga på flera språk. en lista, se [Twilio-API-bibliotek][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="9a6cd-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="9a6cd-126">Viktiga aspekter av Twilio-API: et är Twilio verb och Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="9a6cd-126">Key aspects of the Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="9a6cd-127"><a id="Verbs"></a>Twilio-verb</span><span class="sxs-lookup"><span data-stu-id="9a6cd-127"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="9a6cd-128">API: et tillämpar Twilio verb; till exempel den  **&lt;säg&gt;**  verb instruerar Twilio att hörbart leverera ett meddelande på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-128">The API makes use of Twilio verbs; for example, the **&lt;Say&gt;** verb instructs Twilio to audibly deliver a message on a call.</span></span>

<span data-ttu-id="9a6cd-129">Följande är en lista över Twilio verb.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-129">The following is a list of Twilio verbs.</span></span> <span data-ttu-id="9a6cd-130">Lär dig mer om vilka verb och funktioner via [Markup Language Twilio dokumentationen](http://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="9a6cd-130">Learn about the other verbs and capabilities via [Twilio Markup Language documentation](http://www.twilio.com/docs/api/twiml).</span></span>

* <span data-ttu-id="9a6cd-131">**&lt;Ring&gt;**: anroparen ansluter till en annan telefon.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-131">**&lt;Dial&gt;**: Connects the caller to another phone.</span></span>
* <span data-ttu-id="9a6cd-132">**&lt;Samla in&gt;**: samlar in siffror som anges på telefon tangentbordet.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-132">**&lt;Gather&gt;**: Collects numeric digits entered on the telephone keypad.</span></span>
* <span data-ttu-id="9a6cd-133">**&lt;Koppla ned&gt;**: slutar ett anrop.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="9a6cd-134">**&lt;Spela upp&gt;**: spelar en ljudfil.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-134">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="9a6cd-135">**&lt;Pausa&gt;**: tyst väntar på ett angivet antal sekunder.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-135">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="9a6cd-136">**&lt;Posten&gt;**: registrerar anroparens röst och returnerar en URL för en fil som innehåller inspelningen.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-136">**&lt;Record&gt;**: Records the caller's voice and returns a URL of a file that contains the recording.</span></span>
* <span data-ttu-id="9a6cd-137">**&lt;Omdirigera&gt;**: Överför kontroll över ett samtal eller SMS till TwiML på en annan URL.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-137">**&lt;Redirect&gt;**: Transfers control of a call or SMS to the TwiML at a different URL.</span></span>
* <span data-ttu-id="9a6cd-138">**&lt;Avvisa&gt;**: avvisar ett inkommande samtal till Twilio-nummer utan fakturering du</span><span class="sxs-lookup"><span data-stu-id="9a6cd-138">**&lt;Reject&gt;**: Rejects an incoming call to your Twilio number without billing you</span></span>
* <span data-ttu-id="9a6cd-139">**&lt;Säg&gt;**: konverterar text till tal, som görs på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-139">**&lt;Say&gt;**: Converts text to speech that is made on a call.</span></span>
* <span data-ttu-id="9a6cd-140">**&lt;SMS&gt;**: skickar ett SMS-meddelande.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-140">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="9a6cd-141"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="9a6cd-141"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="9a6cd-142">TwiML är en uppsättning XML-baserade instruktioner baserat på de Twilio-verb som informerar Twilio för att behandla ett samtal eller SMS.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-142">TwiML is a set of XML-based instructions based on the Twilio verbs that inform Twilio of how to process a call or SMS.</span></span>

<span data-ttu-id="9a6cd-143">Följande TwiML skulle exempelvis Omvandla text **Hello World** till tal.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-143">As an example, the following TwiML would convert the text **Hello World** to speech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

<span data-ttu-id="9a6cd-144">När programmet anropar Twilio-API, är en av parametrarna API den URL som returnerar TwiML-svar.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-144">When your application calls the Twilio API, one of the API parameters is the URL that returns the TwiML response.</span></span> <span data-ttu-id="9a6cd-145">Du kan använda Twilio-tillhandahållna URL: er för utveckling, ange TwiML-svar som används av dina program.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-145">For development purposes, you can use Twilio-provided URLs to provide the TwiML responses used by your applications.</span></span> <span data-ttu-id="9a6cd-146">Du kan också vara värd för din egen URL: er för att skapa TwiML-svar och ett annat alternativ är att använda den **TwiMLResponse** objekt.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-146">You could also host your own URLs to produce the TwiML responses, and another option is to use the **TwiMLResponse** object.</span></span>

<span data-ttu-id="9a6cd-147">Läs mer om Twilio verb, deras attribut och TwiML [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="9a6cd-147">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="9a6cd-148">Mer information om Twilio-API finns [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="9a6cd-148">For additional information about the Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="9a6cd-149"><a id="CreateAccount"></a>Skapa ett Twilio-konto</span><span class="sxs-lookup"><span data-stu-id="9a6cd-149"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="9a6cd-150">När du är redo att skaffa ett Twilio-konto kan logga på [försök Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="9a6cd-150">When you're ready to get a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="9a6cd-151">Du kan börja med ett kostnadsfritt konto och uppgradera ditt konto senare.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-151">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="9a6cd-152">När du registrerar dig för ett Twilio-konto får du ett konto-ID och ett token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-152">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="9a6cd-153">Både behövs för att göra Twilio API-anrop.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-153">Both will be needed to make Twilio API calls.</span></span> <span data-ttu-id="9a6cd-154">För att förhindra obehörig åtkomst till ditt konto, skydda din token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-154">To prevent unauthorized access to your account, keep your authentication token secure.</span></span> <span data-ttu-id="9a6cd-155">Ditt konto-ID och autentisering token kan visas på den [Twilio kontosida][twilio_account], i fälten med etiketten **konto SID** och **AUTH TOKEN**respektive.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-155">Your account ID and authentication token are viewable at the [Twilio account page][twilio_account], in the fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="9a6cd-156"><a id="create_app"></a>Skapa en PHP-program</span><span class="sxs-lookup"><span data-stu-id="9a6cd-156"><a id="create_app"></a>Create a PHP Application</span></span>
<span data-ttu-id="9a6cd-157">Ett PHP-program som använder tjänsten Twilio och körs i Azure är inte annorlunda än andra PHP-program som använder tjänsten Twilio.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-157">A PHP application that uses the Twilio service and is running in Azure is no different than any other PHP application that uses the Twilio service.</span></span> <span data-ttu-id="9a6cd-158">När Twilio är REST-baserad och kan anropas från PHP på flera sätt, den här artikeln fokuserar på hur du använder Twilio-tjänster med [Twilio-biblioteket för PHP från GitHub][twilio_php].</span><span class="sxs-lookup"><span data-stu-id="9a6cd-158">While Twilio services are REST-based and can be called from PHP in several ways, this article will focus on how to use Twilio services with [Twilio library for PHP from GitHub][twilio_php].</span></span> <span data-ttu-id="9a6cd-159">Mer information om hur du använder Twilio-biblioteket för PHP finns [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].</span><span class="sxs-lookup"><span data-stu-id="9a6cd-159">For more information about using the Twilio library for PHP, see [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].</span></span>

<span data-ttu-id="9a6cd-160">Detaljerade anvisningar för att skapa och distribuera ett Twilio/PHP-program till Azure finns på [hur du gör ett telefonsamtal med Twilio i ett PHP-program på Azure][howto_phonecall_php].</span><span class="sxs-lookup"><span data-stu-id="9a6cd-160">Detailed instructions for building and deploying a Twilio/PHP application to Azure are available at [How to Make a Phone Call Using Twilio in a PHP Application on Azure][howto_phonecall_php].</span></span>

## <span data-ttu-id="9a6cd-161"><a id="configure_app"></a>Konfigurera programmet att använda Twilio-bibliotek</span><span class="sxs-lookup"><span data-stu-id="9a6cd-161"><a id="configure_app"></a>Configure Your Application to Use Twilio Libraries</span></span>
<span data-ttu-id="9a6cd-162">Du kan konfigurera tillämpningsprogrammet till att använda Twilio-biblioteket för PHP på två sätt:</span><span class="sxs-lookup"><span data-stu-id="9a6cd-162">You can configure your application to use the Twilio library for PHP in two ways:</span></span>

1. <span data-ttu-id="9a6cd-163">Hämta Twilio-biblioteket för PHP från GitHub ([https://github.com/twilio/twilio-php][twilio_php]) och Lägg till den **Services** katalogen i ditt program.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-163">Download the Twilio library for PHP from GitHub ([https://github.com/twilio/twilio-php][twilio_php]) and add the **Services** directory to your application.</span></span>
   
    <span data-ttu-id="9a6cd-164">ELLER</span><span class="sxs-lookup"><span data-stu-id="9a6cd-164">-OR-</span></span>
2. <span data-ttu-id="9a6cd-165">Installera Twilio-biblioteket för PHP som PÄRONTRÄD paket.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-165">Install the Twilio library for PHP as a PEAR package.</span></span> <span data-ttu-id="9a6cd-166">Den kan installeras med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="9a6cd-166">It can be installed with the following commands:</span></span>
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

<span data-ttu-id="9a6cd-167">När du har installerat Twilio-biblioteket för PHP, du kan sedan lägga till en **require_once** uttryck överst i PHP-filer för att referera till biblioteket:</span><span class="sxs-lookup"><span data-stu-id="9a6cd-167">Once you have installed the Twilio library for PHP, you can then add a **require_once** statement at the top of your PHP files to reference the library:</span></span>

        require_once 'Services/Twilio.php';

<span data-ttu-id="9a6cd-168">Mer information finns i [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].</span><span class="sxs-lookup"><span data-stu-id="9a6cd-168">For more information, see [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].</span></span>

## <span data-ttu-id="9a6cd-169"><a id="howto_make_call"></a>Så här: göra en utgående anrop</span><span class="sxs-lookup"><span data-stu-id="9a6cd-169"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="9a6cd-170">Följande visar hur du gör en utgående Ring upp med den **Services_Twilio** klass.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-170">The following shows how to make an outgoing call using the **Services_Twilio** class.</span></span> <span data-ttu-id="9a6cd-171">Den här koden används även en webbplats med angivna Twilio för att returnera svaret Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="9a6cd-171">This code also uses a Twilio-provided site to return the Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="9a6cd-172">Ersätt värdena för den **från** och **till** telefonnummer och se till att du kontrollerar den **från** telefonnummer för ditt Twilio-konto innan du kör koden.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-172">Substitute your values for the **From** and **To** phone numbers, and ensure that you verify the **From** phone number for your Twilio account prior to running the code.</span></span>

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
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

<span data-ttu-id="9a6cd-173">Som tidigare nämnts kan används den här koden en angivna Twilio plats för att returnera TwiML svaret.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-173">As mentioned, this code uses a Twilio-provided site to return the TwiML response.</span></span> <span data-ttu-id="9a6cd-174">Du kan använda din egen webbplats i stället för att tillhandahålla TwiML svaret; Mer information finns i [hur du ger TwiML svar från din egen webbplats](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="9a6cd-174">You could instead use your own site to provide the TwiML response; for more information, see [How to Provide TwiML Responses from Your Own Web Site](#howto_provide_twiml_responses).</span></span>

* <span data-ttu-id="9a6cd-175">**Obs**: Om du vill felsöka verifieringsfel för SSL-certifikat finns [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]</span><span class="sxs-lookup"><span data-stu-id="9a6cd-175">**Note**: To troubleshoot SSL certificate validation errors, see [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]</span></span> 

## <span data-ttu-id="9a6cd-176"><a id="howto_send_sms"></a>Så här: skicka ett SMS-meddelande</span><span class="sxs-lookup"><span data-stu-id="9a6cd-176"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="9a6cd-177">Här visas hur du skickar ett SMS-meddelande med den **Services_Twilio** klass.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-177">The following shows how to send an SMS message using the **Services_Twilio** class.</span></span> <span data-ttu-id="9a6cd-178">Den **från** nummer tillhandahålls av Twilio för utvärderingskonton att skicka SMS-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-178">The **From** number is provided by Twilio for trial accounts to send SMS messages.</span></span> <span data-ttu-id="9a6cd-179">Den **till** numret måste verifieras för ditt Twilio-konto innan du kör koden.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-179">The **To** number must be verified for your Twilio account prior to running the code.</span></span>

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <span data-ttu-id="9a6cd-180"><a id="howto_provide_twiml_responses"></a>Så här: Ange TwiML svar från din egen webbplats</span><span class="sxs-lookup"><span data-stu-id="9a6cd-180"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="9a6cd-181">När ditt program initierar ett anrop till Twilio-API, skickar Twilio din begäran till en URL som förväntas returnera ett TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-181">When your application initiates a call to the Twilio API, Twilio will send your request to a URL that is expected to return a TwiML response.</span></span> <span data-ttu-id="9a6cd-182">I exemplet ovan används URL: en som tillhandahålls av Twilio [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="9a6cd-182">The example above uses the Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="9a6cd-183">(När TwiML är avsedd för användning av Twilio, du kan visa it i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-183">(While TwiML is designed for use by Twilio, you can view the it in your browser.</span></span> <span data-ttu-id="9a6cd-184">Klicka till exempel [http://twimlets.com/message] [ twimlet_message_url] att se en tom `<Response>` element; Klicka på ett annat exempel är [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] att se en `<Response>` element som innehåller en `<Say>` element.)</span><span class="sxs-lookup"><span data-stu-id="9a6cd-184">For example, click [http://twimlets.com/message][twimlet_message_url] to see an empty `<Response>` element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] to see a `<Response>` element that contains a `<Say>` element.)</span></span>

<span data-ttu-id="9a6cd-185">I stället för på den angivna Twilio URL, kan du skapa din egen webbplats som returnerar HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-185">Instead of relying on the Twilio-provided URL, you can create your own site that returns HTTP responses.</span></span> <span data-ttu-id="9a6cd-186">Du kan skapa webbplatsen på alla språk som returnerar XML-svar; Det här avsnittet förutsätter att du använder PHP för att skapa TwiML.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-186">You can create the site in any language that returns XML responses; this topic assumes you'll be using PHP to create the TwiML.</span></span>

<span data-ttu-id="9a6cd-187">Sidan följande PHP resulterar i ett TwiML-svar som säger **Hello World** på anropet.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-187">The following PHP page results in a TwiML response that says **Hello World** on the call.</span></span>

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

<span data-ttu-id="9a6cd-188">Du ser i exemplet ovan, är TwiML svaret bara ett XML-dokument.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-188">As you can see from the example above, the TwiML response is simply an XML document.</span></span> <span data-ttu-id="9a6cd-189">Twilio-bibliotek för PHP innehåller klasser som ska generera TwiML för dig.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-189">The Twilio library for PHP contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="9a6cd-190">Exemplet nedan skapar motsvarande svaret som ovan, men använder den **Services\_Twilio\_Twiml** klass i Twilio-biblioteket för PHP:</span><span class="sxs-lookup"><span data-stu-id="9a6cd-190">The example below produces the equivalent response as shown above, but uses the **Services\_Twilio\_Twiml** class in the Twilio library for PHP:</span></span>

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

<span data-ttu-id="9a6cd-191">Läs mer om TwiML [https://www.twilio.com/docs/api/twiml][twiml_reference].</span><span class="sxs-lookup"><span data-stu-id="9a6cd-191">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml][twiml_reference].</span></span> 

<span data-ttu-id="9a6cd-192">När du har din PHP-sida som ställts in för att ange TwiML svar använder URL för PHP-sidan som URL som skickades till den `Services_Twilio->account->calls->create` metoden.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-192">Once you have your PHP page set up to provide TwiML responses, use the URL of the PHP page as the URL passed into the  `Services_Twilio->account->calls->create`  method.</span></span> <span data-ttu-id="9a6cd-193">Till exempel om du har ett webbprogram med namnet **MyTwiML** distribueras till en Azure-värdtjänsten och namnet på sidan PHP är **mytwiml.php**, URL: en kan skickas till **Services_Twilio -> konto -> anrop -> Skapa** som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="9a6cd-193">For example, if you have a Web application named **MyTwiML** deployed to an Azure hosted service, and the name of the PHP page is **mytwiml.php**, the URL can be passed to  **Services_Twilio->account->calls->create**  as shown in the following example:</span></span>

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
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

<span data-ttu-id="9a6cd-194">Mer information om hur du använder Twilio i Azure med PHP finns [hur du gör ett telefonsamtal med Twilio i ett PHP-program på Azure][howto_phonecall_php].</span><span class="sxs-lookup"><span data-stu-id="9a6cd-194">For additional information about using Twilio in Azure with PHP, see [How to Make a Phone Call Using Twilio in a PHP Application on Azure][howto_phonecall_php].</span></span>

## <span data-ttu-id="9a6cd-195"><a id="AdditionalServices"></a>Så här: använda ytterligare Twilio-tjänster</span><span class="sxs-lookup"><span data-stu-id="9a6cd-195"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="9a6cd-196">Förutom de exempel som visas här, erbjuder Twilio webbaserade API: er som du kan använda för att utnyttja ytterligare funktioner för Twilio från Azure-program.</span><span class="sxs-lookup"><span data-stu-id="9a6cd-196">In addition to the examples shown here, Twilio offers web-based APIs that you can use to leverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="9a6cd-197">Fullständig information finns i [Twilio-API-dokumentationen][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="9a6cd-197">For full details, see the [Twilio API documentation][twilio_api_documentation].</span></span>

## <span data-ttu-id="9a6cd-198"><a id="NextSteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9a6cd-198"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="9a6cd-199">Nu när du har lärt dig grunderna i Twilio-tjänsten kan du följa dessa länkar om du vill veta mer:</span><span class="sxs-lookup"><span data-stu-id="9a6cd-199">Now that you've learned the basics of the Twilio service, follow these links to learn more:</span></span>

* <span data-ttu-id="9a6cd-200">[Riktlinjer för Twilio-säkerhet][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="9a6cd-200">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="9a6cd-201">[Twilio ta och exempelkod][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="9a6cd-201">[Twilio HowTo's and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="9a6cd-202">[Twilio snabbstarten självstudier][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="9a6cd-202">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span> 
* <span data-ttu-id="9a6cd-203">[Twilio på GitHub][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="9a6cd-203">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="9a6cd-204">[Prata med Twilio-Support][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="9a6cd-204">[Talk to Twilio Support][twilio_support]</span></span>

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
