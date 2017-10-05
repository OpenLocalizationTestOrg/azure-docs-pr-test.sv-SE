---
title: "Hur du använder Notification Hubs med PHP"
description: "Lär dig hur du använder Azure Notification Hubs från PHP backend."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 0156f994-96d0-4878-b07b-49b7be4fd856
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: php
ms.devlang: php
ms.topic: article
ms.date: 06/07/2016
ms.author: yuaxu
ms.openlocfilehash: c27b6308ff528224a0398e0ff40537db05417bb0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-notification-hubs-from-php"></a><span data-ttu-id="79a70-103">Hur du använder Notification Hubs från PHP</span><span class="sxs-lookup"><span data-stu-id="79a70-103">How to use Notification Hubs from PHP</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="79a70-104">Du kan använda alla funktioner i Notification Hubs från en Java/PHP/Ruby serverdel med Notification Hub REST-gränssnitt som beskrivs i avsnittet MSDN [Notification Hub REST API: er](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="79a70-104">You can access all Notification Hubs features from a Java/PHP/Ruby back-end using the Notification Hub REST interface as described in the MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span>

<span data-ttu-id="79a70-105">I det här avsnittet visar vi hur du:</span><span class="sxs-lookup"><span data-stu-id="79a70-105">In this topic we show how to:</span></span>

* <span data-ttu-id="79a70-106">Skapa ett REST-klient för Notification Hubs funktionerna i PHP;</span><span class="sxs-lookup"><span data-stu-id="79a70-106">Build a REST client for Notification Hubs features in PHP;</span></span>
* <span data-ttu-id="79a70-107">Följ den [Get igång-kursen](notification-hubs-ios-apple-push-notification-apns-get-started.md) för din mobila plattform väljer implementera backend-delen i PHP.</span><span class="sxs-lookup"><span data-stu-id="79a70-107">Follow the [Get started tutorial](notification-hubs-ios-apple-push-notification-apns-get-started.md) for your mobile platform of choice, implementing the back-end portion in PHP.</span></span>

## <a name="client-interface"></a><span data-ttu-id="79a70-108">Klientgränssnitt</span><span class="sxs-lookup"><span data-stu-id="79a70-108">Client interface</span></span>
<span data-ttu-id="79a70-109">Det huvudsakliga klientgränssnittet kan ge samma metoder som är tillgängliga i den [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx), detta kan du direkt översätta alla självstudier och exempel som är tillgängliga på den här platsen och tillhandahålls av gemenskapen på internet.</span><span class="sxs-lookup"><span data-stu-id="79a70-109">The main client interface can provide the same methods that are available in the [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx), this will allow you to directly translate all the tutorials and samples currently available on this site, and contributed by the community on the internet.</span></span>

<span data-ttu-id="79a70-110">Du hittar kod som är tillgängliga i den [PHP REST wrapper exempel].</span><span class="sxs-lookup"><span data-stu-id="79a70-110">You can find all the code available in the [PHP REST wrapper sample].</span></span>

<span data-ttu-id="79a70-111">Till exempel att skapa en klient:</span><span class="sxs-lookup"><span data-stu-id="79a70-111">For example, to create a client:</span></span>

    $hub = new NotificationHub("connection string", "hubname");    

<span data-ttu-id="79a70-112">Att skicka en iOS interna avisering:</span><span class="sxs-lookup"><span data-stu-id="79a70-112">To send an iOS native notification:</span></span>

    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a><span data-ttu-id="79a70-113">Implementering</span><span class="sxs-lookup"><span data-stu-id="79a70-113">Implementation</span></span>
