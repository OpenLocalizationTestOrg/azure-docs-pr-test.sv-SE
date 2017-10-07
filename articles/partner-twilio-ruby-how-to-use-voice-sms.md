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
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Hur tooUse Twilio för röst- och SMS-funktioner i Ruby
Den här guiden visar hur tooperform vanliga programmeringsuppgifter med hello Twilio API-tjänsten på Azure. hello-scenarier som tas upp inkluderar att ringa och ett kort meddelande (SMS Service)-meddelande skickas. Mer information om Twilio och använder röst- och SMS i dina program finns hello [nästa steg](#NextSteps) avsnitt.

## <a id="WhatIs"></a>Vad är Twilio?
Twilio är en telefoni webbtjänst API som kan du använda din befintliga web språk och kunskaper toobuild röst och SMS-program. Twilio är en från tredje part (inte en funktion i Azure och inte en Microsoft-produkt).

**Twilio röst** kan ditt program toomake och ta emot telefonsamtal. **Twilio SMS** kan ditt program toomake och ta emot SMS-meddelanden. **Twilio klienten** gör att dina program tooenable röstkommunikation via befintliga Internet-anslutningar, inklusive mobila anslutningar.

## <a id="Pricing"></a>Priser för Twilio och specialerbjudanden
Information om priser Twilio finns på [Twilio priser][twilio_pricing]. Azure-kunder får en [specialerbjudande][special_offer]: en kredit på 1000 texter eller 1000 inkommande minuter. toosign för detta erbjudande eller få mer information, besök [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Begrepp
Hej Twilio API är en RESTful-API som tillhandahåller röst- och SMS-funktioner för program. Klientbibliotek är tillgängliga på flera språk. en lista, se [Twilio-API-bibliotek][twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML är en uppsättning XML-baserade instruktioner som talar om för Twilio på hur tooprocess ett samtal eller SMS.

Exempelvis skulle hello följande TwiML omvandla hello text **Hello World** toospeech.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Alla TwiML dokument har `<Response>` som deras rotelementet. Därifrån kan använda du Twilio verb toodefine hello beteendet för ditt program.

### <a id="Verbs"></a>TwiML verb
Twilio-verb är XML-taggar som talar om Twilio vad för**gör**. Till exempel hello  **&lt;säg&gt;**  verb instruerar Twilio tooaudibly leverera ett meddelande på ett samtal. 

hello följer en lista över Twilio verb.

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

Läs mer om Twilio verb, deras attribut och TwiML [TwiML][twiml]. Mer information om hello Twilio-API finns [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Skapa ett Twilio-konto
När du är klar tooget Twilio-konto kan logga på [försök Twilio][try_twilio]. Du kan börja med ett kostnadsfritt konto och uppgradera ditt konto senare.

När du registrerar dig för ett Twilio-konto får du ett kostnadsfritt telefonnummer för ditt program. Du får även ett SID-konto och en token för autentisering. Båda ska vara nödvändiga toomake Twilio API-anrop. tooprevent obehörig åtkomst till tooyour konto, skydda din token för autentisering. Ditt konto SID och auth token kan visas på hello [Twilio kontosida][twilio_account]i hello fält med namnet **konto SID** och **AUTH TOKEN** , respektive.

### <a id="VerifyPhoneNumbers"></a>Kontrollera telefonnummer
I tillägg toohello tal som du får från Twilio, kan du också kontrollera siffror som du hanterar (d.v.s. din mobiltelefon eller home telefonnumret) för användning i dina program. 

Mer information om hur tooverify ett telefonnummer finns [hantera siffror][verify_phone].

## <a id="create_app"></a>Skapa ett Ruby-program
Ett Ruby-program som använder hello Twilio-tjänst och körs i Azure är inte annorlunda än andra Ruby program som använder hello Twilio-tjänsten. Medan Twilio tjänsterna RESTful och kan anropas från Ruby på flera sätt, den här artikeln fokuserar på hur toouse Twilio tjänster med [Twilio hjälpbibliotek för Ruby][twilio_ruby].

Första, [ställa in en ny Azure Linux VM] [ azure_vm_setup] tooact som värd för ditt nya Ruby webbprogram. Ignorera hello steg som involverar hello skapandet av en spår app bara ställa in hello VM. Kontrollera att du skapar en slutpunkt med en extern port 80 och en intern port 5000.

I hello exemplen nedan, kommer vi att använda [Sinatra][sinatra], ett väldigt enkelt webbramverk för Ruby. Men du kan visserligen använda hello Twilio hjälpbibliotek för Ruby med alla andra webbramverk, inklusive Ruby spår.

SSH till den nya virtuella datorn och skapar en katalog för din nya app. Skapa en fil med namnet Gemfile och kopiera hello följande kod till den i katalogen:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

På kommandoraden för hello kör `bundle install`. Detta installerar hello ovanstående beroenden. Sedan skapar en fil med namnet `web.rb`. Det här är där hello-koden för ditt webbprogram finns. Klistra in följande kod till den hello:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Nu bör du kunna hello kör hello kommandot `ruby web.rb -p 5000`. Detta kommer rotationsrutan upp en liten webbserver på port 5000. Du bör vara kan toobrowse toothis app i webbläsaren genom att besöka hello URL: en inställning för din virtuella Azure-datorn. När du kan komma åt ditt webbprogram i hello webbläsare, är du redo toostart bygga en Twilio-app.

## <a id="configure_app"></a>Konfigurera ditt program tooUse Twilio
Du kan konfigurera biblioteket web app toouse hello Twilio genom att uppdatera din `Gemfile` tooinclude raden:

    gem 'twilio-ruby'

Kör på kommandoraden hello `bundle install`. Öppna `web.rb` och den här raden hello överst, inklusive:

    require 'twilio-ruby'

Du är nu allt klart toouse hello Twilio hjälpbibliotek för Ruby i ditt webbprogram.

## <a id="howto_make_call"></a>Så här: göra en utgående anrop
hello följande visar hur en utgående toomake anropa. Viktiga begrepp omfattar med hello Twilio hjälpbibliotek för Ruby toomake REST API-anrop och återges TwiML. Ersätt värdena för hello **från** och **till** telefonnummer och se till att du kontrollerar hello **från** telefonnummer för Twilio-konto tidigare toorunning hello koden.

Lägg till den här funktionen för`web.md`:

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

Om du öppna upp `http://yourdomain.cloudapp.net/make_call` i en webbläsare som utlöser hello anropet toohello Twilio API toomake hello telefonsamtal. Hej först två parametrar i `client.account.calls.create` är ganska självförklarande: hello nummer hello anropet är `from` och hello nummer hello anropet är `to`. 

Hej tredje parametern (`url`) är hello-URL att Twilio begär tooget instruktioner om vilken toodo när hello anrop är ansluten. I det här fallet vi ställa in en URL (`http://yourdomain.cloudapp.net`) som returnerar ett enkelt TwiML dokument och använder hello `<Say>` verb toodo vissa text till tal- och säg ”Hello apa” toohello personen som tar emot hello anropa.

## <a id="howto_recieve_sms"></a>Så här: ta emot ett SMS-meddelande
I föregående exempel för hello vi initierade en **utgående** telefonsamtal. Den här gången, ska vi använda hello telefonnumret som Twilio angav under registreringen tooprocess en **inkommande** SMS-meddelande.

Första, logga in på tooyour [Twilio instrumentpanelen][twilio_account]. Klicka på ”siffror” i hello översta nav och klicka sedan på hello Twilio-nummer som du har angett. Ser du två URL: er som du kan konfigurera. En röst begärd URL och ett SMS URL-begäran. Dessa är hello URL: er som görs Twilio anrop när ett telefonsamtal eller ett SMS skickas tooyour nummer. hello URL: er kallas även ”web hook”.

Vi vill gärna tooprocess inkommande SMS-meddelanden, så vi uppdatera hello URL för`http://yourdomain.cloudapp.net/sms_url`. Gå vidare och klicka på Spara ändringar på hello hello sidans nederkant. Nu tillbaka `web.rb` vi programmet våra program toohandle detta:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for hello ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Efter att hello ändringen, se till att toore-start ditt webbprogram. Nu ta ut din telefon och skicka ett SMS tooyour Twilio nummer. Du får omedelbart ett SMS-svar som säger ”Hey, Tack för hello ping! Twilio och Azure Berg ”!.

## <a id="additional_services"></a>Så här: använda ytterligare Twilio-tjänster
Dessutom kan toohello exemplen nedan, Twilio erbjuder webbaserade API: er som du använda tooleverage ytterligare Twilio-funktioner från din Azure-program. Fullständig information finns i hello [Twilio-API-dokumentationen][twilio_api_documentation].

### <a id="NextSteps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i hello Twilio-tjänsten ska du följa dessa länkar toolearn mer:

* [Riktlinjer för Twilio-säkerhet][twilio_security_guidelines]
* [Twilio HowTos och exempelkod][twilio_howtos]
* [Twilio snabbstarten självstudier][twilio_quickstarts] 
* [Twilio på GitHub][twilio_on_github]
* [Kontakta tooTwilio Support][twilio_support]

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
