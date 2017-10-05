---
title: "Hur du använder Twilio för röst- och SMS (Python) | Microsoft Docs"
description: "Lär dig att ringa ett telefonsamtal och skicka ett SMS-meddelande med Twilio-API-tjänsten på Azure. Kodexempel som skrivits i Python."
services: 
documentationcenter: python
author: devinrader
manager: twilio
editor: 
ms.assetid: 561bc75b-4ac4-40ba-bcba-48e901f27cc3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: f4a02bb7a7c46e7a0e3c75b870c522eae8294339
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-python"></a><span data-ttu-id="87ce0-104">Hur du använder Twilio för röst- och SMS-funktioner i Python</span><span class="sxs-lookup"><span data-stu-id="87ce0-104">How to Use Twilio for Voice and SMS Capabilities in Python</span></span>
<span data-ttu-id="87ce0-105">Den här guiden visar hur du utför vanliga programmeringsuppgifter med Twilio-API-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="87ce0-105">This guide demonstrates how to perform common programming tasks with the Twilio API service on Azure.</span></span> <span data-ttu-id="87ce0-106">Scenarier som tas upp inkluderar att ringa och ett kort meddelande (SMS Service)-meddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="87ce0-106">The scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="87ce0-107">Mer information om Twilio och använder röst- och SMS i dina program finns i [nästa steg](#NextSteps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="87ce0-107">For more information on Twilio and using voice and SMS in your applications, see the [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="87ce0-108"><a id="WhatIs"></a>Vad är Twilio?</span><span class="sxs-lookup"><span data-stu-id="87ce0-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="87ce0-109">Twilio startar framtiden för kommunikation med, så att utvecklare kan bädda in röst, VoIP och meddelanden i program.</span><span class="sxs-lookup"><span data-stu-id="87ce0-109">Twilio is powering the future of business communications, enabling developers to embed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="87ce0-110">De virtualisera alla infrastruktur som behövs i en molnbaserad, globala miljö, exponera den via Twilio kommunikation API-plattformen.</span><span class="sxs-lookup"><span data-stu-id="87ce0-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through the Twilio communications API platform.</span></span> <span data-ttu-id="87ce0-111">Programmen är enkla att skapa och skalbara.</span><span class="sxs-lookup"><span data-stu-id="87ce0-111">Applications are simple to build and scalable.</span></span> <span data-ttu-id="87ce0-112">Få flexibilitet med lön-som-dig gå priser och dra nytta av molnet tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="87ce0-112">Enjoy flexibility with pay-as-you go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="87ce0-113">**Twilio röst** gör att dina program att göra och ta emot telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="87ce0-113">**Twilio Voice** allows your applications to make and receive phone calls.</span></span>
<span data-ttu-id="87ce0-114">**Twilio SMS** gör att programmet kan skicka och ta emot textmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="87ce0-114">**Twilio SMS** enables your application to send and receive text messages.</span></span>
<span data-ttu-id="87ce0-115">**Twilio klienten** kan du se VoIP-anrop från telefon, surfplatta eller webbläsare och stöder WebRTC.</span><span class="sxs-lookup"><span data-stu-id="87ce0-115">**Twilio Client** allows you to make VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="87ce0-116"><a id="Pricing"></a>Priser för Twilio och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="87ce0-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="87ce0-117">Azure-kunder får en [specialerbjudande] [ special_offer] $10 Twilio kredit när du uppgraderar ditt Twilio-konto.</span><span class="sxs-lookup"><span data-stu-id="87ce0-117">Azure customers receive a [special offer][special_offer] $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="87ce0-118">Den här Twilio-kredit kan tillämpas på alla Twilio-användning ($10 kredit motsvarar upp till 1 000 SMS-meddelanden skickas eller tas emot upp till 1 000 inkommande röst minuter beroende på platsen för ditt mål för telefon och meddelandet eller samtal).</span><span class="sxs-lookup"><span data-stu-id="87ce0-118">This Twilio Credit can be applied to any Twilio usage ($10 credit equivalent to sending as many as 1,000 SMS messages or receiving up to 1000 inbound Voice minutes, depending on the location of your phone number and message or call destination).</span></span> <span data-ttu-id="87ce0-119">Lösa detta [Twilio kredit] [ special_offer] och komma igång.</span><span class="sxs-lookup"><span data-stu-id="87ce0-119">Redeem this [Twilio credit][special_offer] and get started.</span></span>

<span data-ttu-id="87ce0-120">Twilio är en betalning per användning.</span><span class="sxs-lookup"><span data-stu-id="87ce0-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="87ce0-121">Det finns inga avgifter för installation och du kan stänga ditt konto när som helst.</span><span class="sxs-lookup"><span data-stu-id="87ce0-121">There are no set-up fees and you can close your account at any time.</span></span> <span data-ttu-id="87ce0-122">Du hittar mer information i [Twilio priser][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="87ce0-122">You can find more details at [Twilio Pricing][twilio_pricing].</span></span>

## <span data-ttu-id="87ce0-123"><a id="Concepts"></a>Begrepp</span><span class="sxs-lookup"><span data-stu-id="87ce0-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="87ce0-124">Twilio-API är en RESTful-API som tillhandahåller röst- och SMS-funktioner för program.</span><span class="sxs-lookup"><span data-stu-id="87ce0-124">The Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="87ce0-125">Klientbibliotek är tillgängliga på flera språk. en lista, se [Twilio-API-bibliotek][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="87ce0-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="87ce0-126">Viktiga aspekter av Twilio-API: et är Twilio verb och Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="87ce0-126">Key aspects of the Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="87ce0-127"><a id="Verbs"></a>Twilio-verb</span><span class="sxs-lookup"><span data-stu-id="87ce0-127"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="87ce0-128">API: et tillämpar Twilio verb; till exempel den  **&lt;säg&gt;**  verb instruerar Twilio att hörbart leverera ett meddelande på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="87ce0-128">The API makes use of Twilio verbs; for example, the **&lt;Say&gt;** verb instructs Twilio to audibly deliver a message on a call.</span></span>

<span data-ttu-id="87ce0-129">Följande är en lista över Twilio verb.</span><span class="sxs-lookup"><span data-stu-id="87ce0-129">The following is a list of Twilio verbs.</span></span> <span data-ttu-id="87ce0-130">Lär dig mer om vilka verb och funktioner via [Markup Language Twilio dokumentationen][twiml].</span><span class="sxs-lookup"><span data-stu-id="87ce0-130">Learn about the other verbs and capabilities via [Twilio Markup Language documentation][twiml].</span></span>

* <span data-ttu-id="87ce0-131">**&lt;Ring&gt;**: anroparen ansluter till en annan telefon.</span><span class="sxs-lookup"><span data-stu-id="87ce0-131">**&lt;Dial&gt;**: Connects the caller to another phone.</span></span>
* <span data-ttu-id="87ce0-132">**&lt;Samla in&gt;**: samlar in siffror som anges på telefon tangentbordet.</span><span class="sxs-lookup"><span data-stu-id="87ce0-132">**&lt;Gather&gt;**: Collects numeric digits entered on the telephone keypad.</span></span>
* <span data-ttu-id="87ce0-133">**&lt;Koppla ned&gt;**: slutar ett anrop.</span><span class="sxs-lookup"><span data-stu-id="87ce0-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="87ce0-134">**&lt;Pausa&gt;**: tyst väntar på ett angivet antal sekunder.</span><span class="sxs-lookup"><span data-stu-id="87ce0-134">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="87ce0-135">**&lt;Spela upp&gt;**: spelar en ljudfil.</span><span class="sxs-lookup"><span data-stu-id="87ce0-135">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="87ce0-136">**&lt;Kön&gt;**: lägga till den till en kö med anropare.</span><span class="sxs-lookup"><span data-stu-id="87ce0-136">**&lt;Queue&gt;**: Add the to a queue of callers.</span></span>
* <span data-ttu-id="87ce0-137">**&lt;Posten&gt;**: registrerar röst anroparen och returnerar en URL för en fil som innehåller inspelningen.</span><span class="sxs-lookup"><span data-stu-id="87ce0-137">**&lt;Record&gt;**: Records the voice of the caller and returns a URL of a file that contains the recording.</span></span>
* <span data-ttu-id="87ce0-138">**&lt;Omdirigera&gt;**: Överför kontroll över ett samtal eller SMS till TwiML på en annan URL.</span><span class="sxs-lookup"><span data-stu-id="87ce0-138">**&lt;Redirect&gt;**: Transfers control of a call or SMS to the TwiML at a different URL.</span></span>
* <span data-ttu-id="87ce0-139">**&lt;Avvisa&gt;**: avvisar ett inkommande samtal till Twilio-nummer utan fakturering du.</span><span class="sxs-lookup"><span data-stu-id="87ce0-139">**&lt;Reject&gt;**: Rejects an incoming call to your Twilio number without billing you.</span></span>
* <span data-ttu-id="87ce0-140">**&lt;Säg&gt;**: konverterar text till tal, som görs på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="87ce0-140">**&lt;Say&gt;**: Converts text to speech that is made on a call.</span></span>
* <span data-ttu-id="87ce0-141">**&lt;SMS&gt;**: skickar ett SMS-meddelande.</span><span class="sxs-lookup"><span data-stu-id="87ce0-141">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="87ce0-142"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="87ce0-142"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="87ce0-143">TwiML är en uppsättning XML-baserade instruktioner baserat på de Twilio-verb som informerar Twilio för att behandla ett samtal eller SMS.</span><span class="sxs-lookup"><span data-stu-id="87ce0-143">TwiML is a set of XML-based instructions based on the Twilio verbs that inform Twilio of how to process a call or SMS.</span></span>

<span data-ttu-id="87ce0-144">Följande TwiML skulle exempelvis Omvandla text **Hello World** till tal.</span><span class="sxs-lookup"><span data-stu-id="87ce0-144">As an example, the following TwiML would convert the text **Hello World** to speech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

<span data-ttu-id="87ce0-145">När programmet anropar Twilio-API, är en av parametrarna API den URL som returnerar TwiML-svar.</span><span class="sxs-lookup"><span data-stu-id="87ce0-145">When your application calls the Twilio API, one of the API parameters is the URL that returns the TwiML response.</span></span> <span data-ttu-id="87ce0-146">Du kan använda Twilio-tillhandahållna URL: er för utveckling, ange TwiML-svar som används av dina program.</span><span class="sxs-lookup"><span data-stu-id="87ce0-146">For development purposes, you can use Twilio-provided URLs to provide the TwiML responses used by your applications.</span></span> <span data-ttu-id="87ce0-147">Du kan också vara värd för din egen URL: er för att skapa TwiML-svar och ett annat alternativ är att använda den `TwiMLResponse` objekt.</span><span class="sxs-lookup"><span data-stu-id="87ce0-147">You could also host your own URLs to produce the TwiML responses, and another option is to use the `TwiMLResponse` object.</span></span>

<span data-ttu-id="87ce0-148">Läs mer om Twilio verb, deras attribut och TwiML [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="87ce0-148">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="87ce0-149">Mer information om Twilio-API finns [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="87ce0-149">For additional information about the Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="87ce0-150"><a id="CreateAccount"></a>Skapa ett Twilio-konto</span><span class="sxs-lookup"><span data-stu-id="87ce0-150"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="87ce0-151">När du är redo att skaffa ett Twilio-konto kan logga på [försök Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="87ce0-151">When you are ready to get a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="87ce0-152">Du kan börja med ett kostnadsfritt konto och uppgradera ditt konto senare.</span><span class="sxs-lookup"><span data-stu-id="87ce0-152">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="87ce0-153">När du registrerar dig för ett Twilio-konto får du ett SID-konto och en token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="87ce0-153">When you sign up for a Twilio account, you receive an account SID and an authentication token.</span></span> <span data-ttu-id="87ce0-154">Både behövs för att göra Twilio API-anrop.</span><span class="sxs-lookup"><span data-stu-id="87ce0-154">Both will be needed to make Twilio API calls.</span></span> <span data-ttu-id="87ce0-155">För att förhindra obehörig åtkomst till ditt konto, skydda din token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="87ce0-155">To prevent unauthorized access to your account, keep your authentication token secure.</span></span> <span data-ttu-id="87ce0-156">Ditt konto SID och autentiseringstoken kan visas i den [Twilio konsolen][twilio_console], i fälten med etiketten **konto SID** och **AUTH TOKEN**respektive.</span><span class="sxs-lookup"><span data-stu-id="87ce0-156">Your account SID and authentication token are viewable in the [Twilio Console][twilio_console], in the fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="87ce0-157"><a id="create_app"></a>Skapa en Python-program</span><span class="sxs-lookup"><span data-stu-id="87ce0-157"><a id="create_app"></a>Create a Python Application</span></span>
<span data-ttu-id="87ce0-158">En Python-program som använder tjänsten Twilio och körs i Azure är inte annorlunda än andra Python-program som använder Twilio-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="87ce0-158">A Python application that uses the Twilio service and is running in Azure is no different than any other Python application that uses the Twilio service.</span></span> <span data-ttu-id="87ce0-159">När Twilio är REST-baserad och kan anropas från Python på flera sätt, den här artikeln fokuserar på hur du använder Twilio-tjänster med [Twilio-biblioteket för Python från GitHub][twilio_python].</span><span class="sxs-lookup"><span data-stu-id="87ce0-159">While Twilio services are REST-based and can be called from Python in several ways, this article will focus on how to use Twilio services with [Twilio library for Python from GitHub][twilio_python].</span></span> <span data-ttu-id="87ce0-160">Mer information om hur du använder Twilio-biblioteket för Python finns [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].</span><span class="sxs-lookup"><span data-stu-id="87ce0-160">For more information about using the Twilio library for Python, see [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].</span></span>

<span data-ttu-id="87ce0-161">Första [ställa in en ny Azure Linux VM] [azure_vm_setup] för att fungera som värd för din nya Python-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="87ce0-161">First, [set-up a new Azure Linux VM][azure_vm_setup] to act as a host for your new Python web application.</span></span> <span data-ttu-id="87ce0-162">När den virtuella datorn körs, måste du exponera dina program på en offentlig port som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="87ce0-162">Once the Virtual Machine is running, you will need to expose your application on a public port as described below.</span></span>

### <a name="add-an-incoming-rule"></a><span data-ttu-id="87ce0-163">Lägg till en inkommande regel</span><span class="sxs-lookup"><span data-stu-id="87ce0-163">Add An Incoming Rule</span></span>
  1. <span data-ttu-id="87ce0-164">Gå till sidan [Nätverkssäkerhetsgruppen] [azure_nsg].</span><span class="sxs-lookup"><span data-stu-id="87ce0-164">Go to the [Network Security Group][azure_nsg] page.</span></span>
  2. <span data-ttu-id="87ce0-165">Välj Nätverkssäkerhetsgruppen som överensstämmer med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="87ce0-165">Select the Network Security Group that corresponds with your Virtual Machine.</span></span>
  3. <span data-ttu-id="87ce0-166">Lägg till och **utgående regel** för **port 80**.</span><span class="sxs-lookup"><span data-stu-id="87ce0-166">Add and **Outgoing Rule** for **port 80**.</span></span> <span data-ttu-id="87ce0-167">Se till att tillåta inkommande från alla adresser.</span><span class="sxs-lookup"><span data-stu-id="87ce0-167">Be sure to allow incoming from any address.</span></span>

### <a name="set-the-dns-name-label"></a><span data-ttu-id="87ce0-168">Ange DNS-namnetikett</span><span class="sxs-lookup"><span data-stu-id="87ce0-168">Set the DNS Name Label</span></span>
  1. <span data-ttu-id="87ce0-169">Gå till sidan [den offentliga IP-postadresser] [azure_ips].</span><span class="sxs-lookup"><span data-stu-id="87ce0-169">Go to the [The Public IP Adresses][azure_ips] page.</span></span>
  2. <span data-ttu-id="87ce0-170">Välj offentlig IP-Adressen som correspends med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="87ce0-170">Select the Public IP that correspends with your Virtual Machine.</span></span>
  3. <span data-ttu-id="87ce0-171">Ange den **DNS-namnetikett** i den **Configuration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="87ce0-171">Set the **DNS Name Label** in the **Configuration** section.</span></span> <span data-ttu-id="87ce0-172">I det här exemplet den ser ut ungefär så här *din domän etikett*. centralus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="87ce0-172">In the case of this example it will look something like this *your-domain-label*.centralus.cloudapp.azure.com</span></span>

<span data-ttu-id="87ce0-173">När du ska kunna ansluta via SSH till den virtuella datorn kan du installera webbramverk önskat (de mest kända i Python som två [Flask](http://flask.pocoo.org/) och [Django](https://www.djangoproject.com)).</span><span class="sxs-lookup"><span data-stu-id="87ce0-173">Once you are able to connect through SSH to the Virtual Machine you can install the Web Framework of your choice (the two most well known in Python being [Flask](http://flask.pocoo.org/) and [Django](https://www.djangoproject.com)).</span></span> <span data-ttu-id="87ce0-174">Du kan installera någon av dem genom att köra den `pip install` kommando.</span><span class="sxs-lookup"><span data-stu-id="87ce0-174">You can install either of them just by running the `pip install` command.</span></span>

<span data-ttu-id="87ce0-175">Tänk på att vi har konfigurerat den virtuella datorn för att tillåta trafik på port 80.</span><span class="sxs-lookup"><span data-stu-id="87ce0-175">Keep in mind that we configured the Virtual Machine to allow traffic only on port 80.</span></span> <span data-ttu-id="87ce0-176">Så se till att konfigurera programmet att använda den här porten.</span><span class="sxs-lookup"><span data-stu-id="87ce0-176">So make sure to configure the application to use this port.</span></span>

## <span data-ttu-id="87ce0-177"><a id="configure_app"></a>Konfigurera programmet att använda Twilio-bibliotek</span><span class="sxs-lookup"><span data-stu-id="87ce0-177"><a id="configure_app"></a>Configure Your Application to Use Twilio Libraries</span></span>
<span data-ttu-id="87ce0-178">Du kan konfigurera tillämpningsprogrammet till att använda Twilio-biblioteket för Python på två sätt:</span><span class="sxs-lookup"><span data-stu-id="87ce0-178">You can configure your application to use the Twilio library for Python in two ways:</span></span>

* <span data-ttu-id="87ce0-179">Installera Twilio-biblioteket för Python som Pip-paket.</span><span class="sxs-lookup"><span data-stu-id="87ce0-179">Install the Twilio library for Python as a Pip package.</span></span> <span data-ttu-id="87ce0-180">Den kan installeras med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="87ce0-180">It can be installed with the following commands:</span></span>
   
        $ pip install twilio

    <span data-ttu-id="87ce0-181">ELLER</span><span class="sxs-lookup"><span data-stu-id="87ce0-181">-OR-</span></span>

* <span data-ttu-id="87ce0-182">Hämta Twilio-biblioteket för Python från GitHub ([https://github.com/twilio/twilio-python][twilio_python]) och installera den så här:</span><span class="sxs-lookup"><span data-stu-id="87ce0-182">Download the Twilio library for Python from GitHub ([https://github.com/twilio/twilio-python][twilio_python]) and install it like this:</span></span>

        $ python setup.py install

<span data-ttu-id="87ce0-183">När du har installerat Twilio-biblioteket för Python, kan du sedan `import` i Python-filer:</span><span class="sxs-lookup"><span data-stu-id="87ce0-183">Once you have installed the Twilio library for Python, you can then `import` it in your Python files:</span></span>

        import twilio

<span data-ttu-id="87ce0-184">Mer information finns i [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].</span><span class="sxs-lookup"><span data-stu-id="87ce0-184">For more information, see [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].</span></span>

## <span data-ttu-id="87ce0-185"><a id="howto_make_call"></a>Så här: göra en utgående anrop</span><span class="sxs-lookup"><span data-stu-id="87ce0-185"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="87ce0-186">Nedan visas hur du skapar ett utgående anrop.</span><span class="sxs-lookup"><span data-stu-id="87ce0-186">The following shows how to make an outgoing call.</span></span> <span data-ttu-id="87ce0-187">Den här koden används även en webbplats med angivna Twilio för att returnera svaret Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="87ce0-187">This code also uses a Twilio-provided site to return the Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="87ce0-188">Ersätt värdena för den **from_number** och **to_number** telefonnummer och se till att du har bekräftat att den **from_number** telefonnummer för ditt Twilio-konto innan du kör koden.</span><span class="sxs-lookup"><span data-stu-id="87ce0-188">Substitute your values for the **from_number** and **to_number** phone numbers, and ensure that you've verified the **from_number** phone number for your Twilio account before running the code.</span></span>

    from urllib.parse import urlencode

    # Import the Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from_number = "NNNNNNNNNNN"

    # The number of the phone receiving call.
    to_number = "NNNNNNNNNNN"

    # Use the Twilio-provided site for the TwiML response.
    url = "http://twimlets.com/message?"

    # The phone message text.
    message = "Hello world."

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make the call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url + urlencode({'Message': message}))
    print(call.sid)

<span data-ttu-id="87ce0-189">Som tidigare nämnts kan används den här koden en angivna Twilio plats för att returnera TwiML svaret.</span><span class="sxs-lookup"><span data-stu-id="87ce0-189">As mentioned, this code uses a Twilio-provided site to return the TwiML response.</span></span> <span data-ttu-id="87ce0-190">Du kan använda din egen webbplats i stället för att tillhandahålla TwiML svaret; Mer information finns i [hur du ger TwiML svar från din egen webbplats](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="87ce0-190">You could instead use your own site to provide the TwiML response; for more information, see [How to Provide TwiML Responses from Your Own Web Site](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="87ce0-191"><a id="howto_send_sms"></a>Så här: skicka ett SMS-meddelande</span><span class="sxs-lookup"><span data-stu-id="87ce0-191"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="87ce0-192">Här visas hur du skickar ett SMS-meddelande med den `TwilioRestClient` klass.</span><span class="sxs-lookup"><span data-stu-id="87ce0-192">The following shows how to send an SMS message using the `TwilioRestClient` class.</span></span> <span data-ttu-id="87ce0-193">Den **from_number** nummer tillhandahålls av Twilio för utvärderingskonton att skicka SMS-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="87ce0-193">The **from_number** number is provided by Twilio for trial accounts to send SMS messages.</span></span> <span data-ttu-id="87ce0-194">Den **to_number** numret måste verifieras för ditt Twilio-konto innan du kör koden.</span><span class="sxs-lookup"><span data-stu-id="87ce0-194">The **to_number** number must be verified for your Twilio account before running the code.</span></span>

    # Import the Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    from_number = "NNNNNNNNNNN"  # With trial account, texts can only be sent from your Twilio number.
    to_number = "NNNNNNNNNNN"
    message = "Hello world."

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Send the SMS message.
    message = client.messages.create(to=to_number,
                                     from_=from_number,
                                     body=message)

## <span data-ttu-id="87ce0-195"><a id="howto_provide_twiml_responses"></a>Så här: Ange TwiML svar från din egen webbplats</span><span class="sxs-lookup"><span data-stu-id="87ce0-195"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="87ce0-196">När ditt program initierar ett anrop till Twilio-API, skickar Twilio din begäran till en URL som förväntas returnera ett TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="87ce0-196">When your application initiates a call to the Twilio API, Twilio will send your request to a URL that is expected to return a TwiML response.</span></span> <span data-ttu-id="87ce0-197">I exemplet ovan används URL: en som tillhandahålls av Twilio [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="87ce0-197">The example above uses the Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="87ce0-198">(När TwiML är avsedd för användning av Twilio, kan du visa den i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="87ce0-198">(While TwiML is designed for use by Twilio, you can view it in your browser.</span></span> <span data-ttu-id="87ce0-199">Klicka till exempel [http://twimlets.com/message] [ twimlet_message_url] att se en tom `<Response>` element; Klicka på ett annat exempel är [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] att se en `<Response>` element som innehåller en `<Say>` element.)</span><span class="sxs-lookup"><span data-stu-id="87ce0-199">For example, click [http://twimlets.com/message][twimlet_message_url] to see an empty `<Response>` element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] to see a `<Response>` element that contains a `<Say>` element.)</span></span>

<span data-ttu-id="87ce0-200">I stället för på den angivna Twilio URL, kan du skapa din egen webbplats som returnerar HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="87ce0-200">Instead of relying on the Twilio-provided URL, you can create your own site that returns HTTP responses.</span></span> <span data-ttu-id="87ce0-201">Du kan skapa webbplatsen på alla språk som returnerar XML-svar; Det här avsnittet förutsätter att du kommer att använda Python för att skapa TwiML.</span><span class="sxs-lookup"><span data-stu-id="87ce0-201">You can create the site in any language that returns XML responses; this topic assumes you will be using Python to create the TwiML.</span></span>

<span data-ttu-id="87ce0-202">I följande exempel kommer utdata TwiML svar som säger **Hello World** på anropet.</span><span class="sxs-lookup"><span data-stu-id="87ce0-202">The following examples will output a TwiML response that says **Hello World** on the call.</span></span>

<span data-ttu-id="87ce0-203">Med Flask:</span><span class="sxs-lookup"><span data-stu-id="87ce0-203">With Flask:</span></span>

    from flask import Response
    @app.route("/")
    def hello():
        xml = '<Response><Say>Hello world.</Say></Response>'
        return Response(xml, mimetype='text/xml')

<span data-ttu-id="87ce0-204">Med Django:</span><span class="sxs-lookup"><span data-stu-id="87ce0-204">With Django:</span></span>

    from django.http import HttpResponse
    def hello(request):
        xml = '<Response><Say>Hello world.</Say></Response>'
        return HttpResponse(xml, content_type='text/xml')

<span data-ttu-id="87ce0-205">Du ser i exemplet ovan, är TwiML svaret bara ett XML-dokument.</span><span class="sxs-lookup"><span data-stu-id="87ce0-205">As you can see from the example above, the TwiML response is simply an XML document.</span></span> <span data-ttu-id="87ce0-206">Twilio-bibliotek för Python innehåller klasser som ska generera TwiML för dig.</span><span class="sxs-lookup"><span data-stu-id="87ce0-206">The Twilio library for Python contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="87ce0-207">Exemplet nedan skapar motsvarande svaret som ovan, men använder den `twiml` modul i Twilio-biblioteket för Python:</span><span class="sxs-lookup"><span data-stu-id="87ce0-207">The example below produces the equivalent response as shown above, but uses the `twiml` module in the Twilio library for Python:</span></span>

    from twilio import twiml

    response = twiml.Response()
    response.say("Hello world.")
    print(str(response))

<span data-ttu-id="87ce0-208">Läs mer om TwiML [https://www.twilio.com/docs/api/twiml][twiml_reference].</span><span class="sxs-lookup"><span data-stu-id="87ce0-208">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml][twiml_reference].</span></span>

<span data-ttu-id="87ce0-209">När du har Python programmet ställts in för att ange TwiML svar använder URL för programmet som URL som skickades till den `client.calls.create` metoden.</span><span class="sxs-lookup"><span data-stu-id="87ce0-209">Once you have your Python application set up to provide TwiML responses, use the URL of the application as the URL passed into the `client.calls.create`  method.</span></span> <span data-ttu-id="87ce0-210">Till exempel om du har ett webbprogram med namnet **MyTwiML** distribueras till en Azure-värdtjänsten tjänsten, kan du använda webbadressen som webhook som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="87ce0-210">For example, if you have a Web application named **MyTwiML** deployed to an Azure hosted service, you can use its url as webhook as shown in the following example:</span></span>

    from twilio.rest import TwilioRestClient

    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"
    from_number = "NNNNNNNNNNN"
    to_number = "NNNNNNNNNNN"
    url = "http://your-domain-label.centralus.cloudapp.azure.com/MyTwiML/"

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make the call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url)
    print(call.sid)

## <span data-ttu-id="87ce0-211"><a id="AdditionalServices"></a>Så här: använda ytterligare Twilio-tjänster</span><span class="sxs-lookup"><span data-stu-id="87ce0-211"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="87ce0-212">Förutom de exempel som visas här, erbjuder Twilio webbaserade API: er som du kan använda för att utnyttja ytterligare funktioner för Twilio från Azure-program.</span><span class="sxs-lookup"><span data-stu-id="87ce0-212">In addition to the examples shown here, Twilio offers web-based APIs that you can use to leverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="87ce0-213">Fullständig information finns i [Twilio-API-dokumentationen][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="87ce0-213">For full details, see the [Twilio API documentation][twilio_api].</span></span>

## <span data-ttu-id="87ce0-214"><a id="NextSteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="87ce0-214"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="87ce0-215">Nu när du har lärt dig grunderna i Twilio-tjänsten kan du följa dessa länkar om du vill veta mer:</span><span class="sxs-lookup"><span data-stu-id="87ce0-215">Now that you have learned the basics of the Twilio service, follow these links to learn more:</span></span>

* <span data-ttu-id="87ce0-216">[Riktlinjer för Twilio-säkerhet][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="87ce0-216">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="87ce0-217">[Twilio ta guider och exempelkod][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="87ce0-217">[Twilio HowTo Guides and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="87ce0-218">[Twilio snabbstarten självstudier][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="87ce0-218">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span>
* <span data-ttu-id="87ce0-219">[Twilio på GitHub][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="87ce0-219">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="87ce0-220">[Prata med Twilio-Support][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="87ce0-220">[Talk to Twilio Support][twilio_support]</span></span>

[special_offer]: http://ahoy.twilio.com/azure
[twilio_python]: https://github.com/twilio/twilio-python
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-python/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-python/blob/master/README.md

[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