<span data-ttu-id="79a70-114">Om du inte redan gjort det, Följ våra [Get igång-kursen] upp till den senaste avsnitt där du måste implementera serverdel.</span><span class="sxs-lookup"><span data-stu-id="79a70-114">If you did not already, please follow our [Get started tutorial] up to the last section where you have to implement the back-end.</span></span>
<span data-ttu-id="79a70-115">Om du vill att du kan också använda koden från den [PHP REST wrapper exempel] och gå direkt till den [slutföra kursen](#complete-tutorial) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="79a70-115">Also, if you want you can use the code from the [PHP REST wrapper sample] and go directly to the [Complete the tutorial](#complete-tutorial) section.</span></span>

<span data-ttu-id="79a70-116">All information du implementerar en fullständig REST-omslutning finns på [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span><span class="sxs-lookup"><span data-stu-id="79a70-116">All the details to implement a full REST wrapper can be found on [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span></span> <span data-ttu-id="79a70-117">I det här avsnittet beskriver vi PHP-implementeringen av huvudsakliga steg för att komma åt Notification Hub REST-slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="79a70-117">In this section we will describe the PHP implementation of the main steps required to access Notification Hubs REST endpoints:</span></span>

1. <span data-ttu-id="79a70-118">Parsa anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="79a70-118">Parse the connection string</span></span>
2. <span data-ttu-id="79a70-119">Generera autentiseringstoken</span><span class="sxs-lookup"><span data-stu-id="79a70-119">Generate the authorization token</span></span>
3. <span data-ttu-id="79a70-120">Utföra HTTP-anrop</span><span class="sxs-lookup"><span data-stu-id="79a70-120">Perform the HTTP call</span></span>

### <a name="parse-the-connection-string"></a><span data-ttu-id="79a70-121">Parsa anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="79a70-121">Parse the connection string</span></span>
<span data-ttu-id="79a70-122">Här är den viktigaste klass som implementerar klienten, vars konstruktor som Parsar anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="79a70-122">Here is the main class implementing the client, whose constructor that parses the connection string:</span></span>

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";

        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;

        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;

            $this->parseConnectionString($connectionString);
        }

        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }

            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a><span data-ttu-id="79a70-123">Skapa säkerhetstoken</span><span class="sxs-lookup"><span data-stu-id="79a70-123">Create security token</span></span>
<span data-ttu-id="79a70-124">Information om säkerhet token skapa finns [här](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="79a70-124">The details of the security token creation are available [here](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
<span data-ttu-id="79a70-125">Följande metod måste läggas till i **NotificationHub** klassen för att skapa token baserat på URI: N för den aktuella begäranden och de autentiseringsuppgifter som har extraherats från anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="79a70-125">The following method has to be added to the **NotificationHub** class to create the token based on the URI of the current request and the credentials extracted from the connection string.</span></span>

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a><span data-ttu-id="79a70-126">Skicka ett meddelande</span><span class="sxs-lookup"><span data-stu-id="79a70-126">Send a notification</span></span>
<span data-ttu-id="79a70-127">Låt oss först definiera en klass som representerar ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="79a70-127">First, let us define a class representing a notification.</span></span>

    class Notification {
        public $format;
        public $payload;

        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;

        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }

            $this->format = $format;
            $this->payload = $payload;
        }
    }

<span data-ttu-id="79a70-128">Den här klassen är en behållare för en intern notification text eller en uppsättning egenskaperna i fall av ett meddelande om mallen och en uppsättning huvuden som innehåller plattformsspecifika egenskaper (till exempel Apple giltighetstid egenskap och WNS och format (intern plattform eller mall) rubriker).</span><span class="sxs-lookup"><span data-stu-id="79a70-128">This class is a container for a native notification body, or a set of properties on case of a template notification, and a set of headers which contains format (native platform or template) and platform-specific properties (like Apple expiration property and WNS headers).</span></span>

<span data-ttu-id="79a70-129">Mer information finns i [Notification Hub REST API: er dokumentationen](http://msdn.microsoft.com/library/dn495827.aspx) och specifika meddelanden plattformar format för alla tillgängliga alternativ.</span><span class="sxs-lookup"><span data-stu-id="79a70-129">Please refer to the [Notification Hubs REST APIs documentation](http://msdn.microsoft.com/library/dn495827.aspx) and the specific notification platforms' formats for all the options available.</span></span>

<span data-ttu-id="79a70-130">Tillsammans med den här klassen, vi kan nu skriva skicka Meddelandemetoder inuti den **NotificationHub** klass.</span><span class="sxs-lookup"><span data-stu-id="79a70-130">Armed with this class, we can now write the send notification methods inside of the **NotificationHub** class.</span></span>

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }

        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

<span data-ttu-id="79a70-131">Ovanstående metoder skicka en HTTP POST-begäran till slutpunkten /messages för meddelandehubben med rätt brödtext och rubriker för att skicka meddelandet.</span><span class="sxs-lookup"><span data-stu-id="79a70-131">The above methods send an HTTP POST request to the /messages endpoint of your notification hub, with the correct body and headers to send the notification.</span></span>

## <span data-ttu-id="79a70-132"><a name="complete-tutorial"></a>Slutför guiden</span><span class="sxs-lookup"><span data-stu-id="79a70-132"><a name="complete-tutorial"></a>Complete the tutorial</span></span>
<span data-ttu-id="79a70-133">Du kan nu slutföra kursen Kom igång genom att skicka meddelandet från PHP backend.</span><span class="sxs-lookup"><span data-stu-id="79a70-133">Now you can complete the Get Started tutorial by sending the notification from a PHP back-end.</span></span>

<span data-ttu-id="79a70-134">Initiera Notification Hubs-klienten (Ersätt strängen och hubb anslutningsnamn som finns beskrivet i den [Get igång-kursen]):</span><span class="sxs-lookup"><span data-stu-id="79a70-134">Initialize your Notification Hubs client (substitute the connection string and hub name as instructed in the [Get started tutorial]):</span></span>

    $hub = new NotificationHub("connection string", "hubname");    

<span data-ttu-id="79a70-135">Lägg sedan till skicka kod beroende på din mobila målplattform.</span><span class="sxs-lookup"><span data-stu-id="79a70-135">Then add the send code depending on your target mobile platform.</span></span>

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a><span data-ttu-id="79a70-136">Windows Store och Windows Phone 8.1 (utan Silverlight)</span><span class="sxs-lookup"><span data-stu-id="79a70-136">Windows Store and Windows Phone 8.1 (non-Silverlight)</span></span>
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a><span data-ttu-id="79a70-137">iOS</span><span class="sxs-lookup"><span data-stu-id="79a70-137">iOS</span></span>
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a><span data-ttu-id="79a70-138">Android</span><span class="sxs-lookup"><span data-stu-id="79a70-138">Android</span></span>
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a><span data-ttu-id="79a70-139">Windows Phone 8.0 och 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="79a70-139">Windows Phone 8.0 and 8.1 Silverlight</span></span>
    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a><span data-ttu-id="79a70-140">Kindle Fire</span><span class="sxs-lookup"><span data-stu-id="79a70-140">Kindle Fire</span></span>
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

<span data-ttu-id="79a70-141">Köra koden PHP ska producera nu ett meddelande som visas på målenheten.</span><span class="sxs-lookup"><span data-stu-id="79a70-141">Running your PHP code should produce now a notification appearing on your target device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="79a70-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79a70-142">Next Steps</span></span>
<span data-ttu-id="79a70-143">Vi visar hur du skapar en enkel RESTEN av Java-klient för Meddelandehubbar i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="79a70-143">In this topic we showed how to create a simple Java REST client for Notification Hubs.</span></span> <span data-ttu-id="79a70-144">Härifrån kan du:</span><span class="sxs-lookup"><span data-stu-id="79a70-144">From here you can:</span></span>

* <span data-ttu-id="79a70-145">Hämta fullständigt [PHP REST wrapper exempel], som innehåller alla koden ovan.</span><span class="sxs-lookup"><span data-stu-id="79a70-145">Download the full [PHP REST wrapper sample], which contains all the code above.</span></span>
* <span data-ttu-id="79a70-146">Fortsätta lära dig mer om Notification Hubs taggning funktionen i [bryta nyheter självstudiekursen]</span><span class="sxs-lookup"><span data-stu-id="79a70-146">Continue learning about Notification Hubs tagging feature in the [Breaking News tutorial]</span></span>
* <span data-ttu-id="79a70-147">Lär dig mer om push-meddelanden till enskilda användare i [meddela användare självstudiekursen]</span><span class="sxs-lookup"><span data-stu-id="79a70-147">Learn about pushing notifications to individual users in [Notify Users tutorial]</span></span>

<span data-ttu-id="79a70-148">Mer information finns också i [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="79a70-148">For more information, see also the [PHP Developer Center](/develop/php/).</span></span>

<span data-ttu-id="79a70-149">[PHP REST wrapper exempel]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php</span><span class="sxs-lookup"><span data-stu-id="79a70-149">[PHP REST wrapper sample]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php</span></span>
<span data-ttu-id="79a70-150">[Get igång-kursen]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/</span><span class="sxs-lookup"><span data-stu-id="79a70-150">[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/</span></span>

