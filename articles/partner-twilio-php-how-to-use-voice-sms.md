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
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-php"></a>Hur tooUse Twilio för röst- och SMS-funktionerna i PHP
Den här guiden visar hur tooperform vanliga programmeringsuppgifter med hello Twilio API-tjänsten på Azure. hello-scenarier som tas upp inkluderar att ringa och ett kort meddelande (SMS Service)-meddelande skickas. Mer information om Twilio och använder röst- och SMS i dina program finns hello [nästa steg](#NextSteps) avsnitt.

## <a id="WhatIs"></a>Vad är Twilio?
Twilio startar hello framtiden för kommunikation, aktivera utvecklare tooembed röst, VoIP, och meddelanden i program. De virtualisera alla infrastruktur som behövs i en molnbaserad, globala exponera den via hello Twilio kommunikation API-plattformen. Programmen är enkla toobuild och skalbara. Få flexibilitet med lön-som-dig gå priser och dra nytta av molnet tillförlitlighet.

**Twilio röst** kan ditt program toomake och ta emot telefonsamtal. **Twilio SMS** gör att din ansökan toosend och ta emot textmeddelanden. **Twilio klienten** tillåter du toomake VoIP-anrop från telefon, surfplatta eller webbläsare och stöder WebRTC.

## <a id="Pricing"></a>Priser för Twilio och specialerbjudanden
Azure-kunder får en [specialerbjudande](http://www.twilio.com/azure): kostnadsfri $10 Twilio kredit när du uppgraderar ditt Twilio-konto. Den här Twilio-kredit kan vara tillämpade tooany Twilio användning ($10 kredit motsvarande toosending upp till 1 000 SMS-meddelanden eller ta emot in too1000 inkommande röst minuter beroende på hello platsen för ditt mål för telefon och meddelandet eller samtal). Lösa in den här Twilio-kredit och kom igång på: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).

Twilio är en betalning per användning. Det finns inga avgifter för installation och du kan stänga ditt konto när som helst. Du hittar mer information i [Twilio priser][twilio_pricing].

## <a id="Concepts"></a>Begrepp
Hej Twilio API är en RESTful-API som tillhandahåller röst- och SMS-funktioner för program. Klientbibliotek är tillgängliga på flera språk. en lista, se [Twilio-API-bibliotek][twilio_libraries].

Viktiga aspekter av hello Twilio API är Twilio verb och Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-verb
hello API väljer Twilio verb; till exempel hello  **&lt;säg&gt;**  verb instruerar Twilio tooaudibly leverera ett meddelande på ett samtal.

hello följer en lista över Twilio verb. Lär dig mer om hello andra verb och funktioner via [Markup Language Twilio dokumentationen](http://www.twilio.com/docs/api/twiml).

* **&lt;Ring&gt;**: ansluter hello anroparen tooanother phone.
* **&lt;Samla in&gt;**: samlar in siffror som anges på hello telefon tangentbordet.
* **&lt;Koppla ned&gt;**: slutar ett anrop.
* **&lt;Spela upp&gt;**: spelar en ljudfil.
* **&lt;Pausa&gt;**: tyst väntar på ett angivet antal sekunder.
* **&lt;Posten&gt;**: registrerar hello anroparen röst och returnerar en URL för en fil som innehåller hello registrering.
* **&lt;Omdirigera&gt;**: Överför kontroll över ett samtal eller SMS toohello TwiML på en annan URL.
* **&lt;Avvisa&gt;**: avvisar en inkommande anropa tooyour Twilio tal utan fakturering du
* **&lt;Säg&gt;**: konverterar text toospeech som görs på ett samtal.
* **&lt;SMS&gt;**: skickar ett SMS-meddelande.

### <a id="TwiML"></a>TwiML
TwiML är en uppsättning XML-baserade instruktioner baserat på hello Twilio verb som informerar Twilio på hur tooprocess ett samtal eller SMS.

Exempelvis skulle hello följande TwiML omvandla hello text **Hello World** toospeech.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

När programmet-anrop hello Twilio-API, är en av parametrarna för hello API hello URL som returnerar hello TwiML svar. För utveckling, kan du använda Twilio-tillhandahållna URL: er tooprovide hello TwiML svar som används av dina program. Du kan också vara värd för dina egna URL: er tooproduce hello TwiML svar och ett annat alternativ är toouse hello **TwiMLResponse** objekt.

Läs mer om Twilio verb, deras attribut och TwiML [TwiML][twiml]. Mer information om hello Twilio-API finns [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Skapa ett Twilio-konto
När du är klar tooget Twilio-konto kan logga på [försök Twilio][try_twilio]. Du kan börja med ett kostnadsfritt konto och uppgradera ditt konto senare.

När du registrerar dig för ett Twilio-konto får du ett konto-ID och ett token för autentisering. Båda ska vara nödvändiga toomake Twilio API-anrop. tooprevent obehörig åtkomst till tooyour konto, skydda din token för autentisering. Ditt konto-ID och autentisering token kan visas på hello [Twilio kontosida][twilio_account]i hello fält med namnet **konto SID** och **AUTH TOKEN**respektive.

## <a id="create_app"></a>Skapa en PHP-program
Ett PHP-program som använder hello Twilio-tjänsten och körs i Azure är inte annorlunda än andra PHP-program som använder hello Twilio tjänst. När Twilio är REST-baserad och kan anropas från PHP på flera sätt, den här artikeln fokuserar på hur toouse Twilio tjänster med [Twilio-biblioteket för PHP från GitHub][twilio_php]. Mer information om hur du använder hello Twilio-biblioteket för PHP finns [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Detaljerade anvisningar för att skapa och distribuera en Twilio/PHP programmet tooAzure finns på [hur tooMake ett telefonsamtal med Twilio i ett PHP-program på Azure][howto_phonecall_php].

## <a id="configure_app"></a>Konfigurera ditt program tooUse Twilio-bibliotek
Du kan konfigurera biblioteket programmet toouse hello Twilio för PHP på två sätt:

1. Hämta hello Twilio-biblioteket för PHP från GitHub ([https://github.com/twilio/twilio-php][twilio_php]) och Lägg till hello **Services** directory tooyour-program.
   
    ELLER
2. Installera hello Twilio-biblioteket för PHP som PÄRONTRÄD paket. Den kan installeras med hello följande kommandon:
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

När du har installerat hello Twilio-biblioteket för PHP, du kan sedan lägga till en **require_once** instruktionen hello överst i din PHP filer tooreference hello bibliotek:

        require_once 'Services/Twilio.php';

Mer information finns i [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Så här: göra en utgående anrop
hello nedan visar hur en utgående toomake Ring upp med hello **Services_Twilio** klass. Den här koden används även en Twilio-tillhandahållna platsen tooreturn hello Twilio Markup Language (TwiML) svar. Ersätt värdena för hello **från** och **till** telefonnummer och se till att du kontrollerar hello **från** telefonnummer för Twilio-konto tidigare toorunning hello koden.

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

Som tidigare nämnts kan använder den här koden en Twilio-tillhandahållna platsen tooreturn hello TwiML svar. Du kan i stället använda din egen webbplats tooprovide hello TwiML svar; Mer information finns i [hur tooProvide TwiML svar från din egen webbplats](#howto_provide_twiml_responses).

* **Obs**: tootroubleshoot verifieringsfel för SSL-certifikat, se [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation] 

## <a id="howto_send_sms"></a>Så här: skicka ett SMS-meddelande
hello nedan visar hur en SMS-meddelande med toosend hello **Services_Twilio** klass. Hej **från** nummer tillhandahålls av Twilio för utvärderingsversionen konton toosend SMS-meddelanden. Hej **till** numret måste verifieras för Twilio-konto tidigare toorunning hello koden.

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

## <a id="howto_provide_twiml_responses"></a>Så här: Ange TwiML svar från din egen webbplats
När ditt program initierar en anropet toohello Twilio-API, skickar Twilio din begäran tooa URL-adress förväntades tooreturn TwiML svar. hello-exemplet ovan använder hello Twilio-tillhandahållna URL [http://twimlets.com/message][twimlet_message_url]. (När TwiML är avsedd för användning av Twilio, kan du visa hello i webbläsaren. Klicka till exempel [http://twimlets.com/message] [ twimlet_message_url] toosee en tom `<Response>` element, klicka på ett annat exempel är [http://twimlets.com/message? Meddelande % 5B0 %5, D = Hello % 20World] [ twimlet_message_url_hello_world] toosee en `<Response>` element som innehåller en `<Say>` element.)

I stället för på hello angivna Twilio-URL, kan du skapa din egen webbplats som returnerar HTTP-svar. Du kan skapa hello webbplats på alla språk som returnerar XML-svar; Det här avsnittet förutsätter att du använder PHP toocreate hello TwiML.

Hej följande PHP sidan resulterar i ett TwiML-svar som säger **Hello World** i hello-anropet.

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

Du kan se hello-exemplet ovan kan är hello TwiML svar bara ett XML-dokument. Hej Twilio-biblioteket för PHP innehåller klasser som ska generera TwiML för dig. hello exemplet nedan ger hello motsvarande svar som ovan, men använder hello **Services\_Twilio\_Twiml** klass i hello Twilio-biblioteket för PHP:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Läs mer om TwiML [https://www.twilio.com/docs/api/twiml][twiml_reference]. 

När du har PHP sidan Konfigurera tooprovide TwiML svar använder hello URL för hello PHP-sida som hello URL som skickades till hello `Services_Twilio->account->calls->create` metod. Till exempel om du har ett webbprogram med namnet **MyTwiML** distribuerade tooan Azure-värdtjänsten och hello hello PHP-sida heter **mytwiml.php**, hello URL kan skickas för **Services_ Twilio -> konto -> anrop -> Skapa** som visas i följande exempel hello:

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

Mer information om hur du använder Twilio i Azure med PHP finns [hur tooMake ett telefonsamtal med Twilio i ett PHP-program på Azure][howto_phonecall_php].

## <a id="AdditionalServices"></a>Så här: använda ytterligare Twilio-tjänster
Dessutom kan toohello exemplen nedan, Twilio erbjuder webbaserade API: er som du använda tooleverage ytterligare Twilio-funktioner från din Azure-program. Fullständig information finns i hello [Twilio-API-dokumentationen][twilio_api_documentation].

## <a id="NextSteps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i hello Twilio-tjänsten ska du följa dessa länkar toolearn mer:

* [Riktlinjer för Twilio-säkerhet][twilio_security_guidelines]
* [Twilio ta och exempelkod][twilio_howtos]
* [Twilio snabbstarten självstudier][twilio_quickstarts] 
* [Twilio på GitHub][twilio_on_github]
* [Kontakta tooTwilio Support][twilio_support]

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
