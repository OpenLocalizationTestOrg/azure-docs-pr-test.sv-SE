---
title: "aaaHow tooUse Twilio för röst- och SMS (Python) | Microsoft Docs"
description: "Lär dig hur toomake ett telefonsamtal och skicka ett SMS-meddelandet med hello Twilio API-tjänsten på Azure. Kodexempel som skrivits i Python."
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
ms.openlocfilehash: 1f16a107d95c94589b8d61a0971c5b62d639c571
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-python"></a><span data-ttu-id="a9da7-104">Hur tooUse Twilio för röst- och SMS-funktioner i Python</span><span class="sxs-lookup"><span data-stu-id="a9da7-104">How tooUse Twilio for Voice and SMS Capabilities in Python</span></span>
<span data-ttu-id="a9da7-105">Den här guiden visar hur tooperform vanliga programmeringsuppgifter med hello Twilio API-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="a9da7-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="a9da7-106">hello-scenarier som tas upp inkluderar att ringa och ett kort meddelande (SMS Service)-meddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="a9da7-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="a9da7-107">Mer information om Twilio och använder röst- och SMS i dina program finns hello [nästa steg](#NextSteps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a9da7-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="a9da7-108"><a id="WhatIs"></a>Vad är Twilio?</span><span class="sxs-lookup"><span data-stu-id="a9da7-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="a9da7-109">Twilio startar hello framtiden för kommunikation, aktivera utvecklare tooembed röst, VoIP, och meddelanden i program.</span><span class="sxs-lookup"><span data-stu-id="a9da7-109">Twilio is powering hello future of business communications, enabling developers tooembed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="a9da7-110">De virtualisera alla infrastruktur som behövs i en molnbaserad, globala exponera den via hello Twilio kommunikation API-plattformen.</span><span class="sxs-lookup"><span data-stu-id="a9da7-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through hello Twilio communications API platform.</span></span> <span data-ttu-id="a9da7-111">Programmen är enkla toobuild och skalbara.</span><span class="sxs-lookup"><span data-stu-id="a9da7-111">Applications are simple toobuild and scalable.</span></span> <span data-ttu-id="a9da7-112">Få flexibilitet med lön-som-dig gå priser och dra nytta av molnet tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="a9da7-112">Enjoy flexibility with pay-as-you go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="a9da7-113">**Twilio röst** kan ditt program toomake och ta emot telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="a9da7-113">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span>
<span data-ttu-id="a9da7-114">**Twilio SMS** gör att din ansökan toosend och ta emot textmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="a9da7-114">**Twilio SMS** enables your application toosend and receive text messages.</span></span>
<span data-ttu-id="a9da7-115">**Twilio klienten** tillåter du toomake VoIP-anrop från telefon, surfplatta eller webbläsare och stöder WebRTC.</span><span class="sxs-lookup"><span data-stu-id="a9da7-115">**Twilio Client** allows you toomake VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="a9da7-116"><a id="Pricing"></a>Priser för Twilio och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="a9da7-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="a9da7-117">Azure-kunder får en [specialerbjudande] [ special_offer] $10 Twilio kredit när du uppgraderar ditt Twilio-konto.</span><span class="sxs-lookup"><span data-stu-id="a9da7-117">Azure customers receive a [special offer][special_offer] $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="a9da7-118">Den här Twilio-kredit kan vara tillämpade tooany Twilio användning ($10 kredit motsvarande toosending upp till 1 000 SMS-meddelanden eller ta emot in too1000 inkommande röst minuter beroende på hello platsen för ditt mål för telefon och meddelandet eller samtal).</span><span class="sxs-lookup"><span data-stu-id="a9da7-118">This Twilio Credit can be applied tooany Twilio usage ($10 credit equivalent toosending as many as 1,000 SMS messages or receiving up too1000 inbound Voice minutes, depending on hello location of your phone number and message or call destination).</span></span> <span data-ttu-id="a9da7-119">Lösa detta [Twilio kredit] [ special_offer] och komma igång.</span><span class="sxs-lookup"><span data-stu-id="a9da7-119">Redeem this [Twilio credit][special_offer] and get started.</span></span>

<span data-ttu-id="a9da7-120">Twilio är en betalning per användning.</span><span class="sxs-lookup"><span data-stu-id="a9da7-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="a9da7-121">Det finns inga avgifter för installation och du kan stänga ditt konto när som helst.</span><span class="sxs-lookup"><span data-stu-id="a9da7-121">There are no set-up fees and you can close your account at any time.</span></span> <span data-ttu-id="a9da7-122">Du hittar mer information i [Twilio priser][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="a9da7-122">You can find more details at [Twilio Pricing][twilio_pricing].</span></span>

## <span data-ttu-id="a9da7-123"><a id="Concepts"></a>Begrepp</span><span class="sxs-lookup"><span data-stu-id="a9da7-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="a9da7-124">Hej Twilio API är en RESTful-API som tillhandahåller röst- och SMS-funktioner för program.</span><span class="sxs-lookup"><span data-stu-id="a9da7-124">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="a9da7-125">Klientbibliotek är tillgängliga på flera språk. en lista, se [Twilio-API-bibliotek][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="a9da7-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="a9da7-126">Viktiga aspekter av hello Twilio API är Twilio verb och Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="a9da7-126">Key aspects of hello Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="a9da7-127"><a id="Verbs"></a>Twilio-verb</span><span class="sxs-lookup"><span data-stu-id="a9da7-127"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="a9da7-128">hello API väljer Twilio verb; till exempel hello  **&lt;säg&gt;**  verb instruerar Twilio tooaudibly leverera ett meddelande på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="a9da7-128">hello API makes use of Twilio verbs; for example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span>

<span data-ttu-id="a9da7-129">hello följer en lista över Twilio verb.</span><span class="sxs-lookup"><span data-stu-id="a9da7-129">hello following is a list of Twilio verbs.</span></span> <span data-ttu-id="a9da7-130">Lär dig mer om hello andra verb och funktioner via [Markup Language Twilio dokumentationen][twiml].</span><span class="sxs-lookup"><span data-stu-id="a9da7-130">Learn about hello other verbs and capabilities via [Twilio Markup Language documentation][twiml].</span></span>

* <span data-ttu-id="a9da7-131">**&lt;Ring&gt;**: ansluter hello anroparen tooanother phone.</span><span class="sxs-lookup"><span data-stu-id="a9da7-131">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="a9da7-132">**&lt;Samla in&gt;**: samlar in siffror som anges på hello telefon tangentbordet.</span><span class="sxs-lookup"><span data-stu-id="a9da7-132">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="a9da7-133">**&lt;Koppla ned&gt;**: slutar ett anrop.</span><span class="sxs-lookup"><span data-stu-id="a9da7-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="a9da7-134">**&lt;Pausa&gt;**: tyst väntar på ett angivet antal sekunder.</span><span class="sxs-lookup"><span data-stu-id="a9da7-134">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="a9da7-135">**&lt;Spela upp&gt;**: spelar en ljudfil.</span><span class="sxs-lookup"><span data-stu-id="a9da7-135">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="a9da7-136">**&lt;Kön&gt;**: Lägg till hello tooa kö med anropare.</span><span class="sxs-lookup"><span data-stu-id="a9da7-136">**&lt;Queue&gt;**: Add hello tooa queue of callers.</span></span>
* <span data-ttu-id="a9da7-137">**&lt;Posten&gt;**: registrerar hello röst hello anroparen och returnerar en URL för en fil som innehåller hello registrering.</span><span class="sxs-lookup"><span data-stu-id="a9da7-137">**&lt;Record&gt;**: Records hello voice of hello caller and returns a URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="a9da7-138">**&lt;Omdirigera&gt;**: Överför kontroll över ett samtal eller SMS toohello TwiML på en annan URL.</span><span class="sxs-lookup"><span data-stu-id="a9da7-138">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="a9da7-139">**&lt;Avvisa&gt;**: avvisar en inkommande anropa tooyour Twilio tal utan fakturering du.</span><span class="sxs-lookup"><span data-stu-id="a9da7-139">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you.</span></span>
* <span data-ttu-id="a9da7-140">**&lt;Säg&gt;**: konverterar text toospeech som görs på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="a9da7-140">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="a9da7-141">**&lt;SMS&gt;**: skickar ett SMS-meddelande.</span><span class="sxs-lookup"><span data-stu-id="a9da7-141">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="a9da7-142"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="a9da7-142"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="a9da7-143">TwiML är en uppsättning XML-baserade instruktioner baserat på hello Twilio verb som informerar Twilio på hur tooprocess ett samtal eller SMS.</span><span class="sxs-lookup"><span data-stu-id="a9da7-143">TwiML is a set of XML-based instructions based on hello Twilio verbs that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="a9da7-144">Exempelvis skulle hello följande TwiML omvandla hello text **Hello World** toospeech.</span><span class="sxs-lookup"><span data-stu-id="a9da7-144">As an example, hello following TwiML would convert hello text **Hello World** toospeech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

<span data-ttu-id="a9da7-145">När programmet-anrop hello Twilio-API, är en av parametrarna för hello API hello URL som returnerar hello TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="a9da7-145">When your application calls hello Twilio API, one of hello API parameters is hello URL that returns hello TwiML response.</span></span> <span data-ttu-id="a9da7-146">För utveckling, kan du använda Twilio-tillhandahållna URL: er tooprovide hello TwiML svar som används av dina program.</span><span class="sxs-lookup"><span data-stu-id="a9da7-146">For development purposes, you can use Twilio-provided URLs tooprovide hello TwiML responses used by your applications.</span></span> <span data-ttu-id="a9da7-147">Du kan också vara värd för dina egna URL: er tooproduce hello TwiML svar och ett annat alternativ är toouse hello `TwiMLResponse` objekt.</span><span class="sxs-lookup"><span data-stu-id="a9da7-147">You could also host your own URLs tooproduce hello TwiML responses, and another option is toouse hello `TwiMLResponse` object.</span></span>

<span data-ttu-id="a9da7-148">Läs mer om Twilio verb, deras attribut och TwiML [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="a9da7-148">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="a9da7-149">Mer information om hello Twilio-API finns [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="a9da7-149">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="a9da7-150"><a id="CreateAccount"></a>Skapa ett Twilio-konto</span><span class="sxs-lookup"><span data-stu-id="a9da7-150"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="a9da7-151">När du är klar tooget Twilio-konto kan logga på [försök Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="a9da7-151">When you are ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="a9da7-152">Du kan börja med ett kostnadsfritt konto och uppgradera ditt konto senare.</span><span class="sxs-lookup"><span data-stu-id="a9da7-152">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="a9da7-153">När du registrerar dig för ett Twilio-konto får du ett SID-konto och en token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="a9da7-153">When you sign up for a Twilio account, you receive an account SID and an authentication token.</span></span> <span data-ttu-id="a9da7-154">Båda ska vara nödvändiga toomake Twilio API-anrop.</span><span class="sxs-lookup"><span data-stu-id="a9da7-154">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="a9da7-155">tooprevent obehörig åtkomst till tooyour konto, skydda din token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="a9da7-155">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="a9da7-156">Ditt konto SID och autentiseringstoken kan visas i hello [Twilio konsolen][twilio_console]i hello fält med namnet **konto SID** och **AUTH TOKEN**respektive.</span><span class="sxs-lookup"><span data-stu-id="a9da7-156">Your account SID and authentication token are viewable in hello [Twilio Console][twilio_console], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="a9da7-157"><a id="create_app"></a>Skapa en Python-program</span><span class="sxs-lookup"><span data-stu-id="a9da7-157"><a id="create_app"></a>Create a Python Application</span></span>
<span data-ttu-id="a9da7-158">En Python-program som använder hello Twilio-tjänsten och körs i Azure är inte annorlunda än andra Python-program som använder hello Twilio tjänst.</span><span class="sxs-lookup"><span data-stu-id="a9da7-158">A Python application that uses hello Twilio service and is running in Azure is no different than any other Python application that uses hello Twilio service.</span></span> <span data-ttu-id="a9da7-159">När Twilio är REST-baserad och kan anropas från Python på flera sätt, den här artikeln fokuserar på hur toouse Twilio tjänster med [Twilio-biblioteket för Python från GitHub][twilio_python].</span><span class="sxs-lookup"><span data-stu-id="a9da7-159">While Twilio services are REST-based and can be called from Python in several ways, this article will focus on how toouse Twilio services with [Twilio library for Python from GitHub][twilio_python].</span></span> <span data-ttu-id="a9da7-160">Mer information om hur du använder hello Twilio-biblioteket för Python finns [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].</span><span class="sxs-lookup"><span data-stu-id="a9da7-160">For more information about using hello Twilio library for Python, see [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].</span></span>

<span data-ttu-id="a9da7-161">Första [ställa in en ny Azure Linux VM] [azure_vm_setup] tooact som värd för ditt nya Python-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a9da7-161">First, [set-up a new Azure Linux VM][azure_vm_setup] tooact as a host for your new Python web application.</span></span> <span data-ttu-id="a9da7-162">När hello virtuella datorn körs, måste tooexpose programmet på en offentlig port som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="a9da7-162">Once hello Virtual Machine is running, you will need tooexpose your application on a public port as described below.</span></span>

### <a name="add-an-incoming-rule"></a><span data-ttu-id="a9da7-163">Lägg till en inkommande regel</span><span class="sxs-lookup"><span data-stu-id="a9da7-163">Add An Incoming Rule</span></span>
  1. <span data-ttu-id="a9da7-164">Gå toohello [Nätverkssäkerhetsgruppen] [azure_nsg] sidan.</span><span class="sxs-lookup"><span data-stu-id="a9da7-164">Go toohello [Network Security Group][azure_nsg] page.</span></span>
  2. <span data-ttu-id="a9da7-165">Välj hello Nätverkssäkerhetsgrupp som överensstämmer med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a9da7-165">Select hello Network Security Group that corresponds with your Virtual Machine.</span></span>
  3. <span data-ttu-id="a9da7-166">Lägg till och **utgående regel** för **port 80**.</span><span class="sxs-lookup"><span data-stu-id="a9da7-166">Add and **Outgoing Rule** for **port 80**.</span></span> <span data-ttu-id="a9da7-167">Vara säker på att tooallow inkommande från alla adresser.</span><span class="sxs-lookup"><span data-stu-id="a9da7-167">Be sure tooallow incoming from any address.</span></span>

### <a name="set-hello-dns-name-label"></a><span data-ttu-id="a9da7-168">Ange hello DNS-namnetikett</span><span class="sxs-lookup"><span data-stu-id="a9da7-168">Set hello DNS Name Label</span></span>
  1. <span data-ttu-id="a9da7-169">Gå toohello [hello offentliga IP-postadresser] [azure_ips] sidan.</span><span class="sxs-lookup"><span data-stu-id="a9da7-169">Go toohello [hello Public IP Adresses][azure_ips] page.</span></span>
  2. <span data-ttu-id="a9da7-170">Välj hello offentliga IP-Adressen som correspends med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a9da7-170">Select hello Public IP that correspends with your Virtual Machine.</span></span>
  3. <span data-ttu-id="a9da7-171">Ange hello **DNS-namnetikett** i hello **Configuration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a9da7-171">Set hello **DNS Name Label** in hello **Configuration** section.</span></span> <span data-ttu-id="a9da7-172">Det här exemplet hello gäller den ser ut ungefär så här *din domän etikett*. centralus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="a9da7-172">In hello case of this example it will look something like this *your-domain-label*.centralus.cloudapp.azure.com</span></span>

<span data-ttu-id="a9da7-173">När du har tooconnect via SSH toohello virtuell dator som du kan installera hello webbramverk önskat (hello två mest kända i Python som [Flask](http://flask.pocoo.org/) och [Django](https://www.djangoproject.com)).</span><span class="sxs-lookup"><span data-stu-id="a9da7-173">Once you are able tooconnect through SSH toohello Virtual Machine you can install hello Web Framework of your choice (hello two most well known in Python being [Flask](http://flask.pocoo.org/) and [Django](https://www.djangoproject.com)).</span></span> <span data-ttu-id="a9da7-174">Du kan installera någon av dem genom att köra hello `pip install` kommando.</span><span class="sxs-lookup"><span data-stu-id="a9da7-174">You can install either of them just by running hello `pip install` command.</span></span>

<span data-ttu-id="a9da7-175">Tänk på att vi har konfigurerat hello virtuella tooallow trafik endast på port 80.</span><span class="sxs-lookup"><span data-stu-id="a9da7-175">Keep in mind that we configured hello Virtual Machine tooallow traffic only on port 80.</span></span> <span data-ttu-id="a9da7-176">Se till att tooconfigure hello programmet toouse denna port.</span><span class="sxs-lookup"><span data-stu-id="a9da7-176">So make sure tooconfigure hello application toouse this port.</span></span>

## <span data-ttu-id="a9da7-177"><a id="configure_app"></a>Konfigurera ditt program tooUse Twilio-bibliotek</span><span class="sxs-lookup"><span data-stu-id="a9da7-177"><a id="configure_app"></a>Configure Your Application tooUse Twilio Libraries</span></span>
<span data-ttu-id="a9da7-178">Du kan konfigurera biblioteket programmet toouse hello Twilio för Python på två sätt:</span><span class="sxs-lookup"><span data-stu-id="a9da7-178">You can configure your application toouse hello Twilio library for Python in two ways:</span></span>

* <span data-ttu-id="a9da7-179">Installera hello Twilio-biblioteket för Python som Pip-paket.</span><span class="sxs-lookup"><span data-stu-id="a9da7-179">Install hello Twilio library for Python as a Pip package.</span></span> <span data-ttu-id="a9da7-180">Den kan installeras med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="a9da7-180">It can be installed with hello following commands:</span></span>
   
        $ pip install twilio

    <span data-ttu-id="a9da7-181">ELLER</span><span class="sxs-lookup"><span data-stu-id="a9da7-181">-OR-</span></span>

* <span data-ttu-id="a9da7-182">Hämta hello Twilio-biblioteket för Python från GitHub ([https://github.com/twilio/twilio-python][twilio_python]) och installera den så här:</span><span class="sxs-lookup"><span data-stu-id="a9da7-182">Download hello Twilio library for Python from GitHub ([https://github.com/twilio/twilio-python][twilio_python]) and install it like this:</span></span>

        $ python setup.py install

<span data-ttu-id="a9da7-183">När du har installerat hello Twilio-biblioteket för Python, kan du sedan `import` i Python-filer:</span><span class="sxs-lookup"><span data-stu-id="a9da7-183">Once you have installed hello Twilio library for Python, you can then `import` it in your Python files:</span></span>

        import twilio

<span data-ttu-id="a9da7-184">Mer information finns i [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].</span><span class="sxs-lookup"><span data-stu-id="a9da7-184">For more information, see [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].</span></span>

## <span data-ttu-id="a9da7-185"><a id="howto_make_call"></a>Så här: göra en utgående anrop</span><span class="sxs-lookup"><span data-stu-id="a9da7-185"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="a9da7-186">hello följande visar hur en utgående toomake anropa.</span><span class="sxs-lookup"><span data-stu-id="a9da7-186">hello following shows how toomake an outgoing call.</span></span> <span data-ttu-id="a9da7-187">Den här koden används även en Twilio-tillhandahållna platsen tooreturn hello Twilio Markup Language (TwiML) svar.</span><span class="sxs-lookup"><span data-stu-id="a9da7-187">This code also uses a Twilio-provided site tooreturn hello Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="a9da7-188">Ersätt värdena för hello **from_number** och **to_number** telefonnummer och se till att du har kontrollerat hello **from_number** telefonnummer för ditt Twilio-konto innan du kör hello-kod.</span><span class="sxs-lookup"><span data-stu-id="a9da7-188">Substitute your values for hello **from_number** and **to_number** phone numbers, and ensure that you've verified hello **from_number** phone number for your Twilio account before running hello code.</span></span>

    from urllib.parse import urlencode

    # Import hello Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    # hello number of hello phone initiating hello hello call.
    # This should either be a Twilio number or a number that you've verified
    from_number = "NNNNNNNNNNN"

    # hello number of hello phone receiving call.
    to_number = "NNNNNNNNNNN"

    # Use hello Twilio-provided site for hello TwiML response.
    url = "http://twimlets.com/message?"

    # hello phone message text.
    message = "Hello world."

    # Initialize hello Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make hello call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url + urlencode({'Message': message}))
    print(call.sid)

<span data-ttu-id="a9da7-189">Som tidigare nämnts kan använder den här koden en Twilio-tillhandahållna platsen tooreturn hello TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="a9da7-189">As mentioned, this code uses a Twilio-provided site tooreturn hello TwiML response.</span></span> <span data-ttu-id="a9da7-190">Du kan i stället använda din egen webbplats tooprovide hello TwiML svar; Mer information finns i [hur tooProvide TwiML svar från din egen webbplats](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="a9da7-190">You could instead use your own site tooprovide hello TwiML response; for more information, see [How tooProvide TwiML Responses from Your Own Web Site](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="a9da7-191"><a id="howto_send_sms"></a>Så här: skicka ett SMS-meddelande</span><span class="sxs-lookup"><span data-stu-id="a9da7-191"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="a9da7-192">hello nedan visar hur en SMS-meddelande med toosend hello `TwilioRestClient` klass.</span><span class="sxs-lookup"><span data-stu-id="a9da7-192">hello following shows how toosend an SMS message using hello `TwilioRestClient` class.</span></span> <span data-ttu-id="a9da7-193">Hej **from_number** nummer tillhandahålls av Twilio för utvärderingsversionen konton toosend SMS-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a9da7-193">hello **from_number** number is provided by Twilio for trial accounts toosend SMS messages.</span></span> <span data-ttu-id="a9da7-194">Hej **to_number** numret måste verifieras för ditt Twilio-konto innan du kör hello kod.</span><span class="sxs-lookup"><span data-stu-id="a9da7-194">hello **to_number** number must be verified for your Twilio account before running hello code.</span></span>

    # Import hello Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    from_number = "NNNNNNNNNNN"  # With trial account, texts can only be sent from your Twilio number.
    to_number = "NNNNNNNNNNN"
    message = "Hello world."

    # Initialize hello Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Send hello SMS message.
    message = client.messages.create(to=to_number,
                                     from_=from_number,
                                     body=message)

## <span data-ttu-id="a9da7-195"><a id="howto_provide_twiml_responses"></a>Så här: Ange TwiML svar från din egen webbplats</span><span class="sxs-lookup"><span data-stu-id="a9da7-195"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="a9da7-196">När ditt program initierar en anropet toohello Twilio-API, skickar Twilio din begäran tooa URL-adress förväntades tooreturn TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="a9da7-196">When your application initiates a call toohello Twilio API, Twilio will send your request tooa URL that is expected tooreturn a TwiML response.</span></span> <span data-ttu-id="a9da7-197">hello-exemplet ovan använder hello Twilio-tillhandahållna URL [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="a9da7-197">hello example above uses hello Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="a9da7-198">(När TwiML är avsedd för användning av Twilio, kan du visa den i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="a9da7-198">(While TwiML is designed for use by Twilio, you can view it in your browser.</span></span> <span data-ttu-id="a9da7-199">Klicka till exempel [http://twimlets.com/message] [ twimlet_message_url] toosee en tom `<Response>` element, klicka på ett annat exempel är [http://twimlets.com/message? Meddelande % 5B0 %5, D = Hello % 20World] [ twimlet_message_url_hello_world] toosee en `<Response>` element som innehåller en `<Say>` element.)</span><span class="sxs-lookup"><span data-stu-id="a9da7-199">For example, click [http://twimlets.com/message][twimlet_message_url] toosee an empty `<Response>` element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] toosee a `<Response>` element that contains a `<Say>` element.)</span></span>

<span data-ttu-id="a9da7-200">I stället för på hello angivna Twilio-URL, kan du skapa din egen webbplats som returnerar HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="a9da7-200">Instead of relying on hello Twilio-provided URL, you can create your own site that returns HTTP responses.</span></span> <span data-ttu-id="a9da7-201">Du kan skapa hello webbplats på alla språk som returnerar XML-svar; Det här avsnittet förutsätter att du kommer att använda Python toocreate hello TwiML.</span><span class="sxs-lookup"><span data-stu-id="a9da7-201">You can create hello site in any language that returns XML responses; this topic assumes you will be using Python toocreate hello TwiML.</span></span>

<span data-ttu-id="a9da7-202">hello följande exempel kommer utdata TwiML svar som säger **Hello World** i hello-anropet.</span><span class="sxs-lookup"><span data-stu-id="a9da7-202">hello following examples will output a TwiML response that says **Hello World** on hello call.</span></span>

<span data-ttu-id="a9da7-203">Med Flask:</span><span class="sxs-lookup"><span data-stu-id="a9da7-203">With Flask:</span></span>

    from flask import Response
    @app.route("/")
    def hello():
        xml = '<Response><Say>Hello world.</Say></Response>'
        return Response(xml, mimetype='text/xml')

<span data-ttu-id="a9da7-204">Med Django:</span><span class="sxs-lookup"><span data-stu-id="a9da7-204">With Django:</span></span>

    from django.http import HttpResponse
    def hello(request):
        xml = '<Response><Say>Hello world.</Say></Response>'
        return HttpResponse(xml, content_type='text/xml')

<span data-ttu-id="a9da7-205">Du kan se hello-exemplet ovan kan är hello TwiML svar bara ett XML-dokument.</span><span class="sxs-lookup"><span data-stu-id="a9da7-205">As you can see from hello example above, hello TwiML response is simply an XML document.</span></span> <span data-ttu-id="a9da7-206">Hej Twilio-biblioteket för Python innehåller klasser som ska generera TwiML för dig.</span><span class="sxs-lookup"><span data-stu-id="a9da7-206">hello Twilio library for Python contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="a9da7-207">hello exemplet nedan ger hello motsvarande svar som ovan, men använder hello `twiml` modul i hello Twilio-biblioteket för Python:</span><span class="sxs-lookup"><span data-stu-id="a9da7-207">hello example below produces hello equivalent response as shown above, but uses hello `twiml` module in hello Twilio library for Python:</span></span>

    from twilio import twiml

    response = twiml.Response()
    response.say("Hello world.")
    print(str(response))

<span data-ttu-id="a9da7-208">Läs mer om TwiML [https://www.twilio.com/docs/api/twiml][twiml_reference].</span><span class="sxs-lookup"><span data-stu-id="a9da7-208">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml][twiml_reference].</span></span>

<span data-ttu-id="a9da7-209">När du har Python programmet Konfigurera tooprovide TwiML svar använder hello URL för hello program som hello URL som skickades till hello `client.calls.create` metod.</span><span class="sxs-lookup"><span data-stu-id="a9da7-209">Once you have your Python application set up tooprovide TwiML responses, use hello URL of hello application as hello URL passed into hello `client.calls.create`  method.</span></span> <span data-ttu-id="a9da7-210">Till exempel om du har ett webbprogram med namnet **MyTwiML** distribuerade tooan Azure värdbaserade tjänsten kan du använda webbadressen som webhook som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a9da7-210">For example, if you have a Web application named **MyTwiML** deployed tooan Azure hosted service, you can use its url as webhook as shown in hello following example:</span></span>

    from twilio.rest import TwilioRestClient

    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"
    from_number = "NNNNNNNNNNN"
    to_number = "NNNNNNNNNNN"
    url = "http://your-domain-label.centralus.cloudapp.azure.com/MyTwiML/"

    # Initialize hello Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make hello call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url)
    print(call.sid)

## <span data-ttu-id="a9da7-211"><a id="AdditionalServices"></a>Så här: använda ytterligare Twilio-tjänster</span><span class="sxs-lookup"><span data-stu-id="a9da7-211"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="a9da7-212">Dessutom kan toohello exemplen nedan, Twilio erbjuder webbaserade API: er som du använda tooleverage ytterligare Twilio-funktioner från din Azure-program.</span><span class="sxs-lookup"><span data-stu-id="a9da7-212">In addition toohello examples shown here, Twilio offers web-based APIs that you can use tooleverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="a9da7-213">Fullständig information finns i hello [Twilio-API-dokumentationen][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="a9da7-213">For full details, see hello [Twilio API documentation][twilio_api].</span></span>

## <span data-ttu-id="a9da7-214"><a id="NextSteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9da7-214"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="a9da7-215">Nu när du har lärt dig hello grunderna i hello Twilio-tjänsten, följa dessa länkar toolearn mer:</span><span class="sxs-lookup"><span data-stu-id="a9da7-215">Now that you have learned hello basics of hello Twilio service, follow these links toolearn more:</span></span>

* <span data-ttu-id="a9da7-216">[Riktlinjer för Twilio-säkerhet][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="a9da7-216">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="a9da7-217">[Twilio ta guider och exempelkod][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="a9da7-217">[Twilio HowTo Guides and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="a9da7-218">[Twilio snabbstarten självstudier][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="a9da7-218">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span>
* <span data-ttu-id="a9da7-219">[Twilio på GitHub][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="a9da7-219">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="a9da7-220">[Kontakta tooTwilio Support][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="a9da7-220">[Talk tooTwilio Support][twilio_support]</span></span>

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
