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
# <a name="how-toomake-a-phone-call-using-twilio-in-a-php-application-on-azure"></a><span data-ttu-id="1d4c2-104">Hur tooMake ett telefonsamtal med Twilio i ett PHP-program på Azure</span><span class="sxs-lookup"><span data-stu-id="1d4c2-104">How tooMake a Phone Call Using Twilio in a PHP Application on Azure</span></span>
<span data-ttu-id="1d4c2-105">hello följande exempel visar hur du kan använda Twilio toomake ett samtal från en PHP-webbplats finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-105">hello following example shows you how you can use Twilio toomake a call from a PHP web page hosted in Azure.</span></span> <span data-ttu-id="1d4c2-106">hello resulterande program frågar hello användaren för telefonsamtal värden som visas i följande skärmbild som visar hello.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-106">hello resulting application will prompt hello user for phone call values, as shown in hello following screen shot.</span></span>

![Azure anropet formuläret med hjälp av Twilio och PHP][twilio_php]

<span data-ttu-id="1d4c2-108">Du behöver toodo hello följande toouse hello koden i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="1d4c2-108">You'll need toodo hello following toouse hello code in this topic:</span></span>

1. <span data-ttu-id="1d4c2-109">Skaffa ett Twilio-konto och autentisering-token från din [Twilio konsolen][twilio_console].</span><span class="sxs-lookup"><span data-stu-id="1d4c2-109">Acquire a Twilio account and authentication token from your [Twilio Console][twilio_console].</span></span> <span data-ttu-id="1d4c2-110">tooget igång med Twilio, utvärdera priser på [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="1d4c2-110">tooget started with Twilio, evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="1d4c2-111">Du kan registrera dig för ett utvärderingskonto på [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="1d4c2-111">You can sign up for a trial account at [https://www.twilio.com/try-twilio][try_twilio].</span></span>
2. <span data-ttu-id="1d4c2-112">Hämta hello [Twilio-biblioteket för PHP](https://github.com/twilio/twilio-php) eller installera den som ett PÄRONTRÄD paket.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-112">Obtain hello [Twilio library for PHP](https://github.com/twilio/twilio-php) or install it as a PEAR package.</span></span> <span data-ttu-id="1d4c2-113">Mer information finns i hello [Readme-filen](https://github.com/twilio/twilio-php/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="1d4c2-113">For more information, see hello [readme file](https://github.com/twilio/twilio-php/blob/master/README.md).</span></span>
3. <span data-ttu-id="1d4c2-114">Installera hello Azure SDK för PHP.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-114">Install hello Azure SDK for PHP.</span></span> <span data-ttu-id="1d4c2-115">En översikt över hello SDK och instruktioner om hur du installerar den finns [konfigurera hello Azure SDK för PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md)</span><span class="sxs-lookup"><span data-stu-id="1d4c2-115">For an overview of hello SDK and instructions on installing it, see [Set up hello Azure SDK for PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md)</span></span>

## <a name="create-a-web-form-for-making-a-call"></a><span data-ttu-id="1d4c2-116">Skapa ett webbformulär för ett samtal</span><span class="sxs-lookup"><span data-stu-id="1d4c2-116">Create a web form for making a call</span></span>
<span data-ttu-id="1d4c2-117">hello följande HTML kod visas hur toobuild en webbsida (**callform.html**) som hämtar användardata för ett samtal:</span><span class="sxs-lookup"><span data-stu-id="1d4c2-117">hello following HTML code shows how toobuild a web page (**callform.html**) that retrieves user data for making a call:</span></span>

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

## <a name="create-hello-code-toomake-hello-call"></a><span data-ttu-id="1d4c2-118">Skapa hello kod toomake hello anrop</span><span class="sxs-lookup"><span data-stu-id="1d4c2-118">Create hello code toomake hello call</span></span>
<span data-ttu-id="1d4c2-119">Hej följande kod visar hur toobuild **makecall.php**, som anropas när hello användaren skickar hello formuläret som visas av **callform.html**.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-119">hello following code shows how toobuild **makecall.php**, which is called when hello user submits hello form displayed by **callform.html**.</span></span> <span data-ttu-id="1d4c2-120">hello koden nedan skapar anropet hälsningsmeddelande och genererar hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-120">hello code shown below creates hello call message and generates hello call.</span></span> <span data-ttu-id="1d4c2-121">Dessutom vara säker på att toouse ditt Twilio-konto och autentisering token från hello [Twilio konsolen] [ twilio_console] i stället för hello platshållare för värden som har tilldelats för**$sid** och **$token** i hello koden nedan.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-121">Also, be sure toouse your Twilio account and authentication token from hello [Twilio Console][twilio_console] instead of hello placeholder values assigned too**$sid** and **$token** in hello code below.</span></span>

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

<span data-ttu-id="1d4c2-122">Dessutom toomaking hello-anrop **makecall.php** visar vissa anrop metadata, som visas i hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-122">In addition toomaking hello call, **makecall.php** displays some call metadata, as is shown in hello image below.</span></span> <span data-ttu-id="1d4c2-123">Mer information om anropet metadata finns [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].</span><span class="sxs-lookup"><span data-stu-id="1d4c2-123">For more information about call metadata, see [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].</span></span>

![Azure anropet svaret med hjälp av Twilio och PHP][twilio_php_response]

## <a name="run-hello-application"></a><span data-ttu-id="1d4c2-125">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="1d4c2-125">Run hello application</span></span>
<span data-ttu-id="1d4c2-126">hello nästa steg är toodeploy ditt program tooAzure webbplatser.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-126">hello next step is toodeploy your application tooAzure Websites.</span></span> <span data-ttu-id="1d4c2-127">hello innehåller följande artiklar hello information för att skapa en webbplats och distribuera din kod med Git, FTP eller WebMatrix (om inte all information i varje artikel relevanta):</span><span class="sxs-lookup"><span data-stu-id="1d4c2-127">hello following articles contain hello information for creating a website and deploying your code with Git, FTP, or WebMatrix (though not all information in each article is relevant):</span></span>

* [<span data-ttu-id="1d4c2-128">Skapa en PHP-MySQL Azure-webbplats och distribuera med Git</span><span class="sxs-lookup"><span data-stu-id="1d4c2-128">Create a PHP-MySQL Azure Web Site and deploy using Git</span></span>](app-service-web/web-sites-php-mysql-deploy-use-git.md)
* [<span data-ttu-id="1d4c2-129">Skapa en PHP-MySQL Azure-webbplats och distribuera med FTP</span><span class="sxs-lookup"><span data-stu-id="1d4c2-129">Create a PHP-MySQL Azure Web Site and Deploy Using FTP</span></span>](app-service-web/web-sites-php-mysql-deploy-use-ftp.md)

## <a name="next-steps"></a><span data-ttu-id="1d4c2-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d4c2-130">Next steps</span></span>
<span data-ttu-id="1d4c2-131">Den här koden har angetts tooshow du grundläggande funktioner med Twilio i PHP i Azure.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-131">This code was provided tooshow you basic functionality using Twilio in PHP on Azure.</span></span> <span data-ttu-id="1d4c2-132">Du kanske vill tooadd mer felhantering eller andra funktioner innan du distribuerar tooAzure i produktion.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-132">Before deploying tooAzure in production, you may want tooadd more error handling or other features.</span></span> <span data-ttu-id="1d4c2-133">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1d4c2-133">For example:</span></span>

* <span data-ttu-id="1d4c2-134">Istället för att använda ett webbformulär, kan du använda Azure storage-blobbar eller SQL-databas toostore telefonnummer och anropa text.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-134">Instead of using a web form, you could use Azure storage blobs or SQL Database toostore phone numbers and call text.</span></span> <span data-ttu-id="1d4c2-135">Information om hur du använder Azure storage-blobbar i PHP finns [med hjälp av Azure Storage med PHP-program][howto_blob_storage_php].</span><span class="sxs-lookup"><span data-stu-id="1d4c2-135">For information about using Azure storage blobs in PHP, see [Using Azure Storage with PHP Applications][howto_blob_storage_php].</span></span> <span data-ttu-id="1d4c2-136">Information om hur du använder SQL-databas i PHP finns [med hjälp av SQL-databas med PHP-program][howto_sql_azure_php].</span><span class="sxs-lookup"><span data-stu-id="1d4c2-136">For information about using SQL Database in PHP, see [Using SQL Database with PHP Applications][howto_sql_azure_php].</span></span>
* <span data-ttu-id="1d4c2-137">Hej **makecall.php** kod använder Twilio-tillhandahållna URL ([http://twimlets.com/message][twimlet_message_url]) tooprovide svaret Twilio Markup Language (TwiML) som informerar Twilio hur tooproceed med hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-137">hello **makecall.php** code uses Twilio-provided URL ([http://twimlets.com/message][twimlet_message_url]) tooprovide a Twilio Markup Language (TwiML) response that informs Twilio how tooproceed with hello call.</span></span> <span data-ttu-id="1d4c2-138">Till exempel hello TwiML som returneras kan innehålla en `<Say>` verb som resulterar i texten är talade toohello anrop mottagaren.</span><span class="sxs-lookup"><span data-stu-id="1d4c2-138">For example, hello TwiML that is returned can contain a `<Say>` verb that results in text being spoken toohello call recipient.</span></span> <span data-ttu-id="1d4c2-139">Istället för att använda hello Twilio-tillhandahållna URL kan du bygga egna service toorespond tooTwilio begäran. Mer information finns i [hur tooUse Twilio för röst- och SMS-funktionerna i PHP][howto_twilio_voice_sms_php].</span><span class="sxs-lookup"><span data-stu-id="1d4c2-139">Instead of using hello Twilio-provided URL, you could build your own service toorespond tooTwilio's request; for more information, see [How tooUse Twilio for Voice and SMS Capabilities in PHP][howto_twilio_voice_sms_php].</span></span> <span data-ttu-id="1d4c2-140">Mer information om TwiML kan hittas på [http://www.twilio.com/docs/api/twiml][twiml], och mer information om `<Say>` och andra Twilio-verb kan hittas på [http:// www.twilio.com/docs/API/twiml/Say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="1d4c2-140">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml], and more information about `<Say>` and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>
* <span data-ttu-id="1d4c2-141">Läsa hello Twilio säkerhetsriktlinjer på [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="1d4c2-141">Read hello Twilio security guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>

<span data-ttu-id="1d4c2-142">Mer information om Twilio finns [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="1d4c2-142">For additional information about Twilio, see [https://www.twilio.com/docs][twilio_docs].</span></span>

## <a name="see-also"></a><span data-ttu-id="1d4c2-143">Se även</span><span class="sxs-lookup"><span data-stu-id="1d4c2-143">See Also</span></span>
* [<span data-ttu-id="1d4c2-144">Hur tooUse Twilio för röst- och SMS-funktionerna i PHP</span><span class="sxs-lookup"><span data-stu-id="1d4c2-144">How tooUse Twilio for Voice and SMS Capabilities in PHP</span></span>](partner-twilio-php-how-to-use-voice-sms.md)

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
