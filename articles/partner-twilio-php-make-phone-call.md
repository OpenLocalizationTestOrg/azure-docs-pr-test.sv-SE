---
title: "Hur du gör ett telefonsamtal från Twilio (PHP) | Microsoft Docs"
description: "Lär dig att ringa ett telefonsamtal och skicka ett SMS-meddelande med Twilio-API-tjänsten på Azure. Exempel är för PHP-program."
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
ms.openlocfilehash: 9866a196b3be10548d7a431430e570b41c190fc0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Hur du gör ett telefonsamtal med Twilio i ett PHP-program på Azure
I följande exempel visas hur du kan använda Twilio ringa ett samtal från en PHP-webbplats finns i Azure. Exempelprogrammet uppmanas användaren för telefonsamtal värden som visas i följande skärmbild visar.

![Azure anropet formuläret med hjälp av Twilio och PHP][twilio_php]

Du behöver göra följande för att använda koden i det här avsnittet:

1. Skaffa ett Twilio-konto och autentisering-token från din [Twilio konsolen][twilio_console]. Kom igång med Twilio, utvärdera priser på [http://www.twilio.com/pricing][twilio_pricing]. Du kan registrera dig för ett utvärderingskonto på [https://www.twilio.com/try-twilio][try_twilio].
2. Hämta den [Twilio-biblioteket för PHP](https://github.com/twilio/twilio-php) eller installera den som ett PÄRONTRÄD paket. Mer information finns i [Readme-filen](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Installera Azure SDK för PHP. 
<!-- For an overview of the SDK and instructions on installing it, see [Set up the Azure SDK for PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md) -->

## <a name="create-a-web-form-for-making-a-call"></a>Skapa ett webbformulär för ett samtal
Följande HTML-kod visar hur du skapar en webbsida (**callform.html**) som hämtar användardata för ett samtal:

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
        <td><input name="callText" size="100" type="text" value="Hello. This is the call text. Good bye."></td>
      </tr>
      <tr>
        <td colspan="2"><input type="submit" value="Make this call"></td>
      </tr>
    </table>
  </form><br>
</body>
</html>
```

## <a name="create-the-code-to-make-the-call"></a>Skapa koden för att göra anrop
Följande kod visar hur du skapar **makecall.php**, som anropas när användaren skickar formuläret som visas av **callform.html**. Koden nedan skapar anropet meddelandet och genererar anropet. Dessutom måste du använda dina Twilio-konto och autentisering-token från den [Twilio konsolen] [ twilio_console] i stället för platshållarvärdena för **$sid** och  **$token** i koden nedan.

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

Förutom att göra anropet **makecall.php** visar vissa anrop metadata, som visas i bilden nedan. Mer information om anropet metadata finns [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Azure anropet svaret med hjälp av Twilio och PHP][twilio_php_response]

## <a name="run-the-application"></a>Köra programmet
Nästa steg är att [distribuera appen till Azure-Webbappar med Git](app-service/app-service-web-get-started-php.md) (även om inte alla informationen om det är relevant). 

## <a name="next-steps"></a>Nästa steg
Den här koden har angetts för att visa grundläggande funktioner med Twilio i PHP i Azure. Innan du distribuerar till Azure i produktion, kanske du vill lägga till flera felhantering eller andra funktioner. Exempel:

* Istället för att använda ett webbformulär, kunde du använda Azure storage-blobbar eller SQL-databas för att lagra telefonnummer och anropa text. Information om hur du använder Azure storage-blobbar i PHP finns [med hjälp av Azure Storage med PHP-program][howto_blob_storage_php]. Information om hur du använder SQL-databas i PHP finns [med hjälp av SQL-databas med PHP-program][howto_sql_azure_php].
* Den **makecall.php** kod använder Twilio-tillhandahållna URL ([http://twimlets.com/message][twimlet_message_url]) att tillhandahålla ett Twilio Markup Language (TwiML)-svar som informerar Twilio hur till Fortsätt med anropet. Till exempel TwiML som returneras kan innehålla en `<Say>` verb som resulterar i text som talas till mottagaren anrop. I stället för med den angivna Twilio-URL, kan du skapa tjänsten för att svara på Twilios begäran. Mer information finns i [så Använd Twilio för röst- och SMS-funktionerna i PHP][howto_twilio_voice_sms_php]. Mer information om TwiML kan hittas på [http://www.twilio.com/docs/api/twiml][twiml], och mer information om `<Say>` och andra Twilio-verb kan hittas på [http:// www.twilio.com/docs/API/twiml/Say][twilio_say].
* Läs Twilio-riktlinjer för säkerhet på [https://www.twilio.com/docs/security][twilio_docs_security].

Mer information om Twilio finns [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Se även
* [Hur du använder Twilio för röst- och SMS-funktionerna i PHP](partner-twilio-php-how-to-use-voice-sms.md)

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
[twilio_php_github]: https://github.com/twilio/twilio-php
