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
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-java"></a>Hur tooUse Twilio för röst- och SMS-funktioner i Java
Den här guiden visar hur tooperform vanliga programmeringsuppgifter med hello Twilio API-tjänsten på Azure. hello-scenarier som tas upp inkluderar att ringa och ett kort meddelande (SMS Service)-meddelande skickas. Mer information om Twilio och använder röst- och SMS i dina program finns hello [nästa steg](#NextSteps) avsnitt.

## <a id="WhatIs"></a>Vad är Twilio?
Twilio är en telefoni webbtjänst API som kan du använda din befintliga web språk och kunskaper toobuild röst och SMS-program. Twilio är en från tredje part (inte en funktion i Azure och inte en Microsoft-produkt).

**Twilio röst** kan ditt program toomake och ta emot telefonsamtal. **Twilio SMS** kan ditt program toomake och ta emot SMS-meddelanden. **Twilio klienten** gör att dina program tooenable röstkommunikation via befintliga Internet-anslutningar, inklusive mobila anslutningar.

## <a id="Pricing"></a>Priser för Twilio och specialerbjudanden
Information om priser Twilio finns på [Twilio priser][twilio_pricing]. Azure-kunder får en [specialerbjudande][special_offer]: en kredit på 1000 texter eller 1000 inkommande minuter. toosign för detta erbjudande eller få mer information, besök [http://ahoy.twilio.com/azure][special_offer].

## <a id="Concepts"></a>Begrepp
Hej Twilio API är en RESTful-API som tillhandahåller röst- och SMS-funktioner för program. Klientbibliotek är tillgängliga på flera språk. en lista, se [Twilio-API-bibliotek][twilio_libraries].

Viktiga aspekter av hello Twilio API är Twilio verb och Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-verb
hello API väljer Twilio verb; till exempel hello  **&lt;säg&gt;**  verb instruerar Twilio tooaudibly leverera ett meddelande på ett samtal.

hello följer en lista över Twilio verb.

* **&lt;Ring&gt;**: ansluter hello anroparen tooanother phone.
* **&lt;Samla in&gt;**: samlar in siffror som anges på hello telefon tangentbordet.
* **&lt;Koppla ned&gt;**: slutar ett anrop.
* **&lt;Spela upp&gt;**: spelar en ljudfil.
* **&lt;Kön&gt;**: Lägg till hello tooa kö med anropare.
* **&lt;Pausa&gt;**: tyst väntar på ett angivet antal sekunder.
* **&lt;Posten&gt;**: registrerar hello anroparen röst och returnerar en URL för en fil som innehåller hello registrering.
* **&lt;Omdirigera&gt;**: Överför kontroll över ett samtal eller SMS toohello TwiML på en annan URL.
* **&lt;Avvisa&gt;**: avvisar en inkommande anropa tooyour Twilio tal utan fakturering du.
* **&lt;Säg&gt;**: konverterar text toospeech som görs på ett samtal.
* **&lt;SMS&gt;**: skickar ett SMS-meddelande.

### <a id="TwiML"></a>TwiML
TwiML är en uppsättning XML-baserade instruktioner baserat på hello Twilio verb som informerar Twilio på hur tooprocess ett samtal eller SMS.

Exempelvis skulle hello följande TwiML omvandla hello text **Hello World!** toospeech.

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

När programmet-anrop hello Twilio-API, är en av parametrarna för hello API hello URL som returnerar hello TwiML svar. För utveckling, kan du använda Twilio-tillhandahållna URL: er tooprovide hello TwiML svar som används av dina program. Du kan också vara värd för dina egna URL: er tooproduce hello TwiML svar och ett annat alternativ är toouse hello **TwiMLResponse** objekt.

Läs mer om Twilio verb, deras attribut och TwiML [TwiML][twiml]. Mer information om hello Twilio-API finns [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Skapa ett Twilio-konto
När du är klar tooget Twilio-konto kan logga på [försök Twilio][try_twilio]. Du kan börja med ett kostnadsfritt konto och uppgradera ditt konto senare.

När du registrerar dig för ett Twilio-konto får du ett konto-ID och ett token för autentisering. Båda ska vara nödvändiga toomake Twilio API-anrop. tooprevent obehörig åtkomst till tooyour konto, skydda din token för autentisering. Ditt konto-ID och autentisering token kan visas på hello [Twilio konsolen][twilio_console]i hello fält med namnet **konto SID** och **AUTH TOKEN**respektive.

## <a id="create_app"></a>Skapa ett Java-program
1. Hämta hello Twilio JAR och Lägg den tooyour Java skapa sökvägen och WAR-distribution för sammansättningen. Vid [https://github.com/twilio/twilio-java][twilio_java], kan du hämta hello GitHub källor och skapa egna JAR eller hämta en förskapad JAR (med eller utan beroenden).
2. Se till att din JDK **cacerts** keystore innehåller hello Equifax certifikatutfärdare för säkra-certifikat med MD5 fingeravtryck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello serienummer är 35:DE:F4:CF och hello SHA1 fingeravtryck är D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Det här är hello certifikat certifikatutfärdarens certifikat för hello [https://api.twilio.com] [ twilio_api_service] -tjänsten som anropas när du använder Twilio APIs. Information om att säkerställa ditt JDK **cacerts** keystore innehåller hello rätt CA-certifikat, se [att lägga till ett certifikat toohello Java CA-certifikatarkivet][add_ca_cert].

Detaljerade anvisningar för att använda hello Twilio-klientbibliotek för Java finns på [hur tooMake ett telefonsamtal med Twilio i ett Java-program på Azure][howto_phonecall_java].

## <a id="configure_app"></a>Konfigurera ditt program tooUse Twilio-bibliotek
I din kod kan du lägga till **importera** instruktioner överst hello i källfilerna för hello Twilio paket eller klasser du vill toouse i ditt program.

För Java-källfiler:

```java
    import com.twilio.*;
    import com.twilio.rest.api.*;
    import com.twilio.type.*;
    import com.twilio.twiml.*;
```

Java-sida (JSP) källfiler:

```java
    import="com.twilio.*"
    import="com.twilio.rest.api.*"
    import="com.twilio.type.*"
    import="com.twilio.twiml.*"
 ```
 
Beroende på vilka Twilio paket eller klasser du vill toouse, din **importera** instruktioner kan vara olika.

## <a id="howto_make_call"></a>Så här: göra en utgående anrop
hello nedan visar hur en utgående toomake Ring upp med hello **anropa** klass. Den här koden används även en Twilio-tillhandahållna platsen tooreturn hello Twilio Markup Language (TwiML) svar. Ersätt värdena för hello **från** och **till** telefonnummer och se till att du kontrollerar hello **från** telefonnummer för Twilio-konto tidigare toorunning hello koden.

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

Mer information om hello parametrar i toohello **Call.creator** -metoden finns [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Som tidigare nämnts kan använder den här koden en Twilio-tillhandahållna platsen tooreturn hello TwiML svar. Du kan i stället använda din egen webbplats tooprovide hello TwiML svar; Mer information finns i [hur tooProvide TwiML svar i ett Java-program på Azure](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Så här: skicka ett SMS-meddelande
hello nedan visar hur en SMS-meddelande med toosend hello **meddelandet** klass. Hej **från** nummer, **4155992671**, tillhandahålls av Twilio för utvärderingsversionen konton toosend SMS-meddelanden. Hej **till** numret måste verifieras för Twilio-konto tidigare toorunning hello koden.

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

Mer information om hello parametrar i toohello **Message.creator** -metoden finns [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Så här: Ange TwiML svar från din egen webbplats
När ditt program initierar anropet toohello Twilio-API, till exempel via hello **CallCreator.create** metoden Twilio att skicka din begäran tooa URL som är förväntade tooreturn ett TwiML svar. hello-exemplet ovan använder hello Twilio-tillhandahållna URL [http://twimlets.com/message][twimlet_message_url]. (När TwiML är avsedd för användning av webbtjänster, du kan visa hello TwiML i webbläsaren. Klicka till exempel [http://twimlets.com/message] [ twimlet_message_url] toosee en tom  **&lt;svar&gt;**  element; ett annat exempel är att klicka på [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21] [ twimlet_message_url_hello_world] toosee en  **&lt;svar&gt;**  element som innehåller en  **&lt;Säg &gt;**  element.)

I stället för på hello angivna Twilio-URL, kan du skapa din egen URL: en webbplats som returnerar HTTP-svar. Du kan skapa hello webbplats på alla språk som returnerar HTTP-svar Det här avsnittet förutsätter att du ska vara värd för hello URL i en JSP-sida.

Hej följande JSP sidan resulterar i ett TwiML-svar som säger **Hello World!** i hello anropet.

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

hello följande JSP sidan resulterar i ett TwiML-svar som säger lite text har flera pausar och anger information om hello Twilio API-versionen och hello Azure rollnamn.

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

Hej **ApiVersion** parametern är tillgängliga i Twilio röst begäranden (SMS begäranden inte). toosee hello tillgängliga parametrarna för Twilio röst- och SMS-begäranden, se <https://www.twilio.com/docs/api/twiml/twilio_request> och <https://www.twilio.com/docs/api/twiml/sms/twilio_request >respektive. Hej **RoleName** miljövariabeln är tillgänglig som en del av Azure-distribution. (Om du vill tooadd anpassade miljövariabler så att de kan öppnas från **System.getenv**, i avsnittet hello miljö variabler vid [diverse konfigurationsinställningar för rollen] [misc_role_config_settings].)

När du har JSP sidan Konfigurera tooprovide TwiML svar använder hello URL till hello JSP sida som hello URL som skickades till hello **Call.creator** metod. Till exempel om du har ett webbprogram med namnet MyTwiML distribueras tooan Azure värdbaserade tjänsten, och hello hello JSP sidan heter mytwiml.jsp, hello URL kan skickas för**Call.creator** enligt hello följande:

```java
    // Declare tooand From numbers and hello URL of your JSP page
    PhoneNumber too= new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, tooand URL values
    // then make hello call by executing hello create() method
    Call.creator(to, from, uri).create();
```

Ett annat alternativ för att svara med TwiML är via hello **VoiceResponse** klassen, som är tillgängliga i hello **com.twilio.twiml** paketet.

Mer information om hur du använder Twilio i Azure med Java finns [hur tooMake ett telefonsamtal med Twilio i ett Java-program på Azure][howto_phonecall_java].

## <a id="AdditionalServices"></a>Så här: använda ytterligare Twilio-tjänster
Dessutom kan toohello exemplen nedan, Twilio erbjuder webbaserade API: er som du använda tooleverage ytterligare Twilio-funktioner från din Azure-program. Fullständig information finns i hello [Twilio-API-dokumentationen][twilio_api_documentation].

## <a id="NextSteps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i hello Twilio-tjänsten ska du följa dessa länkar toolearn mer:

* [Riktlinjer för Twilio-säkerhet][twilio_security_guidelines]
* [Twilio ta och exempelkod][twilio_howtos]
* [Twilio snabbstarten självstudier][twilio_quickstarts]
* [Twilio på GitHub][twilio_on_github]
* [Kontakta tooTwilio Support][twilio_support]

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
