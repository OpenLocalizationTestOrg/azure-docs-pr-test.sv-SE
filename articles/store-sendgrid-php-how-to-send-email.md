---
title: "Hur du använder SendGrid e-posttjänst (PHP) | Microsoft Docs"
description: "Lär dig hur skicka e-post med SendGrid-e-posttjänsten på Azure. Kodexempel som skrivits i PHP."
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
ms.openlocfilehash: 523b986f66a2e48685e9707903194856f0dcf4a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a><span data-ttu-id="2cf41-104">Hur du använder tjänsten SendGrid e-post från PHP</span><span class="sxs-lookup"><span data-stu-id="2cf41-104">How to Use the SendGrid Email Service from PHP</span></span>
<span data-ttu-id="2cf41-105">Den här guiden visar hur du utför vanliga programmeringsuppgifter med SendGrid-e-posttjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="2cf41-105">This guide demonstrates how to perform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="2cf41-106">Exemplen är skrivna i PHP.</span><span class="sxs-lookup"><span data-stu-id="2cf41-106">The samples are written in PHP.</span></span>
<span data-ttu-id="2cf41-107">Scenarier som tas upp inkluderar **konstruera e-post**, **skicka e-post**, och **lägga till bilagor**.</span><span class="sxs-lookup"><span data-stu-id="2cf41-107">The scenarios covered include **constructing email**, **sending email**, and **adding attachments**.</span></span> <span data-ttu-id="2cf41-108">Mer information om SendGrid och skicka e-post finns i [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2cf41-108">For more information on SendGrid and sending email, see the [Next Steps](#next-steps) section.</span></span>

## <a name="what-is-the-sendgrid-email-service"></a><span data-ttu-id="2cf41-109">Vad är SendGrid e-posttjänst?</span><span class="sxs-lookup"><span data-stu-id="2cf41-109">What is the SendGrid Email Service?</span></span>
<span data-ttu-id="2cf41-110">SendGrid är en [molnbaserade e-posttjänst] som ger tillförlitliga [transaktionella e-postleverans], skalbarhet och analys i realtid tillsammans med flexibel API: er som gör det enkelt anpassad integrering.</span><span class="sxs-lookup"><span data-stu-id="2cf41-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="2cf41-111">Vanliga Användningsscenarier för SendGrid är:</span><span class="sxs-lookup"><span data-stu-id="2cf41-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="2cf41-112">Skicka automatiskt kvitton till kunder</span><span class="sxs-lookup"><span data-stu-id="2cf41-112">Automatically sending receipts to customers</span></span>
* <span data-ttu-id="2cf41-113">Administrera distribution visas för att skicka kunder månatliga e-reklamblad och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="2cf41-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="2cf41-114">Samla in realtid mätvärden för sådant som blockerade e-post och kunden svarstider</span><span class="sxs-lookup"><span data-stu-id="2cf41-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="2cf41-115">Generera rapporter för att identifiera trender</span><span class="sxs-lookup"><span data-stu-id="2cf41-115">Generating reports to help identify trends</span></span>
* <span data-ttu-id="2cf41-116">Vidarebefordran av kundfrågor</span><span class="sxs-lookup"><span data-stu-id="2cf41-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="2cf41-117">E-postaviseringar från ditt program</span><span class="sxs-lookup"><span data-stu-id="2cf41-117">Email notifications from your application</span></span>

<span data-ttu-id="2cf41-118">Mer information finns i [https://sendgrid.com][https://sendgrid.com].</span><span class="sxs-lookup"><span data-stu-id="2cf41-118">For more information, see [https://sendgrid.com][https://sendgrid.com].</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="2cf41-119">Skapa ett SendGrid-konto</span><span class="sxs-lookup"><span data-stu-id="2cf41-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a><span data-ttu-id="2cf41-120">Med SendGrid från PHP-program</span><span class="sxs-lookup"><span data-stu-id="2cf41-120">Using SendGrid from your PHP Application</span></span>
<span data-ttu-id="2cf41-121">Med SendGrid i ett Azure PHP-program kräver ingen särskild konfiguration eller kodning.</span><span class="sxs-lookup"><span data-stu-id="2cf41-121">Using SendGrid in an Azure PHP application requires no special configuration or coding.</span></span> <span data-ttu-id="2cf41-122">Eftersom SendGrid är en tjänst, kan den användas på exakt samma sätt från ett moln-program som går från ett lokalt program.</span><span class="sxs-lookup"><span data-stu-id="2cf41-122">Because SendGrid is a service, it can be accessed in exactly the same way from a cloud application as it can from an on-premises application.</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="2cf41-123">Så här: skicka ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="2cf41-123">How to: Send an Email</span></span>
<span data-ttu-id="2cf41-124">Du kan skicka e-post med SMTP- eller webb-API som tillhandahålls av SendGrid.</span><span class="sxs-lookup"><span data-stu-id="2cf41-124">You can send email using either SMTP or the Web API provided by SendGrid.</span></span>

### <a name="smtp-api"></a><span data-ttu-id="2cf41-125">SMTP-API</span><span class="sxs-lookup"><span data-stu-id="2cf41-125">SMTP API</span></span>
<span data-ttu-id="2cf41-126">Om du vill skicka e-post med SendGrid SMTP-API, Använd *Swift adresshuvud*, ett komponentbaserade bibliotek för att skicka e-post från PHP-program.</span><span class="sxs-lookup"><span data-stu-id="2cf41-126">To send email using the SendGrid SMTP API, use *Swift Mailer*, a component-based library for sending emails from PHP applications.</span></span> <span data-ttu-id="2cf41-127">Du kan hämta den *Swift adresshuvud* bibliotek från [http://swiftmailer.org/download] [ http://swiftmailer.org/download] v5.3.0 (använda [Composer] installera Swift Adresshuvud).</span><span class="sxs-lookup"><span data-stu-id="2cf41-127">You can download the *Swift Mailer* library from [http://swiftmailer.org/download][http://swiftmailer.org/download] v5.3.0 (use [Composer] to install Swift Mailer).</span></span> <span data-ttu-id="2cf41-128">Skicka e-post med biblioteket innebär att du skapar instanser av den <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_adresshuvud</span>, och <span class="auto-style2">Swift\_meddelande </span> klasser, ange lämpliga egenskaper och anropa den <span class="auto-style2">Swift\_Mailer::send</span> metod.</span><span class="sxs-lookup"><span data-stu-id="2cf41-128">Sending email with the library involves creating instances of the <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_Mailer</span>, and <span class="auto-style2">Swift\_Message</span> classes, setting the appropriate properties, and calling the <span class="auto-style2">Swift\_Mailer::send</span> method.</span></span>

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the receiver is able to view html emails then only the html
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
     $from = array('someone@example.com' => 'Name To Appear');
     // Email recipients
     $to = array(
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

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');

     // send message
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a><span data-ttu-id="2cf41-129">Webb-API</span><span class="sxs-lookup"><span data-stu-id="2cf41-129">Web API</span></span>
<span data-ttu-id="2cf41-130">Använd PHP'S [curl funktionen] [ curl function] att skicka e-post med SendGrid Web API.</span><span class="sxs-lookup"><span data-stu-id="2cf41-130">Use PHP's [curl function][curl function] to send email using the SendGrid Web API.</span></span>

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

     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);

     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

     // obtain response
     $response = curl_exec($session);
     curl_close($session);

     // print everything out
     print_r($response);

<span data-ttu-id="2cf41-131">Sendgrid's Web API är mycket likt ett REST-API, om den inte verkligen en RESTful-API eftersom i de flesta anrop, både få och POST verb är utbytbara.</span><span class="sxs-lookup"><span data-stu-id="2cf41-131">SendGrid's Web API is very similar to a REST API, though it is not truly a RESTful API since, in most calls, both GET and POST verbs can be used interchangeably.</span></span>

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="2cf41-132">Så här: Lägg till en bifogad fil</span><span class="sxs-lookup"><span data-stu-id="2cf41-132">How to: Add an Attachment</span></span>
### <a name="smtp-api"></a><span data-ttu-id="2cf41-133">SMTP-API</span><span class="sxs-lookup"><span data-stu-id="2cf41-133">SMTP API</span></span>
<span data-ttu-id="2cf41-134">En ytterligare kodrad till exempelskript för att skicka ett e-postmeddelande med Swift adresshuvud innebär att skicka bifogade filer med hjälp av SMTP-API.</span><span class="sxs-lookup"><span data-stu-id="2cf41-134">Sending an attachment using the SMTP API involves one additional line of code to the example script for sending an email with Swift Mailer.</span></span>

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
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
     $from = array('someone@example.com' => 'Name To Appear');

     // Email recipients
     $to = array(
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

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));

     // send message
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

<span data-ttu-id="2cf41-135">Ytterligare kodrad är följande:</span><span class="sxs-lookup"><span data-stu-id="2cf41-135">The additional line of code is as follows:</span></span>

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

<span data-ttu-id="2cf41-136">Kod anropar metoden bifoga på den <span class="auto-style2">Swift\_meddelandet</span> objekt och använder statisk metod <span class="auto-style2">fromPath</span> på den <span class="auto-style2">Swift\_bilaga</span> klass för att få och bifoga en fil i ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="2cf41-136">This line of code calls the attach method on the <span class="auto-style2">Swift\_Message</span> object and uses static method <span class="auto-style2">fromPath</span> on the <span class="auto-style2">Swift\_Attachment</span> class to get and attach a file to a message.</span></span>

### <a name="web-api"></a><span data-ttu-id="2cf41-137">Webb-API</span><span class="sxs-lookup"><span data-stu-id="2cf41-137">Web API</span></span>
<span data-ttu-id="2cf41-138">Skicka bifogade filer med hjälp av Web API liknar e-post med hjälp av Web-API.</span><span class="sxs-lookup"><span data-stu-id="2cf41-138">Sending an attachment using the Web API is very similar to sending an email using the Web API.</span></span> <span data-ttu-id="2cf41-139">Observera dock att parametermatris i följande exempel måste innehålla det här elementet:</span><span class="sxs-lookup"><span data-stu-id="2cf41-139">However, note that in the example that follows, the parameter array must contain this element:</span></span>

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

<span data-ttu-id="2cf41-140">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2cf41-140">Example:</span></span>

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
         'html' => '<p> the HTML </p>',
         'text' => 'the plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );

     print_r($params);

     $request = $url.'api/mail.send.json';

     // Generate curl request
     $session = curl_init($request);

     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);

     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

     // obtain response
     $response = curl_exec($session);
     curl_close($session);

     // print everything out
     print_r($response);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a><span data-ttu-id="2cf41-141">Så här: använda filter för att aktivera sidfötter, spårning och analys</span><span class="sxs-lookup"><span data-stu-id="2cf41-141">How to: Use Filters to Enable Footers, Tracking, and Analytics</span></span>
<span data-ttu-id="2cf41-142">SendGrid ger ytterligare e-postfunktioner genom att använda ”filter”.</span><span class="sxs-lookup"><span data-stu-id="2cf41-142">SendGrid provides additional email functionality through the use of 'filters'.</span></span> <span data-ttu-id="2cf41-143">Dessa finns inställningar som du kan lägga till ett e-postmeddelande för att aktivera vissa funktioner, till exempel aktivera Klicka spårning, Google analytics, prenumeration spårning och så vidare.</span><span class="sxs-lookup"><span data-stu-id="2cf41-143">These are settings that can be added to an email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span>

<span data-ttu-id="2cf41-144">Filter kan tillämpas på ett meddelande med hjälp av egenskapen filter.</span><span class="sxs-lookup"><span data-stu-id="2cf41-144">Filters can be applied to a message by using the filters property.</span></span> <span data-ttu-id="2cf41-145">Varje filter som anges av ett hash-värde som innehåller filter-specifika inställningar.</span><span class="sxs-lookup"><span data-stu-id="2cf41-145">Each filter is specified by a hash containing filter-specific settings.</span></span> <span data-ttu-id="2cf41-146">I följande exempel aktiverar sidfot filter och anger ett textmeddelande som läggs till längst ned i e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="2cf41-146">The following example enables the footer filter and specifies a text message that will be appended to the bottom of the email message.</span></span>
<span data-ttu-id="2cf41-147">Det här exemplet ska vi använda [sendgrid php biblioteket].</span><span class="sxs-lookup"><span data-stu-id="2cf41-147">For this example we will use [sendgrid-php library].</span></span>
<span data-ttu-id="2cf41-148">Använd [Composer] installera biblioteket:</span><span class="sxs-lookup"><span data-stu-id="2cf41-148">Use [Composer] to install library:</span></span>

    php composer.phar require sendgrid/sendgrid 2.1.1

<span data-ttu-id="2cf41-149">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2cf41-149">Example:</span></span>    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // The list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request to SendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify the names of the recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of the above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have
     // enabled the footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // The subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy the user here. If
     // a sender list IS specified above, this email address becomes irrelevant.
     $to = 'john@contoso.com';

     # Create the body of the message (a plain-text and an HTML version).
     # text is your plain-text email
     # html is your html version of the email
     # if the receiver is able to view html emails then only the html
     # email will be displayed

     /*
      * Note the variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html>
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     to call you at -time- EST to discuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach the body of the email
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

## <a name="next-steps"></a><span data-ttu-id="2cf41-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2cf41-150">Next Steps</span></span>
<span data-ttu-id="2cf41-151">Nu när du har lärt dig grunderna om tjänsten SendGrid e-post, kan du följa dessa länkar om du vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="2cf41-151">Now that you've learned the basics of the SendGrid Email service, follow these links to learn more.</span></span>

* <span data-ttu-id="2cf41-152">SendGrid-dokumentation: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="2cf41-152">SendGrid documentation: <https://sendgrid.com/docs></span></span>
* <span data-ttu-id="2cf41-153">SendGrid PHP-bibliotek: <https://github.com/sendgrid/sendgrid-php></span><span class="sxs-lookup"><span data-stu-id="2cf41-153">SendGrid PHP library: <https://github.com/sendgrid/sendgrid-php></span></span>
* <span data-ttu-id="2cf41-154">SendGrid specialerbjudande för Azure-kunder: <https://sendgrid.com/windowsazure.html></span><span class="sxs-lookup"><span data-stu-id="2cf41-154">SendGrid special offer for Azure customers: <https://sendgrid.com/windowsazure.html></span></span>

<span data-ttu-id="2cf41-155">Mer information finns också i [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="2cf41-155">For more information, see also the [PHP Developer Center](/develop/php/).</span></span>

[https://sendgrid.com]: https://sendgrid.com
[https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
[special offer]: https://www.sendgrid.com/windowsazure.html
[Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
[http://swiftmailer.org/download]: http://swiftmailer.org/download
[curl function]: http://php.net/curl
<span data-ttu-id="2cf41-156">[molnbaserade e-posttjänst]: https://sendgrid.com/email-solutions</span><span class="sxs-lookup"><span data-stu-id="2cf41-156">[cloud-based email service]: https://sendgrid.com/email-solutions</span></span>
<span data-ttu-id="2cf41-157">[transaktionella e-postleverans]: https://sendgrid.com/transactional-email</span><span class="sxs-lookup"><span data-stu-id="2cf41-157">[transactional email delivery]: https://sendgrid.com/transactional-email</span></span>
<span data-ttu-id="2cf41-158">[sendgrid php biblioteket]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1</span><span class="sxs-lookup"><span data-stu-id="2cf41-158">[sendgrid-php library]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1</span></span>
<span data-ttu-id="2cf41-159">[Composer]: https://getcomposer.org/download/</span><span class="sxs-lookup"><span data-stu-id="2cf41-159">[Composer]: https://getcomposer.org/download/</span></span>
