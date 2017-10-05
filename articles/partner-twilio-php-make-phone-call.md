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
ms.openlocfilehash: f35450ace02727ddf392dbbe857b934a45ee022a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a><span data-ttu-id="5d1b5-104">Hur du gör ett telefonsamtal med Twilio i ett PHP-program på Azure</span><span class="sxs-lookup"><span data-stu-id="5d1b5-104">How to Make a Phone Call Using Twilio in a PHP Application on Azure</span></span>
<span data-ttu-id="5d1b5-105">I följande exempel visas hur du kan använda Twilio ringa ett samtal från en PHP-webbplats finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-105">The following example shows you how you can use Twilio to make a call from a PHP web page hosted in Azure.</span></span> <span data-ttu-id="5d1b5-106">Exempelprogrammet uppmanas användaren för telefonsamtal värden som visas i följande skärmbild visar.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-106">The resulting application will prompt the user for phone call values, as shown in the following screen shot.</span></span>

![Azure anropet formuläret med hjälp av Twilio och PHP][twilio_php]

<span data-ttu-id="5d1b5-108">Du behöver göra följande för att använda koden i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="5d1b5-108">You'll need to do the following to use the code in this topic:</span></span>

1. <span data-ttu-id="5d1b5-109">Skaffa ett Twilio-konto och autentisering-token från din [Twilio konsolen][twilio_console].</span><span class="sxs-lookup"><span data-stu-id="5d1b5-109">Acquire a Twilio account and authentication token from your [Twilio Console][twilio_console].</span></span> <span data-ttu-id="5d1b5-110">Kom igång med Twilio, utvärdera priser på [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="5d1b5-110">To get started with Twilio, evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="5d1b5-111">Du kan registrera dig för ett utvärderingskonto på [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="5d1b5-111">You can sign up for a trial account at [https://www.twilio.com/try-twilio][try_twilio].</span></span>
2. <span data-ttu-id="5d1b5-112">Hämta den [Twilio-biblioteket för PHP](https://github.com/twilio/twilio-php) eller installera den som ett PÄRONTRÄD paket.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-112">Obtain the [Twilio library for PHP](https://github.com/twilio/twilio-php) or install it as a PEAR package.</span></span> <span data-ttu-id="5d1b5-113">Mer information finns i [Readme-filen](https://github.com/twilio/twilio-php/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="5d1b5-113">For more information, see the [readme file](https://github.com/twilio/twilio-php/blob/master/README.md).</span></span>
3. <span data-ttu-id="5d1b5-114">Installera Azure SDK för PHP.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-114">Install the Azure SDK for PHP.</span></span> <span data-ttu-id="5d1b5-115">En översikt över SDK och anvisningar om hur du installerar den finns [ställa in Azure SDK för PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md)</span><span class="sxs-lookup"><span data-stu-id="5d1b5-115">For an overview of the SDK and instructions on installing it, see [Set up the Azure SDK for PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md)</span></span>

## <a name="create-a-web-form-for-making-a-call"></a><span data-ttu-id="5d1b5-116">Skapa ett webbformulär för ett samtal</span><span class="sxs-lookup"><span data-stu-id="5d1b5-116">Create a web form for making a call</span></span>
<span data-ttu-id="5d1b5-117">Följande HTML-kod visar hur du skapar en webbsida (**callform.html**) som hämtar användardata för ett samtal:</span><span class="sxs-lookup"><span data-stu-id="5d1b5-117">The following HTML code shows how to build a web page (**callform.html**) that retrieves user data for making a call:</span></span>

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

## <a name="create-the-code-to-make-the-call"></a><span data-ttu-id="5d1b5-118">Skapa koden för att göra anrop</span><span class="sxs-lookup"><span data-stu-id="5d1b5-118">Create the code to make the call</span></span>
<span data-ttu-id="5d1b5-119">Följande kod visar hur du skapar **makecall.php**, som anropas när användaren skickar formuläret som visas av **callform.html**.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-119">The following code shows how to build **makecall.php**, which is called when the user submits the form displayed by **callform.html**.</span></span> <span data-ttu-id="5d1b5-120">Koden nedan skapar anropet meddelandet och genererar anropet.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-120">The code shown below creates the call message and generates the call.</span></span> <span data-ttu-id="5d1b5-121">Dessutom måste du använda dina Twilio-konto och autentisering-token från den [Twilio konsolen] [ twilio_console] i stället för platshållarvärdena för **$sid** och  **$token** i koden nedan.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-121">Also, be sure to use your Twilio account and authentication token from the [Twilio Console][twilio_console] instead of the placeholder values assigned to **$sid** and **$token** in the code below.</span></span>

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

<span data-ttu-id="5d1b5-122">Förutom att göra anropet **makecall.php** visar vissa anrop metadata, som visas i bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-122">In addition to making the call, **makecall.php** displays some call metadata, as is shown in the image below.</span></span> <span data-ttu-id="5d1b5-123">Mer information om anropet metadata finns [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].</span><span class="sxs-lookup"><span data-stu-id="5d1b5-123">For more information about call metadata, see [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].</span></span>

![Azure anropet svaret med hjälp av Twilio och PHP][twilio_php_response]

## <a name="run-the-application"></a><span data-ttu-id="5d1b5-125">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="5d1b5-125">Run the application</span></span>
<span data-ttu-id="5d1b5-126">Nästa steg är att distribuera programmet till Azure Websites.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-126">The next step is to deploy your application to Azure Websites.</span></span> <span data-ttu-id="5d1b5-127">I följande artiklar innehåller information för att skapa en webbplats och distribuera din kod med Git, FTP eller WebMatrix (om inte all information i varje artikel relevanta):</span><span class="sxs-lookup"><span data-stu-id="5d1b5-127">The following articles contain the information for creating a website and deploying your code with Git, FTP, or WebMatrix (though not all information in each article is relevant):</span></span>

* [<span data-ttu-id="5d1b5-128">Skapa en PHP-MySQL Azure-webbplats och distribuera med Git</span><span class="sxs-lookup"><span data-stu-id="5d1b5-128">Create a PHP-MySQL Azure Web Site and deploy using Git</span></span>](app-service-web/web-sites-php-mysql-deploy-use-git.md)
* [<span data-ttu-id="5d1b5-129">Skapa en PHP-MySQL Azure-webbplats och distribuera med FTP</span><span class="sxs-lookup"><span data-stu-id="5d1b5-129">Create a PHP-MySQL Azure Web Site and Deploy Using FTP</span></span>](app-service-web/web-sites-php-mysql-deploy-use-ftp.md)

## <a name="next-steps"></a><span data-ttu-id="5d1b5-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5d1b5-130">Next steps</span></span>
<span data-ttu-id="5d1b5-131">Den här koden har angetts för att visa grundläggande funktioner med Twilio i PHP i Azure.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-131">This code was provided to show you basic functionality using Twilio in PHP on Azure.</span></span> <span data-ttu-id="5d1b5-132">Innan du distribuerar till Azure i produktion, kanske du vill lägga till flera felhantering eller andra funktioner.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-132">Before deploying to Azure in production, you may want to add more error handling or other features.</span></span> <span data-ttu-id="5d1b5-133">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5d1b5-133">For example:</span></span>

* <span data-ttu-id="5d1b5-134">Istället för att använda ett webbformulär, kunde du använda Azure storage-blobbar eller SQL-databas för att lagra telefonnummer och anropa text.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-134">Instead of using a web form, you could use Azure storage blobs or SQL Database to store phone numbers and call text.</span></span> <span data-ttu-id="5d1b5-135">Information om hur du använder Azure storage-blobbar i PHP finns [med hjälp av Azure Storage med PHP-program][howto_blob_storage_php].</span><span class="sxs-lookup"><span data-stu-id="5d1b5-135">For information about using Azure storage blobs in PHP, see [Using Azure Storage with PHP Applications][howto_blob_storage_php].</span></span> <span data-ttu-id="5d1b5-136">Information om hur du använder SQL-databas i PHP finns [med hjälp av SQL-databas med PHP-program][howto_sql_azure_php].</span><span class="sxs-lookup"><span data-stu-id="5d1b5-136">For information about using SQL Database in PHP, see [Using SQL Database with PHP Applications][howto_sql_azure_php].</span></span>
* <span data-ttu-id="5d1b5-137">Den **makecall.php** kod använder Twilio-tillhandahållna URL ([http://twimlets.com/message][twimlet_message_url]) att tillhandahålla ett Twilio Markup Language (TwiML)-svar som informerar Twilio hur till Fortsätt med anropet.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-137">The **makecall.php** code uses Twilio-provided URL ([http://twimlets.com/message][twimlet_message_url]) to provide a Twilio Markup Language (TwiML) response that informs Twilio how to proceed with the call.</span></span> <span data-ttu-id="5d1b5-138">Till exempel TwiML som returneras kan innehålla en `<Say>` verb som resulterar i text som talas till mottagaren anrop.</span><span class="sxs-lookup"><span data-stu-id="5d1b5-138">For example, the TwiML that is returned can contain a `<Say>` verb that results in text being spoken to the call recipient.</span></span> <span data-ttu-id="5d1b5-139">I stället för med den angivna Twilio-URL, kan du skapa tjänsten för att svara på Twilios begäran. Mer information finns i [så Använd Twilio för röst- och SMS-funktionerna i PHP][howto_twilio_voice_sms_php].</span><span class="sxs-lookup"><span data-stu-id="5d1b5-139">Instead of using the Twilio-provided URL, you could build your own service to respond to Twilio's request; for more information, see [How to Use Twilio for Voice and SMS Capabilities in PHP][howto_twilio_voice_sms_php].</span></span> <span data-ttu-id="5d1b5-140">Mer information om TwiML kan hittas på [http://www.twilio.com/docs/api/twiml][twiml], och mer information om `<Say>` och andra Twilio-verb kan hittas på [http:// www.twilio.com/docs/API/twiml/Say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="5d1b5-140">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml], and more information about `<Say>` and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>
* <span data-ttu-id="5d1b5-141">Läs Twilio-riktlinjer för säkerhet på [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="5d1b5-141">Read the Twilio security guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>

<span data-ttu-id="5d1b5-142">Mer information om Twilio finns [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="5d1b5-142">For additional information about Twilio, see [https://www.twilio.com/docs][twilio_docs].</span></span>

## <a name="see-also"></a><span data-ttu-id="5d1b5-143">Se även</span><span class="sxs-lookup"><span data-stu-id="5d1b5-143">See Also</span></span>
* [<span data-ttu-id="5d1b5-144">Hur du använder Twilio för röst- och SMS-funktionerna i PHP</span><span class="sxs-lookup"><span data-stu-id="5d1b5-144">How to Use Twilio for Voice and SMS Capabilities in PHP</span></span>](partner-twilio-php-how-to-use-voice-sms.md)

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
