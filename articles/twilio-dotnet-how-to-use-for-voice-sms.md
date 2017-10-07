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
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-from-azure"></a>Hur toouse Twilio för röst- och SMS-funktioner från Azure
Den här guiden visar hur tooperform vanliga programmeringsuppgifter med hello Twilio API-tjänsten på Azure. hello-scenarier som tas upp inkluderar att ringa och ett kort meddelande (SMS Service)-meddelande skickas. Mer information om Twilio och använder röst- och SMS i dina program finns hello [nästa steg](#NextSteps) avsnitt.

## <a id="WhatIs"></a>Vad är Twilio?
Twilio startar hello framtiden för kommunikation, aktivera utvecklare tooembed röst, VoIP, och meddelanden i program. De virtualisera alla infrastruktur som behövs i en molnbaserad, globala exponera den via hello Twilio kommunikation API-plattformen. Programmen är enkla toobuild och skalbara. Utnyttja flexibilitet med betala per användning priser och dra nytta av molnet tillförlitlighet.

**Twilio röst** kan ditt program toomake och ta emot telefonsamtal. **Twilio SMS** gör att dina program toosend och ta emot SMS-meddelanden. **Twilio klienten** tillåter du toomake VoIP-anrop från telefon, surfplatta eller webbläsare och stöder WebRTC.

## <a id="Pricing"></a>Priser för Twilio och specialerbjudanden
Azure-kunder får en [specialerbjudande](http://www.twilio.com/azure): kostnadsfri $10 Twilio kredit när du uppgraderar ditt Twilio-konto. Den här Twilio-kredit kan vara tillämpade tooany Twilio användning ($10 kredit motsvarande toosending upp till 1 000 SMS-meddelanden eller ta emot in too1000 inkommande röst minuter beroende på hello platsen för ditt mål för telefon och meddelandet eller samtal). Lösa in den här Twilio-kredit och kom igång med [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).

Twilio är en betalning per användning. Det finns inga avgifter för installation och du kan stänga ditt konto när som helst. Du hittar mer information i [Twilio priser](http://www.twilio.com/voice/pricing).

## <a id="Concepts"></a>Begrepp
Hej Twilio API är en RESTful-API som tillhandahåller röst- och SMS-funktioner för program. Klientbibliotek är tillgängliga på flera språk. en lista, se [Twilio-API-bibliotek][twilio_libraries].

Viktiga aspekter av hello Twilio API är Twilio verb och Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-verb
hello API väljer Twilio verb; till exempel hello  **&lt;säg&gt;**  verb instruerar Twilio tooaudibly leverera ett meddelande på ett samtal.

hello följer en lista över Twilio verb.  Lär dig mer om hello andra verb och funktioner via [Markup Language Twilio dokumentationen](http://www.twilio.com/docs/api/twiml).

* **&lt;Ring&gt;**: ansluter hello anroparen tooanother phone.
* **&lt;Samla in&gt;**: samlar in siffror som anges på hello telefon tangentbordet.
* **&lt;Koppla ned&gt;**: slutar ett anrop.
* **&lt;Spela upp&gt;**: spelar en ljudfil.
* **&lt;Pausa&gt;**: tyst väntar på ett angivet antal sekunder.
* **&lt;Posten&gt;**: registrerar hello anroparen röst och returnerar ett URL-Adressen till en fil som innehåller hello registrering.
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

## <a id="create_app"></a>Skapa ett Azure-program
Ett Azure-program som är värd för ett Twilio-aktiverat program skiljer sig inte från andra Azure-program. Du lägger till hello Twilio .NET-bibliotek och konfigurera hello rollen toouse hello Twilio .NET-bibliotek.
Information om hur du skapar ett första Azure-projekt finns [skapa ett Azure-projekt i Visual Studio][vs_project].

## <a id="configure_app"></a>Konfigurera ditt program toouse Twilio-bibliotek
Twilio tillhandahåller en uppsättning .NET helper bibliotek som omsluter olika aspekter av Twilio tooprovide enkelt och smidigt sätt toointeract med hello Twilio REST-API och Twilio klienten toogenerate TwiML svar.

Twilio innehåller fem bibliotek för .NET-utvecklare:
Bibliotek|Beskrivning
---|---
Twilio.API|Hej Twilio Kärnbibliotek som omsluter hello Twilio REST API i ett eget .NET-bibliotek. Det här biblioteket är tillgängliga för .NET, Silverlight och Windows Phone 7.
Twilio.TwiML|Ger en .NET eget sätt toogenerate TwiML markering.
Twilio.MVC|För utvecklare som använder ASP.NET MVC, innehåller det här biblioteket en TwilioController och TwiML ActionResult begäran verifieringsattribut.
Twilio.WebMatrix|För utvecklare som använder Microsofts ledigt WebMatrix utvecklingsverktyg för innehåller det här biblioteket Razor syntax hjälpprogram för olika Twilio-åtgärder.
Twilio.Client.Capability|Innehåller hello kapaciteten token generatorn för användning med hello Twilio klienten JavaScript SDK.

Observera att alla bibliotek kräver .NET 3.5, Silverlight 4 eller Windows Phone 7 eller senare.

hello exemplen i den här guiden använder hello Twilio.API library.

hello-biblioteken kan vara [installeras med hello NuGet package manager tillägget](http://www.twilio.com/docs/csharp/install) tillgängliga för Visual Studio 2010 in too2015.  hello källkoden finns på [GitHub][twilio_github_repo], som innehåller en Wiki som innehåller komplett dokumentation för att använda hello-bibliotek.

Som standard installerar Microsoft Visual Studio 2010 version 1.2 av NuGet. Installera hello Twilio bibliotek kräver version 1.6 av NuGet eller högre. Mer information om installation eller uppdatering av NuGet finns [http://nuget.org/][nuget].

> [!NOTE]
> tooinstall hello senaste versionen av NuGet, måste du först avinstallera hello inlästa version med hello Tilläggshanteraren för Visual Studio. toodo så måste du köra Visual Studio som administratör. Annars är hello avinstallera inaktiverat.
>
>

### <a id="use_nuget"></a>tooadd hello Twilio bibliotek tooyour Visual Studio-projekt:
1. Öppna din lösning i Visual Studio.
2. Högerklicka på **referenser**.
3. Klicka på **hantera NuGet-paket...**
4. Klicka på **Online**.
5. Skriv i hello online sökrutan *twilio*.
6. Klicka på **installera** på hello Twilio-paketet.

## <a id="howto_make_call"></a>Så här: göra en utgående anrop
hello nedan visar hur en utgående toomake Ring upp med hello **CallResource** klass. Den här koden används även en Twilio-tillhandahållna platsen tooreturn hello Twilio Markup Language (TwiML) svar. Ersätt värdena för hello **till** och **från** telefonnummer och se till att du kontrollerar hello **från** telefonnummer för ditt Twilio-konto innan du kör hello kod.

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

Mer information om hello parametrar i toohello **CallResource.Create** -metoden finns [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Som tidigare nämnts kan använder den här koden en Twilio-tillhandahållna platsen tooreturn hello TwiML svar. Du kan i stället använda din egen webbplats tooprovide hello TwiML svar. Mer information finns i [så här: Ange TwiML svar från din egen webbplats](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Så här: skicka ett SMS-meddelande
hello följande skärmbild visar hur en SMS-meddelande med toosend hello **MessageResource** klass. Hej **från** nummer tillhandahålls av Twilio för utvärderingsversionen konton toosend SMS-meddelanden. Hej **till** numret måste verifieras för ditt Twilio-konto innan du kör hello kod.

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

## <a id="howto_provide_twiml_responses"></a>Så här: Ange TwiML svar från din egen webbplats
När ditt program initierar anrop-toohello Twilio API – exempelvis via hello **CallResource.Create** metod - Twilio skickar din begäran tooan URL som är förväntade tooreturn TwiML svar. hello exemplet i [så här: utgående ringa](#howto_make_call) använder hello Twilio-tillhandahållna URL [http://twimlets.com/message] [ twimlet_message_url] tooreturn hello svar.

> [!NOTE]
> Medan TwiML är avsedd för användning av webbtjänster, kan du visa hello TwiML i webbläsaren. Klicka till exempel [http://twimlets.com/message] [ twimlet_message_url] toosee en tom &lt;svar&gt; element, klicka på ett annat exempel är [http://twimlets.com/message ? Meddelande % 5B0 %5 D = Hello % 20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) toosee en &lt;svar&gt; element som innehåller en &lt;säg&gt; element.
>
>

I stället för på hello angivna Twilio-URL, kan du skapa din egen URL: en webbplats som returnerar HTTP-svar. Du kan skapa hello webbplats på alla språk som returnerar HTTP-svar. Det här avsnittet förutsätter att du ska vara värd för hello URL från en allmän ASP.NET-hanterare.

hello efter ASP.NET-hanteraren utformar TwiML svar som säger **Hello World** i hello-anropet.

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
    
Du kan se hello-exemplet ovan kan är hello TwiML svar bara ett XML-dokument. Hej Twilio.TwiML biblioteket innehåller klasser som ska generera TwiML för dig. hello exemplet nedan ger hello motsvarande svar som ovan, men använder hello **VoiceResponse** klass.

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

Läs mer om TwiML [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).

När du har konfigurerat en sätt tooprovide TwiML svar du kan ange den URL-toohello **CallResource.Create** metod. Till exempel om du har ett webbprogram med namnet MyTwiML distribueras tooan Azure-molntjänst och hello namnet på din ASP.NET-hanteraren är mytwiml.ashx, hello URL kan skickas för**CallResource.Create** som visas i följande kod hello Exempel:

    // This sample uses hello sandbox number provided by Twilio toomake hello call.
    // Place hello call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx"));
        }

Mer information om hur du använder Twilio på Azure med ASP.NET finns [hur toomake en telefon ringer med Twilio i en webbroll på Azure][howto_phonecall_dotnet].

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
