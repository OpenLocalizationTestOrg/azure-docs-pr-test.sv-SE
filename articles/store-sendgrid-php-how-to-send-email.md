---
title: "aaaHow toouse hello SendGrid e-posttjänst (PHP) | Microsoft Docs"
description: "Lär dig hur skicka e-post med hello SendGrid e-posttjänst på Azure. Kodexempel som skrivits i PHP."
documentationcenter: php
services: 
manager: sendgrid
editor: mollybos
author: thinkingserious
ms.assetid: 51a9928a-4c9e-4b0a-aca8-9a408aeb6f47
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 10/30/2014
ms.author: elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com
ms.openlocfilehash: 0076e56dc185cb8f52e629395e7d2c143cb5cfa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-sendgrid-email-service-from-php"></a>Hur tooUse hello SendGrid e-posttjänst från PHP
Den här guiden visar hur tooperform vanliga programmeringsuppgifter med hello SendGrid e-tjänsten på Azure. hello exempel skrivs i PHP.
hello beskrivs scenarier där **konstruera e-post**, **skicka e-post**, och **lägga till bilagor**. Mer information om SendGrid och skicka e-post finns hello [nästa steg](#next-steps) avsnitt.

## <a name="what-is-hello-sendgrid-email-service"></a>Vad är hello SendGrid e-posttjänst?
SendGrid är en [molnbaserade e-posttjänst] som ger tillförlitliga [transaktionella e-postleverans], skalbarhet och analys i realtid tillsammans med flexibel API: er som gör det enkelt anpassad integrering. Vanliga Användningsscenarier för SendGrid är:

* Automatiskt skickar kvitton toocustomers
* Administrera distribution visas för att skicka kunder månatliga e-reklamblad och specialerbjudanden
* Samla in realtid mätvärden för sådant som blockerade e-post och kunden svarstider
* Generera rapporter toohelp identifiera trender
* Vidarebefordran av kundfrågor
* E-postaviseringar från ditt program

Mer information finns i [https://sendgrid.com][https://sendgrid.com].

## <a name="create-a-sendgrid-account"></a>Skapa ett SendGrid-konto
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>Med SendGrid från PHP-program
Med SendGrid i ett Azure PHP-program kräver ingen särskild konfiguration eller kodning. Eftersom SendGrid är en tjänst, det kan användas i hello exakt samma sätt från ett moln-program som det kan från ett lokalt program.

## <a name="how-to-send-an-email"></a>Så här: skicka ett e-postmeddelande
Du kan skicka e-post med SMTP- eller hello webb-API som tillhandahålls av SendGrid.

### <a name="smtp-api"></a>SMTP-API
toosend e-post med hello SendGrid SMTP-API, Använd *Swift adresshuvud*, ett komponentbaserade bibliotek för att skicka e-post från PHP-program. Du kan hämta hello *Swift adresshuvud* bibliotek från [http://swiftmailer.org/download] [ http://swiftmailer.org/download] v5.3.0 (använda [Composer] tooinstall SWIFT adresshuvud). Skicka e-post med hello bibliotek innebär att du skapar instanser av den <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_adresshuvud</span>, och <span class="auto-style2">Swift\_meddelande </span> klasser, ange lämpliga egenskaper och anropa den <span class="auto-style2">Swift\_Mailer::send</span> metod.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create hello body of hello message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of hello email
      * If hello receiver is able tooview html emails then only hello html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name tooAppear');
     // Email recipients
     $too= array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';

     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach hello body of hello email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');

     // send message
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out too'.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a>Webb-API
Använd PHP'S [curl funktionen] [ curl function] toosend e-post med hjälp av hello SendGrid Web API.

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );

     $request = $url.'api/mail.send.json';

     // Generate curl request
     $session = curl_init($request);

     // Tell curl toouse HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);

     // Tell curl that this is hello body of hello POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

     // Tell curl not tooreturn headers, but do return hello response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

     // obtain response
     $response = curl_exec($session);
     curl_close($session);

     // print everything out
     print_r($response);

Sendgrid's Web API är mycket lik tooa REST-API, om den inte är verkligen en RESTful-API eftersom i de flesta anrop, både få och POST verb är utbytbara.

## <a name="how-to-add-an-attachment"></a>Så här: Lägg till en bifogad fil
### <a name="smtp-api"></a>SMTP-API
En ytterligare rad med kod toohello exempelskript för att skicka ett e-postmeddelande med Swift adresshuvud innebär att skicka bifogade filer med hjälp av hello SMTP-API.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create hello body of hello message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of hello email
      * If hello reciever is able tooview html emails then only hello html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name tooAppear');

     // Email recipients
     $too= array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';

     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach hello body of hello email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));

     // send message
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out too'.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

