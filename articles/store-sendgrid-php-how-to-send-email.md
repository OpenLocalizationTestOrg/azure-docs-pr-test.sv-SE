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
# <a name="how-toouse-hello-sendgrid-email-service-from-php"></a><span data-ttu-id="1e461-104">Hur tooUse hello SendGrid e-posttjänst från PHP</span><span class="sxs-lookup"><span data-stu-id="1e461-104">How tooUse hello SendGrid Email Service from PHP</span></span>
<span data-ttu-id="1e461-105">Den här guiden visar hur tooperform vanliga programmeringsuppgifter med hello SendGrid e-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="1e461-105">This guide demonstrates how tooperform common programming tasks with hello SendGrid email service on Azure.</span></span> <span data-ttu-id="1e461-106">hello exempel skrivs i PHP.</span><span class="sxs-lookup"><span data-stu-id="1e461-106">hello samples are written in PHP.</span></span>
<span data-ttu-id="1e461-107">hello beskrivs scenarier där **konstruera e-post**, **skicka e-post**, och **lägga till bilagor**.</span><span class="sxs-lookup"><span data-stu-id="1e461-107">hello scenarios covered include **constructing email**, **sending email**, and **adding attachments**.</span></span> <span data-ttu-id="1e461-108">Mer information om SendGrid och skicka e-post finns hello [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1e461-108">For more information on SendGrid and sending email, see hello [Next Steps](#next-steps) section.</span></span>

## <a name="what-is-hello-sendgrid-email-service"></a><span data-ttu-id="1e461-109">Vad är hello SendGrid e-posttjänst?</span><span class="sxs-lookup"><span data-stu-id="1e461-109">What is hello SendGrid Email Service?</span></span>
<span data-ttu-id="1e461-110">SendGrid är en [molnbaserade e-posttjänst] som ger tillförlitliga [transaktionella e-postleverans], skalbarhet och analys i realtid tillsammans med flexibel API: er som gör det enkelt anpassad integrering.</span><span class="sxs-lookup"><span data-stu-id="1e461-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="1e461-111">Vanliga Användningsscenarier för SendGrid är:</span><span class="sxs-lookup"><span data-stu-id="1e461-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="1e461-112">Automatiskt skickar kvitton toocustomers</span><span class="sxs-lookup"><span data-stu-id="1e461-112">Automatically sending receipts toocustomers</span></span>
* <span data-ttu-id="1e461-113">Administrera distribution visas för att skicka kunder månatliga e-reklamblad och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="1e461-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="1e461-114">Samla in realtid mätvärden för sådant som blockerade e-post och kunden svarstider</span><span class="sxs-lookup"><span data-stu-id="1e461-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="1e461-115">Generera rapporter toohelp identifiera trender</span><span class="sxs-lookup"><span data-stu-id="1e461-115">Generating reports toohelp identify trends</span></span>
* <span data-ttu-id="1e461-116">Vidarebefordran av kundfrågor</span><span class="sxs-lookup"><span data-stu-id="1e461-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="1e461-117">E-postaviseringar från ditt program</span><span class="sxs-lookup"><span data-stu-id="1e461-117">Email notifications from your application</span></span>

<span data-ttu-id="1e461-118">Mer information finns i [https://sendgrid.com][https://sendgrid.com].</span><span class="sxs-lookup"><span data-stu-id="1e461-118">For more information, see [https://sendgrid.com][https://sendgrid.com].</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="1e461-119">Skapa ett SendGrid-konto</span><span class="sxs-lookup"><span data-stu-id="1e461-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a><span data-ttu-id="1e461-120">Med SendGrid från PHP-program</span><span class="sxs-lookup"><span data-stu-id="1e461-120">Using SendGrid from your PHP Application</span></span>
<span data-ttu-id="1e461-121">Med SendGrid i ett Azure PHP-program kräver ingen särskild konfiguration eller kodning.</span><span class="sxs-lookup"><span data-stu-id="1e461-121">Using SendGrid in an Azure PHP application requires no special configuration or coding.</span></span> <span data-ttu-id="1e461-122">Eftersom SendGrid är en tjänst, det kan användas i hello exakt samma sätt från ett moln-program som det kan från ett lokalt program.</span><span class="sxs-lookup"><span data-stu-id="1e461-122">Because SendGrid is a service, it can be accessed in exactly hello same way from a cloud application as it can from an on-premises application.</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="1e461-123">Så här: skicka ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="1e461-123">How to: Send an Email</span></span>
<span data-ttu-id="1e461-124">Du kan skicka e-post med SMTP- eller hello webb-API som tillhandahålls av SendGrid.</span><span class="sxs-lookup"><span data-stu-id="1e461-124">You can send email using either SMTP or hello Web API provided by SendGrid.</span></span>

### <a name="smtp-api"></a><span data-ttu-id="1e461-125">SMTP-API</span><span class="sxs-lookup"><span data-stu-id="1e461-125">SMTP API</span></span>
<span data-ttu-id="1e461-126">toosend e-post med hello SendGrid SMTP-API, Använd *Swift adresshuvud*, ett komponentbaserade bibliotek för att skicka e-post från PHP-program.</span><span class="sxs-lookup"><span data-stu-id="1e461-126">toosend email using hello SendGrid SMTP API, use *Swift Mailer*, a component-based library for sending emails from PHP applications.</span></span> <span data-ttu-id="1e461-127">Du kan hämta hello *Swift adresshuvud* bibliotek från [http://swiftmailer.org/download] [ http://swiftmailer.org/download] v5.3.0 (använda [Composer] tooinstall SWIFT adresshuvud).</span><span class="sxs-lookup"><span data-stu-id="1e461-127">You can download hello *Swift Mailer* library from [http://swiftmailer.org/download][http://swiftmailer.org/download] v5.3.0 (use [Composer] tooinstall Swift Mailer).</span></span> <span data-ttu-id="1e461-128">Skicka e-post med hello bibliotek innebär att du skapar instanser av den <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_adresshuvud</span>, och <span class="auto-style2">Swift\_meddelande </span> klasser, ange lämpliga egenskaper och anropa den <span class="auto-style2">Swift\_Mailer::send</span> metod.</span><span class="sxs-lookup"><span data-stu-id="1e461-128">Sending email with hello library involves creating instances of the <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_Mailer</span>, and <span class="auto-style2">Swift\_Message</span> classes, setting the appropriate properties, and calling the <span class="auto-style2">Swift\_Mailer::send</span> method.</span></span>

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

### <a name="web-api"></a><span data-ttu-id="1e461-129">Webb-API</span><span class="sxs-lookup"><span data-stu-id="1e461-129">Web API</span></span>
<span data-ttu-id="1e461-130">Använd PHP'S [curl funktionen] [ curl function] toosend e-post med hjälp av hello SendGrid Web API.</span><span class="sxs-lookup"><span data-stu-id="1e461-130">Use PHP's [curl function][curl function] toosend email using hello SendGrid Web API.</span></span>

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

<span data-ttu-id="1e461-131">Sendgrid's Web API är mycket lik tooa REST-API, om den inte är verkligen en RESTful-API eftersom i de flesta anrop, både få och POST verb är utbytbara.</span><span class="sxs-lookup"><span data-stu-id="1e461-131">SendGrid's Web API is very similar tooa REST API, though it is not truly a RESTful API since, in most calls, both GET and POST verbs can be used interchangeably.</span></span>

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="1e461-132">Så här: Lägg till en bifogad fil</span><span class="sxs-lookup"><span data-stu-id="1e461-132">How to: Add an Attachment</span></span>
### <a name="smtp-api"></a><span data-ttu-id="1e461-133">SMTP-API</span><span class="sxs-lookup"><span data-stu-id="1e461-133">SMTP API</span></span>
<span data-ttu-id="1e461-134">En ytterligare rad med kod toohello exempelskript för att skicka ett e-postmeddelande med Swift adresshuvud innebär att skicka bifogade filer med hjälp av hello SMTP-API.</span><span class="sxs-lookup"><span data-stu-id="1e461-134">Sending an attachment using hello SMTP API involves one additional line of code toohello example script for sending an email with Swift Mailer.</span></span>

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

<span data-ttu-id="1e461-135">hello ytterligare kodrad är följande:</span><span class="sxs-lookup"><span data-stu-id="1e461-135">hello additional line of code is as follows:</span></span>

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

<span data-ttu-id="1e461-136">Den här raden i koden anropar hello bifoga metoden på den <span class="auto-style2">Swift\_meddelandet</span> objekt och använder statisk metod <span class="auto-style2">fromPath</span> på den <span class="auto-style2">Swift\_bilaga</span>klassen tooget och bifoga en fil tooa meddelande.</span><span class="sxs-lookup"><span data-stu-id="1e461-136">This line of code calls hello attach method on the <span class="auto-style2">Swift\_Message</span> object and uses static method <span class="auto-style2">fromPath</span> on the <span class="auto-style2">Swift\_Attachment</span> class tooget and attach a file tooa message.</span></span>

### <a name="web-api"></a><span data-ttu-id="1e461-137">Webb-API</span><span class="sxs-lookup"><span data-stu-id="1e461-137">Web API</span></span>
<span data-ttu-id="1e461-138">Skicka en bifogad fil med hello Web API är mycket lik toosending en e-post med hjälp av hello webb-API.</span><span class="sxs-lookup"><span data-stu-id="1e461-138">Sending an attachment using hello Web API is very similar toosending an email using hello Web API.</span></span> <span data-ttu-id="1e461-139">Observera dock att hello följande exempel hello-parametermatris måste innehåller i det här elementet:</span><span class="sxs-lookup"><span data-stu-id="1e461-139">However, note that in hello example that follows, hello parameter array must contain this element:</span></span>

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

<span data-ttu-id="1e461-140">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1e461-140">Example:</span></span>

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

## <a name="how-to-use-filters-tooenable-footers-tracking-and-analytics"></a><span data-ttu-id="1e461-141">Så här: filter tooEnable sidfötter, spårning och analys</span><span class="sxs-lookup"><span data-stu-id="1e461-141">How to: Use Filters tooEnable Footers, Tracking, and Analytics</span></span>
<span data-ttu-id="1e461-142">SendGrid ger ytterligare e-postfunktioner via hello ”filter”.</span><span class="sxs-lookup"><span data-stu-id="1e461-142">SendGrid provides additional email functionality through hello use of 'filters'.</span></span> <span data-ttu-id="1e461-143">Dessa är inställningar som kan läggas till tooan e-postmeddelande för att aktivera vissa funktioner, till exempel aktivera klickar du på spårning, Google analytics, prenumerationen spårning och så vidare.</span><span class="sxs-lookup"><span data-stu-id="1e461-143">These are settings that can be added tooan email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span>

<span data-ttu-id="1e461-144">Filter kan vara tillämpade tooa meddelande med hello filter-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="1e461-144">Filters can be applied tooa message by using hello filters property.</span></span> <span data-ttu-id="1e461-145">Varje filter som anges av ett hash-värde som innehåller filter-specifika inställningar.</span><span class="sxs-lookup"><span data-stu-id="1e461-145">Each filter is specified by a hash containing filter-specific settings.</span></span> <span data-ttu-id="1e461-146">I följande exempel aktiverar hello sidfot filter och anger ett textmeddelande som ska läggas till toohello längst ned på hello e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="1e461-146">The following example enables hello footer filter and specifies a text message that will be appended toohello bottom of hello email message.</span></span>
<span data-ttu-id="1e461-147">Det här exemplet ska vi använda [sendgrid php biblioteket].</span><span class="sxs-lookup"><span data-stu-id="1e461-147">For this example we will use [sendgrid-php library].</span></span>
<span data-ttu-id="1e461-148">Använd [Composer] tooinstall bibliotek:</span><span class="sxs-lookup"><span data-stu-id="1e461-148">Use [Composer] tooinstall library:</span></span>

    php composer.phar require sendgrid/sendgrid 2.1.1

<span data-ttu-id="1e461-149">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1e461-149">Example:</span></span>    

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

## <a name="next-steps"></a><span data-ttu-id="1e461-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e461-150">Next Steps</span></span>
<span data-ttu-id="1e461-151">Nu när du har lärt dig hello grunderna i hello SendGrid e-posttjänst, följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="1e461-151">Now that you've learned hello basics of hello SendGrid Email service, follow these links toolearn more.</span></span>

* <span data-ttu-id="1e461-152">SendGrid-dokumentation: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="1e461-152">SendGrid documentation: <https://sendgrid.com/docs></span></span>
* <span data-ttu-id="1e461-153">SendGrid PHP-bibliotek: <https://github.com/sendgrid/sendgrid-php></span><span class="sxs-lookup"><span data-stu-id="1e461-153">SendGrid PHP library: <https://github.com/sendgrid/sendgrid-php></span></span>
* <span data-ttu-id="1e461-154">SendGrid specialerbjudande för Azure-kunder: <https://sendgrid.com/windowsazure.html></span><span class="sxs-lookup"><span data-stu-id="1e461-154">SendGrid special offer for Azure customers: <https://sendgrid.com/windowsazure.html></span></span>

<span data-ttu-id="1e461-155">Mer information finns också hello [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="1e461-155">For more information, see also hello [PHP Developer Center](/develop/php/).</span></span>

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
