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
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-python"></a>Hur tooUse Twilio för röst- och SMS-funktioner i Python
Den här guiden visar hur tooperform vanliga programmeringsuppgifter med hello Twilio API-tjänsten på Azure. hello-scenarier som tas upp inkluderar att ringa och ett kort meddelande (SMS Service)-meddelande skickas. Mer information om Twilio och använder röst- och SMS i dina program finns hello [nästa steg](#NextSteps) avsnitt.

## <a id="WhatIs"></a>Vad är Twilio?
Twilio startar hello framtiden för kommunikation, aktivera utvecklare tooembed röst, VoIP, och meddelanden i program. De virtualisera alla infrastruktur som behövs i en molnbaserad, globala exponera den via hello Twilio kommunikation API-plattformen. Programmen är enkla toobuild och skalbara. Få flexibilitet med lön-som-dig gå priser och dra nytta av molnet tillförlitlighet.

**Twilio röst** kan ditt program toomake och ta emot telefonsamtal.
**Twilio SMS** gör att din ansökan toosend och ta emot textmeddelanden.
**Twilio klienten** tillåter du toomake VoIP-anrop från telefon, surfplatta eller webbläsare och stöder WebRTC.

## <a id="Pricing"></a>Priser för Twilio och specialerbjudanden
Azure-kunder får en [specialerbjudande] [ special_offer] $10 Twilio kredit när du uppgraderar ditt Twilio-konto. Den här Twilio-kredit kan vara tillämpade tooany Twilio användning ($10 kredit motsvarande toosending upp till 1 000 SMS-meddelanden eller ta emot in too1000 inkommande röst minuter beroende på hello platsen för ditt mål för telefon och meddelandet eller samtal). Lösa detta [Twilio kredit] [ special_offer] och komma igång.

Twilio är en betalning per användning. Det finns inga avgifter för installation och du kan stänga ditt konto när som helst. Du hittar mer information i [Twilio priser][twilio_pricing].

## <a id="Concepts"></a>Begrepp
Hej Twilio API är en RESTful-API som tillhandahåller röst- och SMS-funktioner för program. Klientbibliotek är tillgängliga på flera språk. en lista, se [Twilio-API-bibliotek][twilio_libraries].

Viktiga aspekter av hello Twilio API är Twilio verb och Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-verb
hello API väljer Twilio verb; till exempel hello  **&lt;säg&gt;**  verb instruerar Twilio tooaudibly leverera ett meddelande på ett samtal.

hello följer en lista över Twilio verb. Lär dig mer om hello andra verb och funktioner via [Markup Language Twilio dokumentationen][twiml].

* **&lt;Ring&gt;**: ansluter hello anroparen tooanother phone.
* **&lt;Samla in&gt;**: samlar in siffror som anges på hello telefon tangentbordet.
* **&lt;Koppla ned&gt;**: slutar ett anrop.
* **&lt;Pausa&gt;**: tyst väntar på ett angivet antal sekunder.
* **&lt;Spela upp&gt;**: spelar en ljudfil.
* **&lt;Kön&gt;**: Lägg till hello tooa kö med anropare.
* **&lt;Posten&gt;**: registrerar hello röst hello anroparen och returnerar en URL för en fil som innehåller hello registrering.
* **&lt;Omdirigera&gt;**: Överför kontroll över ett samtal eller SMS toohello TwiML på en annan URL.
* **&lt;Avvisa&gt;**: avvisar en inkommande anropa tooyour Twilio tal utan fakturering du.
* **&lt;Säg&gt;**: konverterar text toospeech som görs på ett samtal.
* **&lt;SMS&gt;**: skickar ett SMS-meddelande.

### <a id="TwiML"></a>TwiML
TwiML är en uppsättning XML-baserade instruktioner baserat på hello Twilio verb som informerar Twilio på hur tooprocess ett samtal eller SMS.

Exempelvis skulle hello följande TwiML omvandla hello text **Hello World** toospeech.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

När programmet-anrop hello Twilio-API, är en av parametrarna för hello API hello URL som returnerar hello TwiML svar. För utveckling, kan du använda Twilio-tillhandahållna URL: er tooprovide hello TwiML svar som används av dina program. Du kan också vara värd för dina egna URL: er tooproduce hello TwiML svar och ett annat alternativ är toouse hello `TwiMLResponse` objekt.

Läs mer om Twilio verb, deras attribut och TwiML [TwiML][twiml]. Mer information om hello Twilio-API finns [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Skapa ett Twilio-konto
När du är klar tooget Twilio-konto kan logga på [försök Twilio][try_twilio]. Du kan börja med ett kostnadsfritt konto och uppgradera ditt konto senare.

När du registrerar dig för ett Twilio-konto får du ett SID-konto och en token för autentisering. Båda ska vara nödvändiga toomake Twilio API-anrop. tooprevent obehörig åtkomst till tooyour konto, skydda din token för autentisering. Ditt konto SID och autentiseringstoken kan visas i hello [Twilio konsolen][twilio_console]i hello fält med namnet **konto SID** och **AUTH TOKEN**respektive.

## <a id="create_app"></a>Skapa en Python-program
En Python-program som använder hello Twilio-tjänsten och körs i Azure är inte annorlunda än andra Python-program som använder hello Twilio tjänst. När Twilio är REST-baserad och kan anropas från Python på flera sätt, den här artikeln fokuserar på hur toouse Twilio tjänster med [Twilio-biblioteket för Python från GitHub][twilio_python]. Mer information om hur du använder hello Twilio-biblioteket för Python finns [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].

Första [ställa in en ny Azure Linux VM] [azure_vm_setup] tooact som värd för ditt nya Python-webbprogram. När hello virtuella datorn körs, måste tooexpose programmet på en offentlig port som beskrivs nedan.

### <a name="add-an-incoming-rule"></a>Lägg till en inkommande regel
  1. Gå toohello [Nätverkssäkerhetsgruppen] [azure_nsg] sidan.
  2. Välj hello Nätverkssäkerhetsgrupp som överensstämmer med den virtuella datorn.
  3. Lägg till och **utgående regel** för **port 80**. Vara säker på att tooallow inkommande från alla adresser.

### <a name="set-hello-dns-name-label"></a>Ange hello DNS-namnetikett
  1. Gå toohello [hello offentliga IP-postadresser] [azure_ips] sidan.
  2. Välj hello offentliga IP-Adressen som correspends med den virtuella datorn.
  3. Ange hello **DNS-namnetikett** i hello **Configuration** avsnitt. Det här exemplet hello gäller den ser ut ungefär så här *din domän etikett*. centralus.cloudapp.azure.com

När du har tooconnect via SSH toohello virtuell dator som du kan installera hello webbramverk önskat (hello två mest kända i Python som [Flask](http://flask.pocoo.org/) och [Django](https://www.djangoproject.com)). Du kan installera någon av dem genom att köra hello `pip install` kommando.

Tänk på att vi har konfigurerat hello virtuella tooallow trafik endast på port 80. Se till att tooconfigure hello programmet toouse denna port.

## <a id="configure_app"></a>Konfigurera ditt program tooUse Twilio-bibliotek
Du kan konfigurera biblioteket programmet toouse hello Twilio för Python på två sätt:

* Installera hello Twilio-biblioteket för Python som Pip-paket. Den kan installeras med hello följande kommandon:
   
        $ pip install twilio

    ELLER

* Hämta hello Twilio-biblioteket för Python från GitHub ([https://github.com/twilio/twilio-python][twilio_python]) och installera den så här:

        $ python setup.py install

När du har installerat hello Twilio-biblioteket för Python, kan du sedan `import` i Python-filer:

        import twilio

Mer information finns i [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Så här: göra en utgående anrop
hello följande visar hur en utgående toomake anropa. Den här koden används även en Twilio-tillhandahållna platsen tooreturn hello Twilio Markup Language (TwiML) svar. Ersätt värdena för hello **from_number** och **to_number** telefonnummer och se till att du har kontrollerat hello **from_number** telefonnummer för ditt Twilio-konto innan du kör hello-kod.

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

Som tidigare nämnts kan använder den här koden en Twilio-tillhandahållna platsen tooreturn hello TwiML svar. Du kan i stället använda din egen webbplats tooprovide hello TwiML svar; Mer information finns i [hur tooProvide TwiML svar från din egen webbplats](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Så här: skicka ett SMS-meddelande
hello nedan visar hur en SMS-meddelande med toosend hello `TwilioRestClient` klass. Hej **from_number** nummer tillhandahålls av Twilio för utvärderingsversionen konton toosend SMS-meddelanden. Hej **to_number** numret måste verifieras för ditt Twilio-konto innan du kör hello kod.

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

## <a id="howto_provide_twiml_responses"></a>Så här: Ange TwiML svar från din egen webbplats
När ditt program initierar en anropet toohello Twilio-API, skickar Twilio din begäran tooa URL-adress förväntades tooreturn TwiML svar. hello-exemplet ovan använder hello Twilio-tillhandahållna URL [http://twimlets.com/message][twimlet_message_url]. (När TwiML är avsedd för användning av Twilio, kan du visa den i webbläsaren. Klicka till exempel [http://twimlets.com/message] [ twimlet_message_url] toosee en tom `<Response>` element, klicka på ett annat exempel är [http://twimlets.com/message? Meddelande % 5B0 %5, D = Hello % 20World] [ twimlet_message_url_hello_world] toosee en `<Response>` element som innehåller en `<Say>` element.)

I stället för på hello angivna Twilio-URL, kan du skapa din egen webbplats som returnerar HTTP-svar. Du kan skapa hello webbplats på alla språk som returnerar XML-svar; Det här avsnittet förutsätter att du kommer att använda Python toocreate hello TwiML.

hello följande exempel kommer utdata TwiML svar som säger **Hello World** i hello-anropet.

Med Flask:

    from flask import Response
    @app.route("/")
    def hello():
        xml = '<Response><Say>Hello world.</Say></Response>'
        return Response(xml, mimetype='text/xml')

Med Django:

    from django.http import HttpResponse
    def hello(request):
        xml = '<Response><Say>Hello world.</Say></Response>'
        return HttpResponse(xml, content_type='text/xml')

Du kan se hello-exemplet ovan kan är hello TwiML svar bara ett XML-dokument. Hej Twilio-biblioteket för Python innehåller klasser som ska generera TwiML för dig. hello exemplet nedan ger hello motsvarande svar som ovan, men använder hello `twiml` modul i hello Twilio-biblioteket för Python:

    from twilio import twiml

    response = twiml.Response()
    response.say("Hello world.")
    print(str(response))

Läs mer om TwiML [https://www.twilio.com/docs/api/twiml][twiml_reference].

När du har Python programmet Konfigurera tooprovide TwiML svar använder hello URL för hello program som hello URL som skickades till hello `client.calls.create` metod. Till exempel om du har ett webbprogram med namnet **MyTwiML** distribuerade tooan Azure värdbaserade tjänsten kan du använda webbadressen som webhook som visas i följande exempel hello:

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

## <a id="AdditionalServices"></a>Så här: använda ytterligare Twilio-tjänster
Dessutom kan toohello exemplen nedan, Twilio erbjuder webbaserade API: er som du använda tooleverage ytterligare Twilio-funktioner från din Azure-program. Fullständig information finns i hello [Twilio-API-dokumentationen][twilio_api].

## <a id="NextSteps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i hello Twilio-tjänsten, följa dessa länkar toolearn mer:

* [Riktlinjer för Twilio-säkerhet][twilio_security_guidelines]
* [Twilio ta guider och exempelkod][twilio_howtos]
* [Twilio snabbstarten självstudier][twilio_quickstarts]
* [Twilio på GitHub][twilio_on_github]
* [Kontakta tooTwilio Support][twilio_support]

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
