---
title: "aaaHow toomake ett telefonsamtal från Twilio (PHP) | Microsoft Docs"
description: "Lär dig hur toomake ett telefonsamtal och skicka ett SMS-meddelandet med hello Twilio API-tjänsten på Azure. Exempel är för PHP-program."
documentationcenter: php
services: 
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 44e35adc-be06-4700-beee-8c9e2c20c540
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: e6fecc345bf9ae787d14d533bd8d96b175c2453b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomake-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Hur tooMake ett telefonsamtal med Twilio i ett PHP-program på Azure
hello följande exempel visar hur du kan använda Twilio toomake ett samtal från en PHP-webbplats finns i Azure. hello resulterande program frågar hello användaren för telefonsamtal värden som visas i följande skärmbild som visar hello.

![Azure anropet formuläret med hjälp av Twilio och PHP][twilio_php]

Du behöver toodo hello följande toouse hello koden i det här avsnittet:

1. Skaffa ett Twilio-konto och autentisering-token från din [Twilio konsolen][twilio_console]. tooget igång med Twilio, utvärdera priser på [http://www.twilio.com/pricing][twilio_pricing]. Du kan registrera dig för ett utvärderingskonto på [https://www.twilio.com/try-twilio][try_twilio].
2. Hämta hello [Twilio-biblioteket för PHP](https://github.com/twilio/twilio-php) eller installera den som ett PÄRONTRÄD paket. Mer information finns i hello [Readme-filen](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Installera hello Azure SDK för PHP. En översikt över hello SDK och instruktioner om hur du installerar den finns [konfigurera hello Azure SDK för PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md)

## <a name="create-a-web-form-for-making-a-call"></a>Skapa ett webbformulär för ett samtal
hello följande HTML kod visas hur toobuild en webbsida (**callform.html**) som hämtar användardata för ett samtal:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Automated call form</title>
</head>
<body>
  <h1>Automated Call Form</h1>
  <p>Fill in all fields and click <b>Make this call</b>.</p>
  <form action="makecall.php" method="post">
    <table>
      <tr>
        <td>To:</td>
        <td><input name="callTo" size="50" type="text" value=""></td>
      </tr>
      <tr>
        <td>From:</td>
        <td><input name="callFrom" size="50" type="text" value=""></td>
      </tr>
      <tr>
        <td>Call message:</td>
        <td><input name="callText" size="100" type="text" value="Hello. This is hello call text. Good bye."></td>
      </tr>
      <tr>
        <td colspan="2"><input type="submit" value="Make this call"></td>
      </tr>
    </table>
  </form><br>
</body>
</html>
```

## <a name="create-hello-code-toomake-hello-call"></a>Skapa hello kod toomake hello anrop
Hej följande kod visar hur toobuild **makecall.php**, som anropas när hello användaren skickar hello formuläret som visas av **callform.html**. hello koden nedan skapar anropet hälsningsmeddelande och genererar hello-anrop. Dessutom vara säker på att toouse ditt Twilio-konto och autentisering token från hello [Twilio konsolen] [ twilio_console] i stället för hello platshållare för värden som har tilldelats för**$sid** och **$token** i hello koden nedan.

```html
<html>
<head><title>Making call...</title></head>
<body>
<p>Your call is being made.</p>

<?php
require_once 'path/to/vendor/autoload.php';

$sid   = "your_account_sid";
$token = "your_authentication_token";

$from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
$to_number   = $_POST['callTo'];
$message     = $_POST['callText'];

$client = new Twilio\Rest\Client($sid, $token);

$call = $client->calls->create(
            $to_number,
            $from_number,
            array('url' => http://twimlets.com/message?Message=' . urlencode($message))
        );

echo "Call status: " . $call->status . "<br />";
echo "URI resource: " . $call->uri . "<br />";
?>
</body>
</html>
```

Dessutom toomaking hello-anrop **makecall.php** visar vissa anrop metadata, som visas i hello bilden nedan. Mer information om anropet metadata finns [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Azure anropet svaret med hjälp av Twilio och PHP][twilio_php_response]

## <a name="run-hello-application"></a>Kör programmet hello
hello nästa steg är toodeploy ditt program tooAzure webbplatser. hello innehåller följande artiklar hello information för att skapa en webbplats och distribuera din kod med Git, FTP eller WebMatrix (om inte all information i varje artikel relevanta):

* [Skapa en PHP-MySQL Azure-webbplats och distribuera med Git](app-service-web/web-sites-php-mysql-deploy-use-git.md)
* [Skapa en PHP-MySQL Azure-webbplats och distribuera med FTP](app-service-web/web-sites-php-mysql-deploy-use-ftp.md)

## <a name="next-steps"></a>Nästa steg
Den här koden har angetts tooshow du grundläggande funktioner med Twilio i PHP i Azure. Du kanske vill tooadd mer felhantering eller andra funktioner innan du distribuerar tooAzure i produktion. Exempel:

* Istället för att använda ett webbformulär, kan du använda Azure storage-blobbar eller SQL-databas toostore telefonnummer och anropa text. Information om hur du använder Azure storage-blobbar i PHP finns [med hjälp av Azure Storage med PHP-program][howto_blob_storage_php]. Information om hur du använder SQL-databas i PHP finns [med hjälp av SQL-databas med PHP-program][howto_sql_azure_php].
* Hej **makecall.php** kod använder Twilio-tillhandahållna URL ([http://twimlets.com/message][twimlet_message_url]) tooprovide svaret Twilio Markup Language (TwiML) som informerar Twilio hur tooproceed med hello-anrop. Till exempel hello TwiML som returneras kan innehålla en `<Say>` verb som resulterar i texten är talade toohello anrop mottagaren. Istället för att använda hello Twilio-tillhandahållna URL kan du bygga egna service toorespond tooTwilio begäran. Mer information finns i [hur tooUse Twilio för röst- och SMS-funktionerna i PHP][howto_twilio_voice_sms_php]. Mer information om TwiML kan hittas på [http://www.twilio.com/docs/api/twiml][twiml], och mer information om `<Say>` och andra Twilio-verb kan hittas på [http:// www.twilio.com/docs/API/twiml/Say][twilio_say].
* Läsa hello Twilio säkerhetsriktlinjer på [https://www.twilio.com/docs/security][twilio_docs_security].

Mer information om Twilio finns [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Se även
* [Hur tooUse Twilio för röst- och SMS-funktionerna i PHP](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/docs/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php
