---
title: "Hur du använder Twilio för röst- och SMS (.NET) | Microsoft Docs"
description: "Lär dig att ringa ett telefonsamtal och skicka ett SMS-meddelande med Twilio-API-tjänsten på Azure. Kodexempel som skrivits i .NET."
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
ms.openlocfilehash: 1442e3af26ae87e645cf207228ed1197b2afdd4d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-from-azure"></a><span data-ttu-id="53250-104">Hur du använder Twilio för röst- och SMS-funktioner från Azure</span><span class="sxs-lookup"><span data-stu-id="53250-104">How to use Twilio for voice and SMS capabilities from Azure</span></span>
<span data-ttu-id="53250-105">Den här guiden visar hur du utför vanliga programmeringsuppgifter med Twilio-API-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="53250-105">This guide demonstrates how to perform common programming tasks with the Twilio API service on Azure.</span></span> <span data-ttu-id="53250-106">Scenarier som tas upp inkluderar att ringa och ett kort meddelande (SMS Service)-meddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="53250-106">The scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="53250-107">Mer information om Twilio och använder röst- och SMS i dina program finns i [nästa steg](#NextSteps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="53250-107">For more information on Twilio and using voice and SMS in your applications, see the [Next steps](#NextSteps) section.</span></span>

## <span data-ttu-id="53250-108"><a id="WhatIs"></a>Vad är Twilio?</span><span class="sxs-lookup"><span data-stu-id="53250-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="53250-109">Twilio startar framtiden för kommunikation med, så att utvecklare kan bädda in röst, VoIP och meddelanden i program.</span><span class="sxs-lookup"><span data-stu-id="53250-109">Twilio is powering the future of business communications, enabling developers to embed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="53250-110">De virtualisera alla infrastruktur som behövs i en molnbaserad, globala miljö, exponera den via Twilio kommunikation API-plattformen.</span><span class="sxs-lookup"><span data-stu-id="53250-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through the Twilio communications API platform.</span></span> <span data-ttu-id="53250-111">Programmen är enkla att skapa och skalbara.</span><span class="sxs-lookup"><span data-stu-id="53250-111">Applications are simple to build and scalable.</span></span> <span data-ttu-id="53250-112">Utnyttja flexibilitet med betala per användning priser och dra nytta av molnet tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="53250-112">Enjoy flexibility with pay-as-you-go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="53250-113">**Twilio röst** gör att dina program att göra och ta emot telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="53250-113">**Twilio Voice** allows your applications to make and receive phone calls.</span></span> <span data-ttu-id="53250-114">**Twilio SMS** kan skicka och ta emot SMS-meddelanden-program.</span><span class="sxs-lookup"><span data-stu-id="53250-114">**Twilio SMS** enables your applications to send and receive SMS messages.</span></span> <span data-ttu-id="53250-115">**Twilio klienten** kan du se VoIP-anrop från telefon, surfplatta eller webbläsare och stöder WebRTC.</span><span class="sxs-lookup"><span data-stu-id="53250-115">**Twilio Client** allows you to make VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="53250-116"><a id="Pricing"></a>Priser för Twilio och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="53250-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="53250-117">Azure-kunder får en [specialerbjudande](http://www.twilio.com/azure): kostnadsfri $10 Twilio kredit när du uppgraderar ditt Twilio-konto.</span><span class="sxs-lookup"><span data-stu-id="53250-117">Azure customers receive a [special offer](http://www.twilio.com/azure): complimentary $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="53250-118">Den här Twilio-kredit kan tillämpas på alla Twilio-användning ($10 kredit motsvarar upp till 1 000 SMS-meddelanden skickas eller tas emot upp till 1 000 inkommande röst minuter beroende på platsen för ditt mål för telefon och meddelandet eller samtal).</span><span class="sxs-lookup"><span data-stu-id="53250-118">This Twilio Credit can be applied to any Twilio usage ($10 credit equivalent to sending as many as 1,000 SMS messages or receiving up to 1000 inbound Voice minutes, depending on the location of your phone number and message or call destination).</span></span> <span data-ttu-id="53250-119">Lösa in den här Twilio-kredit och kom igång med [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span><span class="sxs-lookup"><span data-stu-id="53250-119">Redeem this Twilio credit and get started at [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span></span>

<span data-ttu-id="53250-120">Twilio är en betalning per användning.</span><span class="sxs-lookup"><span data-stu-id="53250-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="53250-121">Det finns inga avgifter för installation och du kan stänga ditt konto när som helst.</span><span class="sxs-lookup"><span data-stu-id="53250-121">There are no set-up fees, and you can close your account at any time.</span></span> <span data-ttu-id="53250-122">Du hittar mer information i [Twilio priser](http://www.twilio.com/voice/pricing).</span><span class="sxs-lookup"><span data-stu-id="53250-122">You can find more details at [Twilio Pricing](http://www.twilio.com/voice/pricing).</span></span>

## <span data-ttu-id="53250-123"><a id="Concepts"></a>Begrepp</span><span class="sxs-lookup"><span data-stu-id="53250-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="53250-124">Twilio-API är en RESTful-API som tillhandahåller röst- och SMS-funktioner för program.</span><span class="sxs-lookup"><span data-stu-id="53250-124">The Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="53250-125">Klientbibliotek är tillgängliga på flera språk. en lista, se [Twilio-API-bibliotek][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="53250-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="53250-126">Viktiga aspekter av Twilio-API: et är Twilio verb och Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="53250-126">Key aspects of the Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="53250-127"><a id="Verbs"></a>Twilio-verb</span><span class="sxs-lookup"><span data-stu-id="53250-127"><a id="Verbs"></a>Twilio verbs</span></span>
<span data-ttu-id="53250-128">API: et tillämpar Twilio verb; till exempel den  **&lt;säg&gt;**  verb instruerar Twilio att hörbart leverera ett meddelande på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="53250-128">The API makes use of Twilio verbs; for example, the **&lt;Say&gt;** verb instructs Twilio to audibly deliver a message on a call.</span></span>

<span data-ttu-id="53250-129">Följande är en lista över Twilio verb.</span><span class="sxs-lookup"><span data-stu-id="53250-129">The following is a list of Twilio verbs.</span></span>  <span data-ttu-id="53250-130">Lär dig mer om vilka verb och funktioner via [Markup Language Twilio dokumentationen](http://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="53250-130">Learn about the other verbs and capabilities via [Twilio Markup Language documentation](http://www.twilio.com/docs/api/twiml).</span></span>

* <span data-ttu-id="53250-131">**&lt;Ring&gt;**: anroparen ansluter till en annan telefon.</span><span class="sxs-lookup"><span data-stu-id="53250-131">**&lt;Dial&gt;**: Connects the caller to another phone.</span></span>
* <span data-ttu-id="53250-132">**&lt;Samla in&gt;**: samlar in siffror som anges på telefon tangentbordet.</span><span class="sxs-lookup"><span data-stu-id="53250-132">**&lt;Gather&gt;**: Collects numeric digits entered on the telephone keypad.</span></span>
* <span data-ttu-id="53250-133">**&lt;Koppla ned&gt;**: slutar ett anrop.</span><span class="sxs-lookup"><span data-stu-id="53250-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="53250-134">**&lt;Spela upp&gt;**: spelar en ljudfil.</span><span class="sxs-lookup"><span data-stu-id="53250-134">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="53250-135">**&lt;Pausa&gt;**: tyst väntar på ett angivet antal sekunder.</span><span class="sxs-lookup"><span data-stu-id="53250-135">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="53250-136">**&lt;Posten&gt;**: registrerar anroparens röst och returnerar ett URL-Adressen till en fil som innehåller inspelningen.</span><span class="sxs-lookup"><span data-stu-id="53250-136">**&lt;Record&gt;**: Records the caller's voice and returns an URL of a file that contains the recording.</span></span>
* <span data-ttu-id="53250-137">**&lt;Omdirigera&gt;**: Överför kontroll över ett samtal eller SMS till TwiML på en annan URL.</span><span class="sxs-lookup"><span data-stu-id="53250-137">**&lt;Redirect&gt;**: Transfers control of a call or SMS to the TwiML at a different URL.</span></span>
* <span data-ttu-id="53250-138">**&lt;Avvisa&gt;**: avvisar ett inkommande samtal till Twilio-nummer utan fakturering du</span><span class="sxs-lookup"><span data-stu-id="53250-138">**&lt;Reject&gt;**: Rejects an incoming call to your Twilio number without billing you</span></span>
* <span data-ttu-id="53250-139">**&lt;Säg&gt;**: konverterar text till tal, som görs på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="53250-139">**&lt;Say&gt;**: Converts text to speech that is made on a call.</span></span>
* <span data-ttu-id="53250-140">**&lt;SMS&gt;**: skickar ett SMS-meddelande.</span><span class="sxs-lookup"><span data-stu-id="53250-140">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="53250-141"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="53250-141"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="53250-142">TwiML är en uppsättning XML-baserade instruktioner baserat på de Twilio-verb som informerar Twilio för att behandla ett samtal eller SMS.</span><span class="sxs-lookup"><span data-stu-id="53250-142">TwiML is a set of XML-based instructions based on the Twilio verbs that inform Twilio of how to process a call or SMS.</span></span>

<span data-ttu-id="53250-143">Följande TwiML skulle exempelvis Omvandla text **Hello World** till tal.</span><span class="sxs-lookup"><span data-stu-id="53250-143">As an example, the following TwiML would convert the text **Hello World** to speech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

<span data-ttu-id="53250-144">När programmet anropar Twilio-API, är en av parametrarna API den URL som returnerar TwiML-svar.</span><span class="sxs-lookup"><span data-stu-id="53250-144">When your application calls the Twilio API, one of the API parameters is the URL that returns the TwiML response.</span></span> <span data-ttu-id="53250-145">Du kan använda Twilio-tillhandahållna URL: er för utveckling, ange TwiML-svar som används av dina program.</span><span class="sxs-lookup"><span data-stu-id="53250-145">For development purposes, you can use Twilio-provided URLs to provide the TwiML responses used by your applications.</span></span> <span data-ttu-id="53250-146">Du kan också vara värd för din egen URL: er för att skapa TwiML-svar och ett annat alternativ är att använda den **TwiMLResponse** objekt.</span><span class="sxs-lookup"><span data-stu-id="53250-146">You could also host your own URLs to produce the TwiML responses, and another option is to use the **TwiMLResponse** object.</span></span>

<span data-ttu-id="53250-147">Läs mer om Twilio verb, deras attribut och TwiML [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="53250-147">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="53250-148">Mer information om Twilio-API finns [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="53250-148">For additional information about the Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="53250-149"><a id="CreateAccount"></a>Skapa ett Twilio-konto</span><span class="sxs-lookup"><span data-stu-id="53250-149"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="53250-150">När du är redo att skaffa ett Twilio-konto kan logga på [försök Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="53250-150">When you're ready to get a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="53250-151">Du kan börja med ett kostnadsfritt konto och uppgradera ditt konto senare.</span><span class="sxs-lookup"><span data-stu-id="53250-151">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="53250-152">När du registrerar dig för ett Twilio-konto får du ett konto-ID och ett token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="53250-152">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="53250-153">Både behövs för att göra Twilio API-anrop.</span><span class="sxs-lookup"><span data-stu-id="53250-153">Both will be needed to make Twilio API calls.</span></span> <span data-ttu-id="53250-154">För att förhindra obehörig åtkomst till ditt konto, skydda din token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="53250-154">To prevent unauthorized access to your account, keep your authentication token secure.</span></span> <span data-ttu-id="53250-155">Ditt konto-ID och autentisering token kan visas på den [Twilio kontosida][twilio_account], i fälten med etiketten **konto SID** och **AUTH TOKEN**respektive.</span><span class="sxs-lookup"><span data-stu-id="53250-155">Your account ID and authentication token are viewable at the [Twilio account page][twilio_account], in the fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="53250-156"><a id="create_app"></a>Skapa ett Azure-program</span><span class="sxs-lookup"><span data-stu-id="53250-156"><a id="create_app"></a>Create an Azure Application</span></span>
<span data-ttu-id="53250-157">Ett Azure-program som är värd för ett Twilio-aktiverat program skiljer sig inte från andra Azure-program.</span><span class="sxs-lookup"><span data-stu-id="53250-157">An Azure application that hosts a Twilio enabled application is no different from any other Azure application.</span></span> <span data-ttu-id="53250-158">Du lägger till Twilio .NET-bibliotek och konfigurera roll om du vill använda Twilio .NET-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="53250-158">You add the Twilio .NET library and configure the role to use the Twilio .NET libraries.</span></span>
<span data-ttu-id="53250-159">Information om hur du skapar ett första Azure-projekt finns [skapa ett Azure-projekt i Visual Studio][vs_project].</span><span class="sxs-lookup"><span data-stu-id="53250-159">For information on creating an initial Azure project, see [Creating an Azure project with Visual Studio][vs_project].</span></span>

## <span data-ttu-id="53250-160"><a id="configure_app"></a>Konfigurera ditt program att använda Twilio-bibliotek</span><span class="sxs-lookup"><span data-stu-id="53250-160"><a id="configure_app"></a>Configure Your Application to use Twilio Libraries</span></span>
<span data-ttu-id="53250-161">Twilio innehåller en uppsättning .NET helper bibliotek som omsluter olika aspekter av Twilio att tillhandahålla enkel och enkelt sätt att interagera med Twilio REST-API och Twilio-klienten för att generera TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="53250-161">Twilio provides a set of .NET helper libraries that wrap various aspects of Twilio to provide simple and easy ways to interact with the Twilio REST API and Twilio Client to generate TwiML responses.</span></span>

<span data-ttu-id="53250-162">Twilio innehåller fem bibliotek för .NET-utvecklare:</span><span class="sxs-lookup"><span data-stu-id="53250-162">Twilio provides five libraries for .NET developers:</span></span>
<span data-ttu-id="53250-163">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="53250-163">Library</span></span>|<span data-ttu-id="53250-164">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53250-164">Description</span></span>
---|---
<span data-ttu-id="53250-165">Twilio.API</span><span class="sxs-lookup"><span data-stu-id="53250-165">Twilio.API</span></span>|<span data-ttu-id="53250-166">Kärnbibliotek som omsluter Twilio REST API i ett eget .NET-bibliotek för Twilio.</span><span class="sxs-lookup"><span data-stu-id="53250-166">The core Twilio library that wraps the Twilio REST API in a friendly .NET library.</span></span> <span data-ttu-id="53250-167">Det här biblioteket är tillgängliga för .NET, Silverlight och Windows Phone 7.</span><span class="sxs-lookup"><span data-stu-id="53250-167">This library is available for .NET, Silverlight, and Windows Phone 7.</span></span>
<span data-ttu-id="53250-168">Twilio.TwiML</span><span class="sxs-lookup"><span data-stu-id="53250-168">Twilio.TwiML</span></span>|<span data-ttu-id="53250-169">Ger ett .NET egna sätt att generera TwiML markering.</span><span class="sxs-lookup"><span data-stu-id="53250-169">Provides a .NET friendly way to generate TwiML markup.</span></span>
<span data-ttu-id="53250-170">Twilio.MVC</span><span class="sxs-lookup"><span data-stu-id="53250-170">Twilio.MVC</span></span>|<span data-ttu-id="53250-171">För utvecklare som använder ASP.NET MVC, innehåller det här biblioteket en TwilioController och TwiML ActionResult begäran verifieringsattribut.</span><span class="sxs-lookup"><span data-stu-id="53250-171">For developers using ASP.NET MVC, this library includes a TwilioController, TwiML ActionResult, and request validation attribute.</span></span>
<span data-ttu-id="53250-172">Twilio.WebMatrix</span><span class="sxs-lookup"><span data-stu-id="53250-172">Twilio.WebMatrix</span></span>|<span data-ttu-id="53250-173">För utvecklare som använder Microsofts ledigt WebMatrix utvecklingsverktyg för innehåller det här biblioteket Razor syntax hjälpprogram för olika Twilio-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="53250-173">For developers using Microsoft's free WebMatrix development tool, this library contains Razor syntax helpers for various Twilio actions.</span></span>
<span data-ttu-id="53250-174">Twilio.Client.Capability</span><span class="sxs-lookup"><span data-stu-id="53250-174">Twilio.Client.Capability</span></span>|<span data-ttu-id="53250-175">Innehåller funktionen token generatorn för användning med Twilio klienten JavaScript SDK.</span><span class="sxs-lookup"><span data-stu-id="53250-175">Contains the Capability token generator for use with the Twilio Client JavaScript SDK.</span></span>

<span data-ttu-id="53250-176">Observera att alla bibliotek kräver .NET 3.5, Silverlight 4 eller Windows Phone 7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="53250-176">Note that all libraries require .NET 3.5, Silverlight 4, or Windows Phone 7 or later.</span></span>

<span data-ttu-id="53250-177">Exemplen i den här guiden använder Twilio.API-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="53250-177">The samples provided in this guide use the Twilio.API library.</span></span>

<span data-ttu-id="53250-178">Biblioteken kan vara [installeras med filnamnstillägget NuGet package manager](http://www.twilio.com/docs/csharp/install) tillgängliga för Visual Studio 2010 upp till 2015.</span><span class="sxs-lookup"><span data-stu-id="53250-178">The libraries can be [installed using the NuGet package manager extension](http://www.twilio.com/docs/csharp/install) available for Visual Studio 2010 up to 2015.</span></span>  <span data-ttu-id="53250-179">Källkoden finns på [GitHub][twilio_github_repo], som innehåller en Wiki som innehåller komplett dokumentation för att använda biblioteken.</span><span class="sxs-lookup"><span data-stu-id="53250-179">The source code is hosted on [GitHub][twilio_github_repo], which includes a Wiki that contains complete documentation for using the libraries.</span></span>

<span data-ttu-id="53250-180">Som standard installerar Microsoft Visual Studio 2010 version 1.2 av NuGet.</span><span class="sxs-lookup"><span data-stu-id="53250-180">By default, Microsoft Visual Studio 2010 installs version 1.2 of NuGet.</span></span> <span data-ttu-id="53250-181">Installera Twilio-bibliotek kräver version 1.6 i NuGet eller högre.</span><span class="sxs-lookup"><span data-stu-id="53250-181">Installing the Twilio libraries requires version 1.6 of NuGet or higher.</span></span> <span data-ttu-id="53250-182">Mer information om installation eller uppdatering av NuGet finns [http://nuget.org/][nuget].</span><span class="sxs-lookup"><span data-stu-id="53250-182">For information on installing or updating NuGet, see [http://nuget.org/][nuget].</span></span>

> [!NOTE]
> <span data-ttu-id="53250-183">Om du vill installera den senaste versionen av NuGet måste du först avinstallera den inlästa versionen med hjälp av Tilläggshanteraren för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="53250-183">To install the latest version of NuGet, you must first uninstall the loaded version using the Visual Studio Extension Manager.</span></span> <span data-ttu-id="53250-184">Om du vill göra det, måste du köra Visual Studio som administratör.</span><span class="sxs-lookup"><span data-stu-id="53250-184">To do so, you must run Visual Studio as administrator.</span></span> <span data-ttu-id="53250-185">I annat fall inaktiveras knappen Avinstallera.</span><span class="sxs-lookup"><span data-stu-id="53250-185">Otherwise, the Uninstall button is disabled.</span></span>
>
>

### <span data-ttu-id="53250-186"><a id="use_nuget"></a>Lägga till Twilio-bibliotek i Visual Studio-projekt:</span><span class="sxs-lookup"><span data-stu-id="53250-186"><a id="use_nuget"></a>To add the Twilio libraries to your Visual Studio project:</span></span>
1. <span data-ttu-id="53250-187">Öppna din lösning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="53250-187">Open your solution in Visual Studio.</span></span>
2. <span data-ttu-id="53250-188">Högerklicka på **referenser**.</span><span class="sxs-lookup"><span data-stu-id="53250-188">Right-click **References**.</span></span>
3. <span data-ttu-id="53250-189">Klicka på **hantera NuGet-paket...**</span><span class="sxs-lookup"><span data-stu-id="53250-189">Click **Manage NuGet Packages...**</span></span>
4. <span data-ttu-id="53250-190">Klicka på **Online**.</span><span class="sxs-lookup"><span data-stu-id="53250-190">Click **Online**.</span></span>
5. <span data-ttu-id="53250-191">Skriv i sökrutan online *twilio*.</span><span class="sxs-lookup"><span data-stu-id="53250-191">In the search online box, type *twilio*.</span></span>
6. <span data-ttu-id="53250-192">Klicka på **installera** på Twilio-paketet.</span><span class="sxs-lookup"><span data-stu-id="53250-192">Click **Install** on the Twilio package.</span></span>

## <span data-ttu-id="53250-193"><a id="howto_make_call"></a>Så här: göra en utgående anrop</span><span class="sxs-lookup"><span data-stu-id="53250-193"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="53250-194">Följande visar hur du gör en utgående Ring upp med den **CallResource** klass.</span><span class="sxs-lookup"><span data-stu-id="53250-194">The following shows how to make an outgoing call using the **CallResource** class.</span></span> <span data-ttu-id="53250-195">Den här koden används även en webbplats med angivna Twilio för att returnera svaret Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="53250-195">This code also uses a Twilio-provided site to return the Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="53250-196">Ersätt värdena för den **till** och **från** telefonnummer och se till att du kontrollerar den **från** telefonnummer för ditt Twilio-konto innan du kör koden.</span><span class="sxs-lookup"><span data-stu-id="53250-196">Substitute your values for the **to** and **from** phone numbers, and ensure that you verify the **from** phone number for your Twilio account before running the code.</span></span>

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize the TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    // Use the Twilio-provided site for the TwiML response.
    var url = "http://twimlets.com/message";
    url = $"{url}?Message%5B0%5D=Hello%20World";

    // Set the call From, To, and URL values to use for the call.
    // This sample uses the sandbox number provided by
    // Twilio to make the call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri(url));
        }

<span data-ttu-id="53250-197">Mer information om de parametrar som skickas till den **CallResource.Create** -metoden finns [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span><span class="sxs-lookup"><span data-stu-id="53250-197">For more information about the parameters passed in to the **CallResource.Create** method, see [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span></span>

<span data-ttu-id="53250-198">Som tidigare nämnts kan används den här koden en angivna Twilio plats för att returnera TwiML svaret.</span><span class="sxs-lookup"><span data-stu-id="53250-198">As mentioned, this code uses a Twilio-provided site to return the TwiML response.</span></span> <span data-ttu-id="53250-199">Du kan använda din egen webbplats i stället för att tillhandahålla TwiML svaret.</span><span class="sxs-lookup"><span data-stu-id="53250-199">You could instead use your own site to provide the TwiML response.</span></span> <span data-ttu-id="53250-200">Mer information finns i [så här: Ange TwiML svar från din egen webbplats](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="53250-200">For more information, see [How to: Provide TwiML responses from your own website](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="53250-201"><a id="howto_send_sms"></a>Så här: skicka ett SMS-meddelande</span><span class="sxs-lookup"><span data-stu-id="53250-201"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="53250-202">Följande skärmbild visar hur du skickar ett SMS-meddelande med den **MessageResource** klass.</span><span class="sxs-lookup"><span data-stu-id="53250-202">The following screenshot shows how to send an SMS message using the **MessageResource**  class.</span></span> <span data-ttu-id="53250-203">Den **från** nummer tillhandahålls av Twilio för utvärderingskonton att skicka SMS-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="53250-203">The **from** number is provided by Twilio for trial accounts to send SMS messages.</span></span> <span data-ttu-id="53250-204">Den **till** numret måste verifieras för ditt Twilio-konto innan du kör kod.</span><span class="sxs-lookup"><span data-stu-id="53250-204">The **to** number must be verified for your Twilio account before you run the code.</span></span>

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize the TwilioClient.
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
        // An exception occurred making the REST call
        Console.WriteLine(ex.Message);
    }

## <span data-ttu-id="53250-205"><a id="howto_provide_twiml_responses"></a>Så här: Ange TwiML svar från din egen webbplats</span><span class="sxs-lookup"><span data-stu-id="53250-205"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own website</span></span>
<span data-ttu-id="53250-206">När ditt program initierar ett anrop till Twilio-API – exempelvis den **CallResource.Create** metod - Twilio skickar din begäran till en URL som förväntas returnera ett TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="53250-206">When your application initiates a call to the Twilio API - for example, via the **CallResource.Create** method - Twilio sends your request to an URL that is expected to return a TwiML response.</span></span> <span data-ttu-id="53250-207">Exemplet i [så här: utgående ringa](#howto_make_call) använder den angivna Twilio URL [http://twimlets.com/message] [ twimlet_message_url] att returnera svaret.</span><span class="sxs-lookup"><span data-stu-id="53250-207">The example in [How to: Make an outgoing call](#howto_make_call) uses the Twilio-provided URL [http://twimlets.com/message][twimlet_message_url] to return the response.</span></span>

> [!NOTE]
> <span data-ttu-id="53250-208">När TwiML är avsedd för användning av webbtjänster, kan du visa TwiML i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="53250-208">While TwiML is designed for use by web services, you can view the TwiML in your browser.</span></span> <span data-ttu-id="53250-209">Klicka till exempel [http://twimlets.com/message] [ twimlet_message_url] att se en tom &lt;svar&gt; element, klicka på ett annat exempel är [http://twimlets.com/message? Meddelande % 5B0 %5 D = Hello % 20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) att se en &lt;svar&gt; element som innehåller en &lt;säg&gt; element.</span><span class="sxs-lookup"><span data-stu-id="53250-209">For example, click [http://twimlets.com/message][twimlet_message_url] to see an empty &lt;Response&gt; element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) to see a &lt;Response&gt; element that contains a &lt;Say&gt; element.</span></span>
>
>

<span data-ttu-id="53250-210">I stället för på den angivna Twilio URL, kan du skapa din egen URL: en webbplats som returnerar HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="53250-210">Instead of relying on the Twilio-provided URL, you can create your own URL site that returns HTTP responses.</span></span> <span data-ttu-id="53250-211">Du kan skapa webbplatsen på alla språk som returnerar HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="53250-211">You can create the site in any language that returns HTTP responses.</span></span> <span data-ttu-id="53250-212">Det här avsnittet förutsätter att du ska vara värd för URL: en från en allmän ASP.NET-hanterare.</span><span class="sxs-lookup"><span data-stu-id="53250-212">This topic assumes you'll be hosting the URL from an ASP.NET generic handler.</span></span>

<span data-ttu-id="53250-213">Följande ASP.NET-hanteraren utformar TwiML svar som säger **Hello World** på anropet.</span><span class="sxs-lookup"><span data-stu-id="53250-213">The following ASP.NET Handler crafts a TwiML response that says **Hello World** on the call.</span></span>

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
    
<span data-ttu-id="53250-214">Du ser i exemplet ovan, är TwiML svaret bara ett XML-dokument.</span><span class="sxs-lookup"><span data-stu-id="53250-214">As you can see from the example above, the TwiML response is simply an XML document.</span></span> <span data-ttu-id="53250-215">Twilio.TwiML-bibliotek innehåller klasser som ska generera TwiML för dig.</span><span class="sxs-lookup"><span data-stu-id="53250-215">The Twilio.TwiML library contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="53250-216">Exemplet nedan skapar motsvarande svaret som ovan, men använder den **VoiceResponse** klass.</span><span class="sxs-lookup"><span data-stu-id="53250-216">The example below produces the equivalent response as shown above, but uses the **VoiceResponse** class.</span></span>

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

<span data-ttu-id="53250-217">Läs mer om TwiML [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="53250-217">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).</span></span>

<span data-ttu-id="53250-218">När du har konfigurerat ett sätt att ange TwiML svar du kan ange Webbadressen till den **CallResource.Create** metod.</span><span class="sxs-lookup"><span data-stu-id="53250-218">Once you have set up a way to provide TwiML responses, you can pass that URL to the **CallResource.Create** method.</span></span> <span data-ttu-id="53250-219">Till exempel om du har ett webbprogram med namnet MyTwiML som distribueras till en Azure-molntjänst och namnet på din ASP.NET-hanteraren är mytwiml.ashx, URL: en kan skickas till **CallResource.Create** som visas i följande kodexempel:</span><span class="sxs-lookup"><span data-stu-id="53250-219">For example, if you have a web application named MyTwiML deployed to an Azure cloud service, and the name of your ASP.NET Handler is mytwiml.ashx, the URL can be passed to **CallResource.Create** as shown in the following code sample:</span></span>

    // This sample uses the sandbox number provided by Twilio to make the call.
    // Place the call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx"));
        }

<span data-ttu-id="53250-220">Mer information om hur du använder Twilio på Azure med ASP.NET finns [så att ringa ett telefonsamtal med Twilio i en webbroll på Azure][howto_phonecall_dotnet].</span><span class="sxs-lookup"><span data-stu-id="53250-220">For additional information about using Twilio on Azure with ASP.NET, see [How to make a phone call using Twilio in a web role on Azure][howto_phonecall_dotnet].</span></span>

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
