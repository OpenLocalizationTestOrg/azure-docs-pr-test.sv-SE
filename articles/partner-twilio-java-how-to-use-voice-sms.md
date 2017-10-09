---
title: "aaaHow tooUse Twilio för röst- och SMS (Java) | Microsoft Docs"
description: "Lär dig hur toomake ett telefonsamtal och skicka ett SMS-meddelandet med hello Twilio API-tjänsten på Azure. Kodexempel som skrivits i Java."
services: 
documentationcenter: java
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: f3508965-5527-4255-9d51-5d5f926f4d43
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: a186e2c8e73ced928bd0dec348971034f10ba82c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-java"></a><span data-ttu-id="05614-104">Hur tooUse Twilio för röst- och SMS-funktioner i Java</span><span class="sxs-lookup"><span data-stu-id="05614-104">How tooUse Twilio for Voice and SMS Capabilities in Java</span></span>
<span data-ttu-id="05614-105">Den här guiden visar hur tooperform vanliga programmeringsuppgifter med hello Twilio API-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="05614-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="05614-106">hello-scenarier som tas upp inkluderar att ringa och ett kort meddelande (SMS Service)-meddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="05614-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="05614-107">Mer information om Twilio och använder röst- och SMS i dina program finns hello [nästa steg](#NextSteps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="05614-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="05614-108"><a id="WhatIs"></a>Vad är Twilio?</span><span class="sxs-lookup"><span data-stu-id="05614-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="05614-109">Twilio är en telefoni webbtjänst API som kan du använda din befintliga web språk och kunskaper toobuild röst och SMS-program.</span><span class="sxs-lookup"><span data-stu-id="05614-109">Twilio is a telephony web-service API that lets you use your existing web languages and skills toobuild voice and SMS applications.</span></span> <span data-ttu-id="05614-110">Twilio är en från tredje part (inte en funktion i Azure och inte en Microsoft-produkt).</span><span class="sxs-lookup"><span data-stu-id="05614-110">Twilio is a third-party service (not an Azure feature and not a Microsoft product).</span></span>

<span data-ttu-id="05614-111">**Twilio röst** kan ditt program toomake och ta emot telefonsamtal.</span><span class="sxs-lookup"><span data-stu-id="05614-111">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span> <span data-ttu-id="05614-112">**Twilio SMS** kan ditt program toomake och ta emot SMS-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="05614-112">**Twilio SMS** allows your applications toomake and receive SMS messages.</span></span> <span data-ttu-id="05614-113">**Twilio klienten** gör att dina program tooenable röstkommunikation via befintliga Internet-anslutningar, inklusive mobila anslutningar.</span><span class="sxs-lookup"><span data-stu-id="05614-113">**Twilio Client** allows your applications tooenable voice communication using existing Internet connections, including mobile connections.</span></span>

## <span data-ttu-id="05614-114"><a id="Pricing"></a>Priser för Twilio och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="05614-114"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="05614-115">Information om priser Twilio finns på [Twilio priser][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="05614-115">Information about Twilio pricing is available at [Twilio Pricing][twilio_pricing].</span></span> <span data-ttu-id="05614-116">Azure-kunder får en [specialerbjudande][special_offer]: en kredit på 1000 texter eller 1000 inkommande minuter.</span><span class="sxs-lookup"><span data-stu-id="05614-116">Azure customers receive a [special offer][special_offer]: a free credit of 1000 texts or 1000 inbound minutes.</span></span> <span data-ttu-id="05614-117">toosign för detta erbjudande eller få mer information, besök [http://ahoy.twilio.com/azure][special_offer].</span><span class="sxs-lookup"><span data-stu-id="05614-117">toosign up for this offer or get more information, please visit [http://ahoy.twilio.com/azure][special_offer].</span></span>

## <span data-ttu-id="05614-118"><a id="Concepts"></a>Begrepp</span><span class="sxs-lookup"><span data-stu-id="05614-118"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="05614-119">Hej Twilio API är en RESTful-API som tillhandahåller röst- och SMS-funktioner för program.</span><span class="sxs-lookup"><span data-stu-id="05614-119">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="05614-120">Klientbibliotek är tillgängliga på flera språk. en lista, se [Twilio-API-bibliotek][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="05614-120">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="05614-121">Viktiga aspekter av hello Twilio API är Twilio verb och Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="05614-121">Key aspects of hello Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="05614-122"><a id="Verbs"></a>Twilio-verb</span><span class="sxs-lookup"><span data-stu-id="05614-122"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="05614-123">hello API väljer Twilio verb; till exempel hello  **&lt;säg&gt;**  verb instruerar Twilio tooaudibly leverera ett meddelande på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="05614-123">hello API makes use of Twilio verbs; for example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span>

<span data-ttu-id="05614-124">hello följer en lista över Twilio verb.</span><span class="sxs-lookup"><span data-stu-id="05614-124">hello following is a list of Twilio verbs.</span></span>

* <span data-ttu-id="05614-125">**&lt;Ring&gt;**: ansluter hello anroparen tooanother phone.</span><span class="sxs-lookup"><span data-stu-id="05614-125">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="05614-126">**&lt;Samla in&gt;**: samlar in siffror som anges på hello telefon tangentbordet.</span><span class="sxs-lookup"><span data-stu-id="05614-126">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="05614-127">**&lt;Koppla ned&gt;**: slutar ett anrop.</span><span class="sxs-lookup"><span data-stu-id="05614-127">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="05614-128">**&lt;Spela upp&gt;**: spelar en ljudfil.</span><span class="sxs-lookup"><span data-stu-id="05614-128">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="05614-129">**&lt;Kön&gt;**: Lägg till hello tooa kö med anropare.</span><span class="sxs-lookup"><span data-stu-id="05614-129">**&lt;Queue&gt;**: Add hello tooa queue of callers.</span></span>
* <span data-ttu-id="05614-130">**&lt;Pausa&gt;**: tyst väntar på ett angivet antal sekunder.</span><span class="sxs-lookup"><span data-stu-id="05614-130">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="05614-131">**&lt;Posten&gt;**: registrerar hello anroparen röst och returnerar en URL för en fil som innehåller hello registrering.</span><span class="sxs-lookup"><span data-stu-id="05614-131">**&lt;Record&gt;**: Records hello caller's voice and returns a URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="05614-132">**&lt;Omdirigera&gt;**: Överför kontroll över ett samtal eller SMS toohello TwiML på en annan URL.</span><span class="sxs-lookup"><span data-stu-id="05614-132">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="05614-133">**&lt;Avvisa&gt;**: avvisar en inkommande anropa tooyour Twilio tal utan fakturering du.</span><span class="sxs-lookup"><span data-stu-id="05614-133">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you.</span></span>
* <span data-ttu-id="05614-134">**&lt;Säg&gt;**: konverterar text toospeech som görs på ett samtal.</span><span class="sxs-lookup"><span data-stu-id="05614-134">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="05614-135">**&lt;SMS&gt;**: skickar ett SMS-meddelande.</span><span class="sxs-lookup"><span data-stu-id="05614-135">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="05614-136"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="05614-136"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="05614-137">TwiML är en uppsättning XML-baserade instruktioner baserat på hello Twilio verb som informerar Twilio på hur tooprocess ett samtal eller SMS.</span><span class="sxs-lookup"><span data-stu-id="05614-137">TwiML is a set of XML-based instructions based on hello Twilio verbs that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="05614-138">Exempelvis skulle hello följande TwiML omvandla hello text **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="05614-138">As an example, hello following TwiML would convert hello text **Hello World!**</span></span> <span data-ttu-id="05614-139">toospeech.</span><span class="sxs-lookup"><span data-stu-id="05614-139">toospeech.</span></span>

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

<span data-ttu-id="05614-140">När programmet-anrop hello Twilio-API, är en av parametrarna för hello API hello URL som returnerar hello TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="05614-140">When your application calls hello Twilio API, one of hello API parameters is hello URL that returns hello TwiML response.</span></span> <span data-ttu-id="05614-141">För utveckling, kan du använda Twilio-tillhandahållna URL: er tooprovide hello TwiML svar som används av dina program.</span><span class="sxs-lookup"><span data-stu-id="05614-141">For development purposes, you can use Twilio-provided URLs tooprovide hello TwiML responses used by your applications.</span></span> <span data-ttu-id="05614-142">Du kan också vara värd för dina egna URL: er tooproduce hello TwiML svar och ett annat alternativ är toouse hello **TwiMLResponse** objekt.</span><span class="sxs-lookup"><span data-stu-id="05614-142">You could also host your own URLs tooproduce hello TwiML responses, and another option is toouse hello **TwiMLResponse** object.</span></span>

<span data-ttu-id="05614-143">Läs mer om Twilio verb, deras attribut och TwiML [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="05614-143">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="05614-144">Mer information om hello Twilio-API finns [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="05614-144">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="05614-145"><a id="CreateAccount"></a>Skapa ett Twilio-konto</span><span class="sxs-lookup"><span data-stu-id="05614-145"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="05614-146">När du är klar tooget Twilio-konto kan logga på [försök Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="05614-146">When you're ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="05614-147">Du kan börja med ett kostnadsfritt konto och uppgradera ditt konto senare.</span><span class="sxs-lookup"><span data-stu-id="05614-147">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="05614-148">När du registrerar dig för ett Twilio-konto får du ett konto-ID och ett token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="05614-148">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="05614-149">Båda ska vara nödvändiga toomake Twilio API-anrop.</span><span class="sxs-lookup"><span data-stu-id="05614-149">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="05614-150">tooprevent obehörig åtkomst till tooyour konto, skydda din token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="05614-150">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="05614-151">Ditt konto-ID och autentisering token kan visas på hello [Twilio konsolen][twilio_console]i hello fält med namnet **konto SID** och **AUTH TOKEN**respektive.</span><span class="sxs-lookup"><span data-stu-id="05614-151">Your account ID and authentication token are viewable at hello [Twilio Console][twilio_console], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="05614-152"><a id="create_app"></a>Skapa ett Java-program</span><span class="sxs-lookup"><span data-stu-id="05614-152"><a id="create_app"></a>Create a Java Application</span></span>
1. <span data-ttu-id="05614-153">Hämta hello Twilio JAR och Lägg den tooyour Java skapa sökvägen och WAR-distribution för sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="05614-153">Obtain hello Twilio JAR and add it tooyour Java build path and your WAR deployment assembly.</span></span> <span data-ttu-id="05614-154">Vid [https://github.com/twilio/twilio-java][twilio_java], kan du hämta hello GitHub källor och skapa egna JAR eller hämta en förskapad JAR (med eller utan beroenden).</span><span class="sxs-lookup"><span data-stu-id="05614-154">At [https://github.com/twilio/twilio-java][twilio_java], you can download hello GitHub sources and create your own JAR, or download a pre-built JAR (with or without dependencies).</span></span>
2. <span data-ttu-id="05614-155">Se till att din JDK **cacerts** keystore innehåller hello Equifax certifikatutfärdare för säkra-certifikat med MD5 fingeravtryck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello serienummer är 35:DE:F4:CF och hello SHA1 fingeravtryck är D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A).</span><span class="sxs-lookup"><span data-stu-id="05614-155">Ensure your JDK's **cacerts** keystore contains hello Equifax Secure Certificate Authority certificate with MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello serial number is 35:DE:F4:CF and hello SHA1 fingerprint is D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A).</span></span> <span data-ttu-id="05614-156">Det här är hello certifikat certifikatutfärdarens certifikat för hello [https://api.twilio.com] [ twilio_api_service] -tjänsten som anropas när du använder Twilio APIs.</span><span class="sxs-lookup"><span data-stu-id="05614-156">This is hello certificate authority (CA) certificate for hello [https://api.twilio.com][twilio_api_service] service, which is called when you use Twilio APIs.</span></span> <span data-ttu-id="05614-157">Information om att säkerställa ditt JDK **cacerts** keystore innehåller hello rätt CA-certifikat, se [att lägga till ett certifikat toohello Java CA-certifikatarkivet][add_ca_cert].</span><span class="sxs-lookup"><span data-stu-id="05614-157">For information about ensuring your JDK's **cacerts** keystore contains hello correct CA certificate, see [Adding a Certificate toohello Java CA Certificate Store][add_ca_cert].</span></span>

<span data-ttu-id="05614-158">Detaljerade anvisningar för att använda hello Twilio-klientbibliotek för Java finns på [hur tooMake ett telefonsamtal med Twilio i ett Java-program på Azure][howto_phonecall_java].</span><span class="sxs-lookup"><span data-stu-id="05614-158">Detailed instructions for using hello Twilio client library for Java are available at [How tooMake a Phone Call Using Twilio in a Java Application on Azure][howto_phonecall_java].</span></span>

## <span data-ttu-id="05614-159"><a id="configure_app"></a>Konfigurera ditt program tooUse Twilio-bibliotek</span><span class="sxs-lookup"><span data-stu-id="05614-159"><a id="configure_app"></a>Configure Your Application tooUse Twilio Libraries</span></span>
<span data-ttu-id="05614-160">I din kod kan du lägga till **importera** instruktioner överst hello i källfilerna för hello Twilio paket eller klasser du vill toouse i ditt program.</span><span class="sxs-lookup"><span data-stu-id="05614-160">Within your code, you can add **import** statements at hello top of your source files for hello Twilio packages or classes you want toouse in your application.</span></span>

<span data-ttu-id="05614-161">För Java-källfiler:</span><span class="sxs-lookup"><span data-stu-id="05614-161">For Java source files:</span></span>

```java
    import com.twilio.*;
    import com.twilio.rest.api.*;
    import com.twilio.type.*;
    import com.twilio.twiml.*;
```

<span data-ttu-id="05614-162">Java-sida (JSP) källfiler:</span><span class="sxs-lookup"><span data-stu-id="05614-162">For Java Server Page (JSP) source files:</span></span>

```java
    import="com.twilio.*"
    import="com.twilio.rest.api.*"
    import="com.twilio.type.*"
    import="com.twilio.twiml.*"
 ```
 
<span data-ttu-id="05614-163">Beroende på vilka Twilio paket eller klasser du vill toouse, din **importera** instruktioner kan vara olika.</span><span class="sxs-lookup"><span data-stu-id="05614-163">Depending on which Twilio packages or classes you want toouse, your **import** statements may be different.</span></span>

## <span data-ttu-id="05614-164"><a id="howto_make_call"></a>Så här: göra en utgående anrop</span><span class="sxs-lookup"><span data-stu-id="05614-164"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="05614-165">hello nedan visar hur en utgående toomake Ring upp med hello **anropa** klass.</span><span class="sxs-lookup"><span data-stu-id="05614-165">hello following shows how toomake an outgoing call using hello **Call** class.</span></span> <span data-ttu-id="05614-166">Den här koden används även en Twilio-tillhandahållna platsen tooreturn hello Twilio Markup Language (TwiML) svar.</span><span class="sxs-lookup"><span data-stu-id="05614-166">This code also uses a Twilio-provided site tooreturn hello Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="05614-167">Ersätt värdena för hello **från** och **till** telefonnummer och se till att du kontrollerar hello **från** telefonnummer för Twilio-konto tidigare toorunning hello koden.</span><span class="sxs-lookup"><span data-stu-id="05614-167">Substitute your values for hello **from** and **to** phone numbers, and ensure that you verify hello **from** phone number for your Twilio account prior toorunning hello code.</span></span>

```java
    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize hello Twilio client.
    Twilio.init(accountSID, authToken);

    // Use hello Twilio-provided site for hello TwiML response.
    URI uri = new URI("http://twimlets.com/message" +
            "?Message%5B0%5D=Hello%20World%21");

    // Declare tooand From numbers
    PhoneNumber too= new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");

    // Create a Call creator passing From, tooand URL values
    // then make hello call by executing hello create() method
    Call.creator(to, from, uri).create();
```

<span data-ttu-id="05614-168">Mer information om hello parametrar i toohello **Call.creator** -metoden finns [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span><span class="sxs-lookup"><span data-stu-id="05614-168">For more information about hello parameters passed in toohello **Call.creator** method, see [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span></span>

<span data-ttu-id="05614-169">Som tidigare nämnts kan använder den här koden en Twilio-tillhandahållna platsen tooreturn hello TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="05614-169">As mentioned, this code uses a Twilio-provided site tooreturn hello TwiML response.</span></span> <span data-ttu-id="05614-170">Du kan i stället använda din egen webbplats tooprovide hello TwiML svar; Mer information finns i [hur tooProvide TwiML svar i ett Java-program på Azure](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="05614-170">You could instead use your own site tooprovide hello TwiML response; for more information, see [How tooProvide TwiML Responses in a Java Application on Azure](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="05614-171"><a id="howto_send_sms"></a>Så här: skicka ett SMS-meddelande</span><span class="sxs-lookup"><span data-stu-id="05614-171"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="05614-172">hello nedan visar hur en SMS-meddelande med toosend hello **meddelandet** klass.</span><span class="sxs-lookup"><span data-stu-id="05614-172">hello following shows how toosend an SMS message using hello **Message** class.</span></span> <span data-ttu-id="05614-173">Hej **från** nummer, **4155992671**, tillhandahålls av Twilio för utvärderingsversionen konton toosend SMS-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="05614-173">hello **from** number, **4155992671**, is provided by Twilio for trial accounts toosend SMS messages.</span></span> <span data-ttu-id="05614-174">Hej **till** numret måste verifieras för Twilio-konto tidigare toorunning hello koden.</span><span class="sxs-lookup"><span data-stu-id="05614-174">hello **to** number must be verified for your Twilio account prior toorunning hello code.</span></span>

```java
    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize hello Twilio client.
    Twilio.init(accountSID, authToken);

    // Declare tooand From numbers and hello Body of hello SMS message
    PhoneNumber too= new PhoneNumber("+14159352345"); // Replace with a valid phone number for your account.
    PhoneNumber from = new PhoneNumber("+14158141829"); // Replace with a valid phone number for your account.
    String body = "Where's Wallace?";

    // Create a Message creator passing From, tooand Body values
    // then send hello SMS message by calling hello create() method
    Message sms = Message.creator(to, from, body).create();
```

<span data-ttu-id="05614-175">Mer information om hello parametrar i toohello **Message.creator** -metoden finns [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].</span><span class="sxs-lookup"><span data-stu-id="05614-175">For more information about hello parameters passed in toohello **Message.creator** method, see [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].</span></span>

## <span data-ttu-id="05614-176"><a id="howto_provide_twiml_responses"></a>Så här: Ange TwiML svar från din egen webbplats</span><span class="sxs-lookup"><span data-stu-id="05614-176"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="05614-177">När ditt program initierar anropet toohello Twilio-API, till exempel via hello **CallCreator.create** metoden Twilio att skicka din begäran tooa URL som är förväntade tooreturn ett TwiML svar.</span><span class="sxs-lookup"><span data-stu-id="05614-177">When your application initiates a call toohello Twilio API, for example via hello **CallCreator.create** method, Twilio will send your request tooa URL that is expected tooreturn a TwiML response.</span></span> <span data-ttu-id="05614-178">hello-exemplet ovan använder hello Twilio-tillhandahållna URL [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="05614-178">hello example above uses hello Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="05614-179">(När TwiML är avsedd för användning av webbtjänster, du kan visa hello TwiML i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="05614-179">(While TwiML is designed for use by Web services, you can view hello TwiML in your browser.</span></span> <span data-ttu-id="05614-180">Klicka till exempel [http://twimlets.com/message] [ twimlet_message_url] toosee en tom  **&lt;svar&gt;**  element; ett annat exempel är att klicka på [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21] [ twimlet_message_url_hello_world] toosee en  **&lt;svar&gt;**  element som innehåller en  **&lt;Säg &gt;**  element.)</span><span class="sxs-lookup"><span data-stu-id="05614-180">For example, click [http://twimlets.com/message][twimlet_message_url] toosee an empty **&lt;Response&gt;** element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21][twimlet_message_url_hello_world] toosee a **&lt;Response&gt;** element that contains a **&lt;Say&gt;** element.)</span></span>

<span data-ttu-id="05614-181">I stället för på hello angivna Twilio-URL, kan du skapa din egen URL: en webbplats som returnerar HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="05614-181">Instead of relying on hello Twilio-provided URL, you can create your own URL site that returns HTTP responses.</span></span> <span data-ttu-id="05614-182">Du kan skapa hello webbplats på alla språk som returnerar HTTP-svar Det här avsnittet förutsätter att du ska vara värd för hello URL i en JSP-sida.</span><span class="sxs-lookup"><span data-stu-id="05614-182">You can create hello site in any language that returns HTTP responses; this topic assumes you'll be hosting hello URL in a JSP page.</span></span>

<span data-ttu-id="05614-183">Hej följande JSP sidan resulterar i ett TwiML-svar som säger **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="05614-183">hello following JSP page results in a TwiML response that says **Hello World!**</span></span> <span data-ttu-id="05614-184">i hello anropet.</span><span class="sxs-lookup"><span data-stu-id="05614-184">on hello call.</span></span>

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

<span data-ttu-id="05614-185">hello följande JSP sidan resulterar i ett TwiML-svar som säger lite text har flera pausar och anger information om hello Twilio API-versionen och hello Azure rollnamn.</span><span class="sxs-lookup"><span data-stu-id="05614-185">hello following JSP page results in a TwiML response that says some text, has several pauses, and says information about hello Twilio API version and hello Azure role name.</span></span>

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello from Azure!</Say>
        <Pause></Pause>
        <Say>hello Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>hello Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>
```

<span data-ttu-id="05614-186">Hej **ApiVersion** parametern är tillgängliga i Twilio röst begäranden (SMS begäranden inte).</span><span class="sxs-lookup"><span data-stu-id="05614-186">hello **ApiVersion** parameter is available in Twilio voice requests (not SMS requests).</span></span> <span data-ttu-id="05614-187">toosee hello tillgängliga parametrarna för Twilio röst- och SMS-begäranden, se <https://www.twilio.com/docs/api/twiml/twilio_request> och <https://www.twilio.com/docs/api/twiml/sms/twilio_request >respektive.</span><span class="sxs-lookup"><span data-stu-id="05614-187">toosee hello available request parameters for Twilio voice and SMS requests, see <https://www.twilio.com/docs/api/twiml/twilio_request> and <https://www.twilio.com/docs/api/twiml/sms/twilio_request>, respectively.</span></span> <span data-ttu-id="05614-188">Hej **RoleName** miljövariabeln är tillgänglig som en del av Azure-distribution.</span><span class="sxs-lookup"><span data-stu-id="05614-188">hello **RoleName** environment variable is available as part of an Azure deployment.</span></span> <span data-ttu-id="05614-189">(Om du vill tooadd anpassade miljövariabler så att de kan öppnas från **System.getenv**, i avsnittet hello miljö variabler vid [diverse konfigurationsinställningar för rollen] [misc_role_config_settings].)</span><span class="sxs-lookup"><span data-stu-id="05614-189">(If you want tooadd custom environment variables so they could be picked up from **System.getenv**, see hello environment variables section at [Miscellaneous Role Configuration Settings][misc_role_config_settings].)</span></span>

<span data-ttu-id="05614-190">När du har JSP sidan Konfigurera tooprovide TwiML svar använder hello URL till hello JSP sida som hello URL som skickades till hello **Call.creator** metod.</span><span class="sxs-lookup"><span data-stu-id="05614-190">Once you have your JSP page set up tooprovide TwiML responses, use hello URL of hello JSP page as hello URL passed into hello **Call.creator** method.</span></span> <span data-ttu-id="05614-191">Till exempel om du har ett webbprogram med namnet MyTwiML distribueras tooan Azure värdbaserade tjänsten, och hello hello JSP sidan heter mytwiml.jsp, hello URL kan skickas för**Call.creator** enligt hello följande:</span><span class="sxs-lookup"><span data-stu-id="05614-191">For example, if you have a Web application named MyTwiML deployed tooan Azure hosted service, and hello name of hello JSP page is mytwiml.jsp, hello URL can be passed too**Call.creator** as shown in hello following:</span></span>

```java
    // Declare tooand From numbers and hello URL of your JSP page
    PhoneNumber too= new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, tooand URL values
    // then make hello call by executing hello create() method
    Call.creator(to, from, uri).create();
```

<span data-ttu-id="05614-192">Ett annat alternativ för att svara med TwiML är via hello **VoiceResponse** klassen, som är tillgängliga i hello **com.twilio.twiml** paketet.</span><span class="sxs-lookup"><span data-stu-id="05614-192">Another option for responding with TwiML is via hello **VoiceResponse** class, which is available in hello **com.twilio.twiml** package.</span></span>

<span data-ttu-id="05614-193">Mer information om hur du använder Twilio i Azure med Java finns [hur tooMake ett telefonsamtal med Twilio i ett Java-program på Azure][howto_phonecall_java].</span><span class="sxs-lookup"><span data-stu-id="05614-193">For additional information about using Twilio in Azure with Java, see [How tooMake a Phone Call Using Twilio in a Java Application on Azure][howto_phonecall_java].</span></span>

## <span data-ttu-id="05614-194"><a id="AdditionalServices"></a>Så här: använda ytterligare Twilio-tjänster</span><span class="sxs-lookup"><span data-stu-id="05614-194"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="05614-195">Dessutom kan toohello exemplen nedan, Twilio erbjuder webbaserade API: er som du använda tooleverage ytterligare Twilio-funktioner från din Azure-program.</span><span class="sxs-lookup"><span data-stu-id="05614-195">In addition toohello examples shown here, Twilio offers web-based APIs that you can use tooleverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="05614-196">Fullständig information finns i hello [Twilio-API-dokumentationen][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="05614-196">For full details, see hello [Twilio API documentation][twilio_api_documentation].</span></span>

## <span data-ttu-id="05614-197"><a id="NextSteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="05614-197"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="05614-198">Nu när du har lärt dig hello grunderna i hello Twilio-tjänsten ska du följa dessa länkar toolearn mer:</span><span class="sxs-lookup"><span data-stu-id="05614-198">Now that you've learned hello basics of hello Twilio service, follow these links toolearn more:</span></span>

* <span data-ttu-id="05614-199">[Riktlinjer för Twilio-säkerhet][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="05614-199">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="05614-200">[Twilio ta och exempelkod][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="05614-200">[Twilio HowTo's and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="05614-201">[Twilio snabbstarten självstudier][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="05614-201">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span>
* <span data-ttu-id="05614-202">[Twilio på GitHub][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="05614-202">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="05614-203">[Kontakta tooTwilio Support][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="05614-203">[Talk tooTwilio Support][twilio_support]</span></span>

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World%21
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/docs
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
