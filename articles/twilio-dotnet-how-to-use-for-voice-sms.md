---
title: "aaaHow tooUse Twilio för röst- och SMS (.NET) | Microsoft Docs"
description: "Lär dig hur toomake ett telefonsamtal och skicka ett SMS-meddelandet med hello Twilio API-tjänsten på Azure. Kodexempel som skrivits i .NET."
services: 
documentationcenter: .net
author: devinrader
manager: twilio
editor: 
ms.assetid: 74d4f3c9-f1cb-4968-b744-36b32cd0e834
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/24/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: f568da87ef15e9f540fee9674de31e983d4acb6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-from-azure"></a><span data-ttu-id="0eaa4-104">Hur toouse Twilio för röst- och SMS-funktioner från Azure</span><span class="sxs-lookup"><span data-stu-id="0eaa4-104">How toouse Twilio for voice and SMS capabilities from Azure</span></span>
<span data-ttu-id="0eaa4-105">Den här guiden visar hur tooperform vanliga programmeringsuppgifter med hello Twilio API-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="0eaa4-106">hello-scenarier som tas upp inkluderar att ringa och ett kort meddelande (SMS Service)-meddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="0eaa4-107">Mer information om Twilio och använder röst- och SMS i dina program finns hello [nästa steg](#NextSteps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next steps](#NextSteps) section.</span></span>

## <span data-ttu-id="0eaa4-108"><a id="WhatIs"></a>Vad är Twilio?</span><span class="sxs-lookup"><span data-stu-id="0eaa4-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="0eaa4-109">Twilio startar hello framtiden för kommunikation, aktivera utvecklare tooembed röst, VoIP, och meddelanden i program.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-109">Twilio is powering hello future of business communications, enabling developers tooembed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="0eaa4-110">De virtualisera alla infrastruktur som behövs i en molnbaserad, globala exponera den via hello Twilio kommunikation API-plattformen.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through hello Twilio communications API platform.</span></span> <span data-ttu-id="0eaa4-111">Programmen är enkla toobuild och skalbara.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-111">Applications are simple toobuild and scalable.</span></span> <span data-ttu-id="0eaa4-112">Utnyttja flexibilitet med betala per användning priser och dra nytta av molnet tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-112">Enjoy flexibility with pay-as-you-go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="0eaa4-113">**Twilio röst** kan ditt program toomake och ta emot telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-113">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span> <span data-ttu-id="0eaa4-114">**Twilio SMS** gör att dina program toosend och ta emot SMS-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-114">**Twilio SMS** enables your applications toosend and receive SMS messages.</span></span> <span data-ttu-id="0eaa4-115">**Twilio klienten** tillåter du toomake VoIP-anrop från telefon, surfplatta eller webbläsare och stöder WebRTC.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-115">**Twilio Client** allows you toomake VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="0eaa4-116"><a id="Pricing"></a>Priser för Twilio och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="0eaa4-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="0eaa4-117">Azure-kunder får en [specialerbjudande](http://www.twilio.com/azure): kostnadsfri $10 Twilio kredit när du uppgraderar ditt Twilio-konto.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-117">Azure customers receive a [special offer](http://www.twilio.com/azure): complimentary $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="0eaa4-118">Den här Twilio-kredit kan vara tillämpade tooany Twilio användning ($10 kredit motsvarande toosending upp till 1 000 SMS-meddelanden eller ta emot in too1000 inkommande röst minuter beroende på hello platsen för ditt mål för telefon och meddelandet eller samtal).</span><span class="sxs-lookup"><span data-stu-id="0eaa4-118">This Twilio Credit can be applied tooany Twilio usage ($10 credit equivalent toosending as many as 1,000 SMS messages or receiving up too1000 inbound Voice minutes, depending on hello location of your phone number and message or call destination).</span></span> <span data-ttu-id="0eaa4-119">Lösa in den här Twilio-kredit och kom igång med [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span><span class="sxs-lookup"><span data-stu-id="0eaa4-119">Redeem this Twilio credit and get started at [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span></span>

<span data-ttu-id="0eaa4-120">Twilio är en betalning per användning.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="0eaa4-121">Det finns inga avgifter för installation och du kan stänga ditt konto när som helst.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-121">There are no set-up fees, and you can close your account at any time.</span></span> <span data-ttu-id="0eaa4-122">Du hittar mer information i [Twilio priser](http://www.twilio.com/voice/pricing).</span><span class="sxs-lookup"><span data-stu-id="0eaa4-122">You can find more details at [Twilio Pricing](http://www.twilio.com/voice/pricing).</span></span>

## <span data-ttu-id="0eaa4-123"><a id="Concepts"></a>Begrepp</span><span class="sxs-lookup"><span data-stu-id="0eaa4-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="0eaa4-124">Hej Twilio API är en RESTful-API som tillhandahåller röst- och SMS-funktioner för program.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-124">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="0eaa4-125">Klientbibliotek är tillgängliga på flera språk. en lista, se [Twilio-API-bibliotek][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="0eaa4-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="0eaa4-126">Viktiga aspekter av hello Twilio API är Twilio verb och Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="0eaa4-126">Key aspects of hello Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="0eaa4-127"><a id="Verbs"></a>Twilio-verb</span><span class="sxs-lookup"><span data-stu-id="0eaa4-127"><a id="Verbs"></a>Twilio verbs</span></span>
<span data-ttu-id="0eaa4-128">hello API väljer Twilio verb; till exempel hello  **&lt;säg&gt;**  verb instruerar Twilio tooaudibly leverera ett meddelande på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-128">hello API makes use of Twilio verbs; for example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span>

<span data-ttu-id="0eaa4-129">hello följer en lista över Twilio verb.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-129">hello following is a list of Twilio verbs.</span></span>  <span data-ttu-id="0eaa4-130">Lär dig mer om hello andra verb och funktioner via [Markup Language Twilio dokumentationen](http://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="0eaa4-130">Learn about hello other verbs and capabilities via [Twilio Markup Language documentation](http://www.twilio.com/docs/api/twiml).</span></span>

* <span data-ttu-id="0eaa4-131">**&lt;Ring&gt;**: ansluter hello anroparen tooanother phone.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-131">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="0eaa4-132">**&lt;Samla in&gt;**: samlar in siffror som anges på hello telefon tangentbordet.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-132">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="0eaa4-133">**&lt;Koppla ned&gt;**: slutar ett anrop.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="0eaa4-134">**&lt;Spela upp&gt;**: spelar en ljudfil.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-134">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="0eaa4-135">**&lt;Pausa&gt;**: tyst väntar på ett angivet antal sekunder.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-135">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="0eaa4-136">**&lt;Posten&gt;**: registrerar hello anroparen röst och returnerar ett URL-Adressen till en fil som innehåller hello registrering.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-136">**&lt;Record&gt;**: Records hello caller's voice and returns an URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="0eaa4-137">**&lt;Omdirigera&gt;**: Överför kontroll över ett samtal eller SMS toohello TwiML på en annan URL.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-137">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="0eaa4-138">**&lt;Avvisa&gt;**: avvisar en inkommande anropa tooyour Twilio tal utan fakturering du</span><span class="sxs-lookup"><span data-stu-id="0eaa4-138">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you</span></span>
* <span data-ttu-id="0eaa4-139">**&lt;Säg&gt;**: konverterar text toospeech som görs på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-139">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="0eaa4-140">**&lt;SMS&gt;**: skickar ett SMS-meddelande.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-140">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="0eaa4-141"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="0eaa4-141"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="0eaa4-142">TwiML är en uppsättning XML-baserade instruktioner baserat på hello Twilio verb som informerar Twilio på hur tooprocess ett samtal eller SMS.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-142">TwiML is a set of XML-based instructions based on hello Twilio verbs that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="0eaa4-143">Exempelvis skulle hello följande TwiML omvandla hello text **Hello World** toospeech.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-143">As an example, hello following TwiML would convert hello text **Hello World** toospeech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

<span data-ttu-id="0eaa4-144">När programmet-anrop hello Twilio-API, är en av parametrarna för hello API hello URL som returnerar hello TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-144">When your application calls hello Twilio API, one of hello API parameters is hello URL that returns hello TwiML response.</span></span> <span data-ttu-id="0eaa4-145">För utveckling, kan du använda Twilio-tillhandahållna URL: er tooprovide hello TwiML svar som används av dina program.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-145">For development purposes, you can use Twilio-provided URLs tooprovide hello TwiML responses used by your applications.</span></span> <span data-ttu-id="0eaa4-146">Du kan också vara värd för dina egna URL: er tooproduce hello TwiML svar och ett annat alternativ är toouse hello **TwiMLResponse** objekt.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-146">You could also host your own URLs tooproduce hello TwiML responses, and another option is toouse hello **TwiMLResponse** object.</span></span>

<span data-ttu-id="0eaa4-147">Läs mer om Twilio verb, deras attribut och TwiML [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="0eaa4-147">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="0eaa4-148">Mer information om hello Twilio-API finns [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="0eaa4-148">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="0eaa4-149"><a id="CreateAccount"></a>Skapa ett Twilio-konto</span><span class="sxs-lookup"><span data-stu-id="0eaa4-149"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="0eaa4-150">När du är klar tooget Twilio-konto kan logga på [försök Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="0eaa4-150">When you're ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="0eaa4-151">Du kan börja med ett kostnadsfritt konto och uppgradera ditt konto senare.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-151">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="0eaa4-152">När du registrerar dig för ett Twilio-konto får du ett konto-ID och ett token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-152">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="0eaa4-153">Båda ska vara nödvändiga toomake Twilio API-anrop.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-153">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="0eaa4-154">tooprevent obehörig åtkomst till tooyour konto, skydda din token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-154">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="0eaa4-155">Ditt konto-ID och autentisering token kan visas på hello [Twilio kontosida][twilio_account]i hello fält med namnet **konto SID** och **AUTH TOKEN**respektive.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-155">Your account ID and authentication token are viewable at hello [Twilio account page][twilio_account], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="0eaa4-156"><a id="create_app"></a>Skapa ett Azure-program</span><span class="sxs-lookup"><span data-stu-id="0eaa4-156"><a id="create_app"></a>Create an Azure Application</span></span>
<span data-ttu-id="0eaa4-157">Ett Azure-program som är värd för ett Twilio-aktiverat program skiljer sig inte från andra Azure-program.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-157">An Azure application that hosts a Twilio enabled application is no different from any other Azure application.</span></span> <span data-ttu-id="0eaa4-158">Du lägger till hello Twilio .NET-bibliotek och konfigurera hello rollen toouse hello Twilio .NET-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-158">You add hello Twilio .NET library and configure hello role toouse hello Twilio .NET libraries.</span></span>
<span data-ttu-id="0eaa4-159">Information om hur du skapar ett första Azure-projekt finns [skapa ett Azure-projekt i Visual Studio][vs_project].</span><span class="sxs-lookup"><span data-stu-id="0eaa4-159">For information on creating an initial Azure project, see [Creating an Azure project with Visual Studio][vs_project].</span></span>

## <span data-ttu-id="0eaa4-160"><a id="configure_app"></a>Konfigurera ditt program toouse Twilio-bibliotek</span><span class="sxs-lookup"><span data-stu-id="0eaa4-160"><a id="configure_app"></a>Configure Your Application toouse Twilio Libraries</span></span>
<span data-ttu-id="0eaa4-161">Twilio tillhandahåller en uppsättning .NET helper bibliotek som omsluter olika aspekter av Twilio tooprovide enkelt och smidigt sätt toointeract med hello Twilio REST-API och Twilio klienten toogenerate TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-161">Twilio provides a set of .NET helper libraries that wrap various aspects of Twilio tooprovide simple and easy ways toointeract with hello Twilio REST API and Twilio Client toogenerate TwiML responses.</span></span>

<span data-ttu-id="0eaa4-162">Twilio innehåller fem bibliotek för .NET-utvecklare:</span><span class="sxs-lookup"><span data-stu-id="0eaa4-162">Twilio provides five libraries for .NET developers:</span></span>
<span data-ttu-id="0eaa4-163">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="0eaa4-163">Library</span></span>|<span data-ttu-id="0eaa4-164">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0eaa4-164">Description</span></span>
---|---
<span data-ttu-id="0eaa4-165">Twilio.API</span><span class="sxs-lookup"><span data-stu-id="0eaa4-165">Twilio.API</span></span>|<span data-ttu-id="0eaa4-166">Hej Twilio Kärnbibliotek som omsluter hello Twilio REST API i ett eget .NET-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-166">hello core Twilio library that wraps hello Twilio REST API in a friendly .NET library.</span></span> <span data-ttu-id="0eaa4-167">Det här biblioteket är tillgängliga för .NET, Silverlight och Windows Phone 7.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-167">This library is available for .NET, Silverlight, and Windows Phone 7.</span></span>
<span data-ttu-id="0eaa4-168">Twilio.TwiML</span><span class="sxs-lookup"><span data-stu-id="0eaa4-168">Twilio.TwiML</span></span>|<span data-ttu-id="0eaa4-169">Ger en .NET eget sätt toogenerate TwiML markering.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-169">Provides a .NET friendly way toogenerate TwiML markup.</span></span>
<span data-ttu-id="0eaa4-170">Twilio.MVC</span><span class="sxs-lookup"><span data-stu-id="0eaa4-170">Twilio.MVC</span></span>|<span data-ttu-id="0eaa4-171">För utvecklare som använder ASP.NET MVC, innehåller det här biblioteket en TwilioController och TwiML ActionResult begäran verifieringsattribut.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-171">For developers using ASP.NET MVC, this library includes a TwilioController, TwiML ActionResult, and request validation attribute.</span></span>
<span data-ttu-id="0eaa4-172">Twilio.WebMatrix</span><span class="sxs-lookup"><span data-stu-id="0eaa4-172">Twilio.WebMatrix</span></span>|<span data-ttu-id="0eaa4-173">För utvecklare som använder Microsofts ledigt WebMatrix utvecklingsverktyg för innehåller det här biblioteket Razor syntax hjälpprogram för olika Twilio-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-173">For developers using Microsoft's free WebMatrix development tool, this library contains Razor syntax helpers for various Twilio actions.</span></span>
<span data-ttu-id="0eaa4-174">Twilio.Client.Capability</span><span class="sxs-lookup"><span data-stu-id="0eaa4-174">Twilio.Client.Capability</span></span>|<span data-ttu-id="0eaa4-175">Innehåller hello kapaciteten token generatorn för användning med hello Twilio klienten JavaScript SDK.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-175">Contains hello Capability token generator for use with hello Twilio Client JavaScript SDK.</span></span>

<span data-ttu-id="0eaa4-176">Observera att alla bibliotek kräver .NET 3.5, Silverlight 4 eller Windows Phone 7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-176">Note that all libraries require .NET 3.5, Silverlight 4, or Windows Phone 7 or later.</span></span>

<span data-ttu-id="0eaa4-177">hello exemplen i den här guiden använder hello Twilio.API library.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-177">hello samples provided in this guide use hello Twilio.API library.</span></span>

<span data-ttu-id="0eaa4-178">hello-biblioteken kan vara [installeras med hello NuGet package manager tillägget](http://www.twilio.com/docs/csharp/install) tillgängliga för Visual Studio 2010 in too2015.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-178">hello libraries can be [installed using hello NuGet package manager extension](http://www.twilio.com/docs/csharp/install) available for Visual Studio 2010 up too2015.</span></span>  <span data-ttu-id="0eaa4-179">hello källkoden finns på [GitHub][twilio_github_repo], som innehåller en Wiki som innehåller komplett dokumentation för att använda hello-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-179">hello source code is hosted on [GitHub][twilio_github_repo], which includes a Wiki that contains complete documentation for using hello libraries.</span></span>

<span data-ttu-id="0eaa4-180">Som standard installerar Microsoft Visual Studio 2010 version 1.2 av NuGet.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-180">By default, Microsoft Visual Studio 2010 installs version 1.2 of NuGet.</span></span> <span data-ttu-id="0eaa4-181">Installera hello Twilio bibliotek kräver version 1.6 av NuGet eller högre.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-181">Installing hello Twilio libraries requires version 1.6 of NuGet or higher.</span></span> <span data-ttu-id="0eaa4-182">Mer information om installation eller uppdatering av NuGet finns [http://nuget.org/][nuget].</span><span class="sxs-lookup"><span data-stu-id="0eaa4-182">For information on installing or updating NuGet, see [http://nuget.org/][nuget].</span></span>

> [!NOTE]
> <span data-ttu-id="0eaa4-183">tooinstall hello senaste versionen av NuGet, måste du först avinstallera hello inlästa version med hello Tilläggshanteraren för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-183">tooinstall hello latest version of NuGet, you must first uninstall hello loaded version using hello Visual Studio Extension Manager.</span></span> <span data-ttu-id="0eaa4-184">toodo så måste du köra Visual Studio som administratör.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-184">toodo so, you must run Visual Studio as administrator.</span></span> <span data-ttu-id="0eaa4-185">Annars är hello avinstallera inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-185">Otherwise, hello Uninstall button is disabled.</span></span>
>
>

### <span data-ttu-id="0eaa4-186"><a id="use_nuget"></a>tooadd hello Twilio bibliotek tooyour Visual Studio-projekt:</span><span class="sxs-lookup"><span data-stu-id="0eaa4-186"><a id="use_nuget"></a>tooadd hello Twilio libraries tooyour Visual Studio project:</span></span>
1. <span data-ttu-id="0eaa4-187">Öppna din lösning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-187">Open your solution in Visual Studio.</span></span>
2. <span data-ttu-id="0eaa4-188">Högerklicka på **referenser**.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-188">Right-click **References**.</span></span>
3. <span data-ttu-id="0eaa4-189">Klicka på **hantera NuGet-paket...**</span><span class="sxs-lookup"><span data-stu-id="0eaa4-189">Click **Manage NuGet Packages...**</span></span>
4. <span data-ttu-id="0eaa4-190">Klicka på **Online**.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-190">Click **Online**.</span></span>
5. <span data-ttu-id="0eaa4-191">Skriv i hello online sökrutan *twilio*.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-191">In hello search online box, type *twilio*.</span></span>
6. <span data-ttu-id="0eaa4-192">Klicka på **installera** på hello Twilio-paketet.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-192">Click **Install** on hello Twilio package.</span></span>

## <span data-ttu-id="0eaa4-193"><a id="howto_make_call"></a>Så här: göra en utgående anrop</span><span class="sxs-lookup"><span data-stu-id="0eaa4-193"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="0eaa4-194">hello nedan visar hur en utgående toomake Ring upp med hello **CallResource** klass.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-194">hello following shows how toomake an outgoing call using hello **CallResource** class.</span></span> <span data-ttu-id="0eaa4-195">Den här koden används även en Twilio-tillhandahållna platsen tooreturn hello Twilio Markup Language (TwiML) svar.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-195">This code also uses a Twilio-provided site tooreturn hello Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="0eaa4-196">Ersätt värdena för hello **till** och **från** telefonnummer och se till att du kontrollerar hello **från** telefonnummer för ditt Twilio-konto innan du kör hello kod.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-196">Substitute your values for hello **to** and **from** phone numbers, and ensure that you verify hello **from** phone number for your Twilio account before running hello code.</span></span>

    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize hello TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    // Use hello Twilio-provided site for hello TwiML response.
    var url = "http://twimlets.com/message";
    url = $"{url}?Message%5B0%5D=Hello%20World";

    // Set hello call From, To, and URL values toouse for hello call.
    // This sample uses hello sandbox number provided by
    // Twilio toomake hello call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri(url));
        }

<span data-ttu-id="0eaa4-197">Mer information om hello parametrar i toohello **CallResource.Create** -metoden finns [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span><span class="sxs-lookup"><span data-stu-id="0eaa4-197">For more information about hello parameters passed in toohello **CallResource.Create** method, see [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span></span>

<span data-ttu-id="0eaa4-198">Som tidigare nämnts kan använder den här koden en Twilio-tillhandahållna platsen tooreturn hello TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-198">As mentioned, this code uses a Twilio-provided site tooreturn hello TwiML response.</span></span> <span data-ttu-id="0eaa4-199">Du kan i stället använda din egen webbplats tooprovide hello TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-199">You could instead use your own site tooprovide hello TwiML response.</span></span> <span data-ttu-id="0eaa4-200">Mer information finns i [så här: Ange TwiML svar från din egen webbplats](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="0eaa4-200">For more information, see [How to: Provide TwiML responses from your own website](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="0eaa4-201"><a id="howto_send_sms"></a>Så här: skicka ett SMS-meddelande</span><span class="sxs-lookup"><span data-stu-id="0eaa4-201"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="0eaa4-202">hello följande skärmbild visar hur en SMS-meddelande med toosend hello **MessageResource** klass.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-202">hello following screenshot shows how toosend an SMS message using hello **MessageResource**  class.</span></span> <span data-ttu-id="0eaa4-203">Hej **från** nummer tillhandahålls av Twilio för utvärderingsversionen konton toosend SMS-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-203">hello **from** number is provided by Twilio for trial accounts toosend SMS messages.</span></span> <span data-ttu-id="0eaa4-204">Hej **till** numret måste verifieras för ditt Twilio-konto innan du kör hello kod.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-204">hello **to** number must be verified for your Twilio account before you run hello code.</span></span>

    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize hello TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    try
    {
        // Send an SMS message.
        var message = MessageResource.Create(
            to: new PhoneNumber("+12069419717"),
            from: new PhoneNumber("+14155992671"),
            body: "This is my SMS message.");
    }
    catch (TwilioException ex)
    {
        // An exception occurred making hello REST call
        Console.WriteLine(ex.Message);
    }

## <span data-ttu-id="0eaa4-205"><a id="howto_provide_twiml_responses"></a>Så här: Ange TwiML svar från din egen webbplats</span><span class="sxs-lookup"><span data-stu-id="0eaa4-205"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own website</span></span>
<span data-ttu-id="0eaa4-206">När ditt program initierar anrop-toohello Twilio API – exempelvis via hello **CallResource.Create** metod - Twilio skickar din begäran tooan URL som är förväntade tooreturn TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-206">When your application initiates a call toohello Twilio API - for example, via hello **CallResource.Create** method - Twilio sends your request tooan URL that is expected tooreturn a TwiML response.</span></span> <span data-ttu-id="0eaa4-207">hello exemplet i [så här: utgående ringa](#howto_make_call) använder hello Twilio-tillhandahållna URL [http://twimlets.com/message] [ twimlet_message_url] tooreturn hello svar.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-207">hello example in [How to: Make an outgoing call](#howto_make_call) uses hello Twilio-provided URL [http://twimlets.com/message][twimlet_message_url] tooreturn hello response.</span></span>

> [!NOTE]
> <span data-ttu-id="0eaa4-208">Medan TwiML är avsedd för användning av webbtjänster, kan du visa hello TwiML i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-208">While TwiML is designed for use by web services, you can view hello TwiML in your browser.</span></span> <span data-ttu-id="0eaa4-209">Klicka till exempel [http://twimlets.com/message] [ twimlet_message_url] toosee en tom &lt;svar&gt; element, klicka på ett annat exempel är [http://twimlets.com/message ? Meddelande % 5B0 %5 D = Hello % 20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) toosee en &lt;svar&gt; element som innehåller en &lt;säg&gt; element.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-209">For example, click [http://twimlets.com/message][twimlet_message_url] toosee an empty &lt;Response&gt; element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) toosee a &lt;Response&gt; element that contains a &lt;Say&gt; element.</span></span>
>
>

<span data-ttu-id="0eaa4-210">I stället för på hello angivna Twilio-URL, kan du skapa din egen URL: en webbplats som returnerar HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-210">Instead of relying on hello Twilio-provided URL, you can create your own URL site that returns HTTP responses.</span></span> <span data-ttu-id="0eaa4-211">Du kan skapa hello webbplats på alla språk som returnerar HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-211">You can create hello site in any language that returns HTTP responses.</span></span> <span data-ttu-id="0eaa4-212">Det här avsnittet förutsätter att du ska vara värd för hello URL från en allmän ASP.NET-hanterare.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-212">This topic assumes you'll be hosting hello URL from an ASP.NET generic handler.</span></span>

<span data-ttu-id="0eaa4-213">hello efter ASP.NET-hanteraren utformar TwiML svar som säger **Hello World** i hello-anropet.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-213">hello following ASP.NET Handler crafts a TwiML response that says **Hello World** on hello call.</span></span>

    using System.Text;
    using System.Web;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {
            public void ProcessRequest(HttpContext context)
            {
                const string twiMLResponse =
                    "<Response><Say>Hello World.</Say></Response>";
                
                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.ContentEncoding = Encoding.UTF8;
                context.Response.Write(twiMLResponse);
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }
    
<span data-ttu-id="0eaa4-214">Du kan se hello-exemplet ovan kan är hello TwiML svar bara ett XML-dokument.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-214">As you can see from hello example above, hello TwiML response is simply an XML document.</span></span> <span data-ttu-id="0eaa4-215">Hej Twilio.TwiML biblioteket innehåller klasser som ska generera TwiML för dig.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-215">hello Twilio.TwiML library contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="0eaa4-216">hello exemplet nedan ger hello motsvarande svar som ovan, men använder hello **VoiceResponse** klass.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-216">hello example below produces hello equivalent response as shown above, but uses hello **VoiceResponse** class.</span></span>

    using System.Web;
    using Twilio.TwiML;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {

            public void ProcessRequest(HttpContext context)
            {
                var twiml = new VoiceResponse();
                twiml.Say("Hello World.");

                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.Write(twiml.ToString());
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }

<span data-ttu-id="0eaa4-217">Läs mer om TwiML [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="0eaa4-217">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).</span></span>

<span data-ttu-id="0eaa4-218">När du har konfigurerat en sätt tooprovide TwiML svar du kan ange den URL-toohello **CallResource.Create** metod.</span><span class="sxs-lookup"><span data-stu-id="0eaa4-218">Once you have set up a way tooprovide TwiML responses, you can pass that URL toohello **CallResource.Create** method.</span></span> <span data-ttu-id="0eaa4-219">Till exempel om du har ett webbprogram med namnet MyTwiML distribueras tooan Azure-molntjänst och hello namnet på din ASP.NET-hanteraren är mytwiml.ashx, hello URL kan skickas för**CallResource.Create** som visas i följande kod hello Exempel:</span><span class="sxs-lookup"><span data-stu-id="0eaa4-219">For example, if you have a web application named MyTwiML deployed tooan Azure cloud service, and hello name of your ASP.NET Handler is mytwiml.ashx, hello URL can be passed too**CallResource.Create** as shown in hello following code sample:</span></span>

    // This sample uses hello sandbox number provided by Twilio toomake hello call.
    // Place hello call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx"));
        }

<span data-ttu-id="0eaa4-220">Mer information om hur du använder Twilio på Azure med ASP.NET finns [hur toomake en telefon ringer med Twilio i en webbroll på Azure][howto_phonecall_dotnet].</span><span class="sxs-lookup"><span data-stu-id="0eaa4-220">For additional information about using Twilio on Azure with ASP.NET, see [How toomake a phone call using Twilio in a web role on Azure][howto_phonecall_dotnet].</span></span>

[!INCLUDE [twilio-additional-services-and-next-steps](../includes/twilio-additional-services-and-next-steps.md)]

[howto_phonecall_dotnet]: partner-twilio-cloud-services-dotnet-phone-call-web-role.md

[twimlet_message_url]: http://twimlets.com/message

[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls

[vs_project]:http://msdn.microsoft.com/library/windowsazure/ee405487.aspx
[nuget]:http://nuget.org/
[twilio_github_repo]:https://github.com/twilio/twilio-csharp

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