hello ytterligare kodrad är följande:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

Den här raden i koden anropar hello bifoga metoden på den <span class="auto-style2">Swift\_meddelandet</span> objekt och använder statisk metod <span class="auto-style2">fromPath</span> på den <span class="auto-style2">Swift\_bilaga</span>klassen tooget och bifoga en fil tooa meddelande.

### <a name="web-api"></a>Webb-API
Skicka en bifogad fil med hello Web API är mycket lik toosending en e-post med hjälp av hello webb-API. Observera dock att hello följande exempel hello-parametermatris måste innehåller i det här elementet:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Exempel:

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';

     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> hello HTML </p>',
         'text' => 'hello plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );

     print_r($params);

     $request = $url.'api/mail.send.json';

     // Generate curl request
     $session = curl_init($request);

     // Tell curl toouse HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);

     // Tell curl that this is hello body of hello POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

     // Tell curl not tooreturn headers, but do return hello response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

     // obtain response
     $response = curl_exec($session);
     curl_close($session);

     // print everything out
     print_r($response);

## <a name="how-to-use-filters-tooenable-footers-tracking-and-analytics"></a>Så här: filter tooEnable sidfötter, spårning och analys
SendGrid ger ytterligare e-postfunktioner via hello ”filter”. Dessa är inställningar som kan läggas till tooan e-postmeddelande för att aktivera vissa funktioner, till exempel aktivera klickar du på spårning, Google analytics, prenumerationen spårning och så vidare.

Filter kan vara tillämpade tooa meddelande med hello filter-egenskapen. Varje filter som anges av ett hash-värde som innehåller filter-specifika inställningar. I följande exempel aktiverar hello sidfot filter och anger ett textmeddelande som ska läggas till toohello längst ned på hello e-postmeddelande.
Det här exemplet ska vi använda [sendgrid php biblioteket].
Använd [Composer] tooinstall bibliotek:

    php composer.phar require sendgrid/sendgrid 2.1.1

Exempel:    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // hello list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request tooSendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify hello names of hello recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of hello above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have
     // enabled hello footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // hello subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy hello user here. If
     // a sender list IS specified above, this email address becomes irrelevant.
     $too= 'john@contoso.com';

     # Create hello body of hello message (a plain-text and an HTML version).
     # text is your plain-text email
     # html is your html version of hello email
     # if hello receiver is able tooview html emails then only hello html
     # email will be displayed

     /*
      * Note hello variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment toocall you at -time- EST toodiscuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html>
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     toocall you at -time- EST toodiscuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach hello body of hello email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i hello SendGrid e-posttjänst, följa dessa länkar toolearn mer.

* SendGrid-dokumentation: <https://sendgrid.com/docs>
* SendGrid PHP-bibliotek: <https://github.com/sendgrid/sendgrid-php>
* SendGrid specialerbjudande för Azure-kunder: <https://sendgrid.com/windowsazure.html>

Mer information finns också hello [PHP Developer Center](/develop/php/).

[https://sendgrid.com]: https://sendgrid.com
[https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
[special offer]: https://www.sendgrid.com/windowsazure.html
[Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
[http://swiftmailer.org/download]: http://swiftmailer.org/download
[curl function]: http://php.net/curl
[molnbaserade e-posttjänst]: https://sendgrid.com/email-solutions
[transaktionella e-postleverans]: https://sendgrid.com/transactional-email
[sendgrid php biblioteket]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
[Composer]: https://getcomposer.org/download/
