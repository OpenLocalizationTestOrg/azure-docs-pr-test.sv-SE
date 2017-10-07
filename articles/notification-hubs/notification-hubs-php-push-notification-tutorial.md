---
title: aaaHow toouse Meddelandehubbar med PHP
description: "Lär dig hur toouse Azure Notification Hubs från PHP backend-."
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
ms.openlocfilehash: 6cd426286a684006a07867fcf44a8ff71be7efa8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-notification-hubs-from-php"></a><span data-ttu-id="780a2-103">Hur toouse Notification Hubs från PHP</span><span class="sxs-lookup"><span data-stu-id="780a2-103">How toouse Notification Hubs from PHP</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="780a2-104">Du kan använda alla funktioner i Notification Hubs från Java/PHP/Ruby backend-gränssnittet hello Notification Hub REST enligt beskrivningen i avsnittet om MSDN hello [Notification Hub REST API: er](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="780a2-104">You can access all Notification Hubs features from a Java/PHP/Ruby back-end using hello Notification Hub REST interface as described in hello MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span>

<span data-ttu-id="780a2-105">I det här avsnittet visar vi hur du:</span><span class="sxs-lookup"><span data-stu-id="780a2-105">In this topic we show how to:</span></span>

* <span data-ttu-id="780a2-106">Skapa ett REST-klient för Notification Hubs funktionerna i PHP;</span><span class="sxs-lookup"><span data-stu-id="780a2-106">Build a REST client for Notification Hubs features in PHP;</span></span>
* <span data-ttu-id="780a2-107">Följ hello [Get igång-kursen](notification-hubs-ios-apple-push-notification-apns-get-started.md) för din mobila plattform väljer, implementera hello backend-delen i PHP.</span><span class="sxs-lookup"><span data-stu-id="780a2-107">Follow hello [Get started tutorial](notification-hubs-ios-apple-push-notification-apns-get-started.md) for your mobile platform of choice, implementing hello back-end portion in PHP.</span></span>

## <a name="client-interface"></a><span data-ttu-id="780a2-108">Klientgränssnitt</span><span class="sxs-lookup"><span data-stu-id="780a2-108">Client interface</span></span>
<span data-ttu-id="780a2-109">hello huvudsakliga klientgränssnittet kan ge hello samma metoder som är tillgängliga i hello [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx), då kan du toodirectly översätta alla hello självstudier och exempel som är tillgängliga på den här platsen och indatakanalen hello community på hello internet.</span><span class="sxs-lookup"><span data-stu-id="780a2-109">hello main client interface can provide hello same methods that are available in hello [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx), this will allow you toodirectly translate all hello tutorials and samples currently available on this site, and contributed by hello community on hello internet.</span></span>

<span data-ttu-id="780a2-110">Du hittar alla hello kod som är tillgängliga i hello [PHP REST wrapper exempel].</span><span class="sxs-lookup"><span data-stu-id="780a2-110">You can find all hello code available in hello [PHP REST wrapper sample].</span></span>

<span data-ttu-id="780a2-111">Till exempel toocreate en klient:</span><span class="sxs-lookup"><span data-stu-id="780a2-111">For example, toocreate a client:</span></span>

    $hub = new NotificationHub("connection string", "hubname");    

<span data-ttu-id="780a2-112">toosend en intern iOS-meddelande:</span><span class="sxs-lookup"><span data-stu-id="780a2-112">toosend an iOS native notification:</span></span>

    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a><span data-ttu-id="780a2-113">Implementering</span><span class="sxs-lookup"><span data-stu-id="780a2-113">Implementation</span></span>
<span data-ttu-id="780a2-114">Om du inte redan gjort det, Följ våra [Get igång-kursen] upp toohello sista avsnittet där du har tooimplement hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="780a2-114">If you did not already, please follow our [Get started tutorial] up toohello last section where you have tooimplement hello back-end.</span></span>
<span data-ttu-id="780a2-115">Om du vill att du kan också använda hello kod från hello [PHP REST wrapper exempel] och gå direkt toohello [fullständig hello kursen](#complete-tutorial) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="780a2-115">Also, if you want you can use hello code from hello [PHP REST wrapper sample] and go directly toohello [Complete hello tutorial](#complete-tutorial) section.</span></span>

<span data-ttu-id="780a2-116">Alla hello information tooimplement hittar en fullständig REST-omslutning på [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span><span class="sxs-lookup"><span data-stu-id="780a2-116">All hello details tooimplement a full REST wrapper can be found on [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span></span> <span data-ttu-id="780a2-117">I det här avsnittet beskriver vi hello PHP implementering av hello Huvudsteg krävs tooaccess Notification Hub REST-slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="780a2-117">In this section we will describe hello PHP implementation of hello main steps required tooaccess Notification Hubs REST endpoints:</span></span>

1. <span data-ttu-id="780a2-118">Parsa anslutningssträngen för hello</span><span class="sxs-lookup"><span data-stu-id="780a2-118">Parse hello connection string</span></span>
2. <span data-ttu-id="780a2-119">Generera hello autentiseringstoken</span><span class="sxs-lookup"><span data-stu-id="780a2-119">Generate hello authorization token</span></span>
3. <span data-ttu-id="780a2-120">Utföra hello HTTP-anrop</span><span class="sxs-lookup"><span data-stu-id="780a2-120">Perform hello HTTP call</span></span>

### <a name="parse-hello-connection-string"></a><span data-ttu-id="780a2-121">Parsa anslutningssträngen för hello</span><span class="sxs-lookup"><span data-stu-id="780a2-121">Parse hello connection string</span></span>
<span data-ttu-id="780a2-122">Här är hello huvudsakliga klassen implementerar hello klienten, vars konstruktor som Parsar hello anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="780a2-122">Here is hello main class implementing hello client, whose constructor that parses hello connection string:</span></span>

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


### <a name="create-security-token"></a><span data-ttu-id="780a2-123">Skapa säkerhetstoken</span><span class="sxs-lookup"><span data-stu-id="780a2-123">Create security token</span></span>
<span data-ttu-id="780a2-124">hello information hello säkerhet token skapas finns [här](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="780a2-124">hello details of hello security token creation are available [here](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
<span data-ttu-id="780a2-125">hello följande metod har lagts till toobe toohello **NotificationHub** klasstoken toocreate hello baserat på hello URI för hello aktuell begäran hello extraheras från hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="780a2-125">hello following method has toobe added toohello **NotificationHub** class toocreate hello token based on hello URI of hello current request and hello credentials extracted from hello connection string.</span></span>

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

### <a name="send-a-notification"></a><span data-ttu-id="780a2-126">Skicka ett meddelande</span><span class="sxs-lookup"><span data-stu-id="780a2-126">Send a notification</span></span>
<span data-ttu-id="780a2-127">Låt oss först definiera en klass som representerar ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="780a2-127">First, let us define a class representing a notification.</span></span>

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

<span data-ttu-id="780a2-128">Den här klassen är en behållare för en intern notification text eller en uppsättning egenskaperna i fall av ett meddelande om mallen och en uppsättning huvuden som innehåller plattformsspecifika egenskaper (till exempel Apple giltighetstid egenskap och WNS och format (intern plattform eller mall) rubriker).</span><span class="sxs-lookup"><span data-stu-id="780a2-128">This class is a container for a native notification body, or a set of properties on case of a template notification, and a set of headers which contains format (native platform or template) and platform-specific properties (like Apple expiration property and WNS headers).</span></span>

<span data-ttu-id="780a2-129">Se toohello [Notification Hub REST API: er dokumentationen](http://msdn.microsoft.com/library/dn495827.aspx) och hello specifika meddelanden plattformar format för alla hello alternativ som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="780a2-129">Please refer toohello [Notification Hubs REST APIs documentation](http://msdn.microsoft.com/library/dn495827.aspx) and hello specific notification platforms' formats for all hello options available.</span></span>

<span data-ttu-id="780a2-130">Tillsammans med den här klassen, vi kan nu skriva hello skicka Meddelandemetoder inuti hello **NotificationHub** klass.</span><span class="sxs-lookup"><span data-stu-id="780a2-130">Armed with this class, we can now write hello send notification methods inside of hello **NotificationHub** class.</span></span>

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

        // Send hello request
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

<span data-ttu-id="780a2-131">hello ovanstående metoder för att skicka en HTTP POST-begäran toohello /messages slutpunkt i din meddelandehubb med hello rätt brödtext och rubriker toosend hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="780a2-131">hello above methods send an HTTP POST request toohello /messages endpoint of your notification hub, with hello correct body and headers toosend hello notification.</span></span>

## <span data-ttu-id="780a2-132"><a name="complete-tutorial"></a>Fullständig hello självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="780a2-132"><a name="complete-tutorial"></a>Complete hello tutorial</span></span>
<span data-ttu-id="780a2-133">Du kan nu slutföra hello komma igång-kursen genom att skicka hello-meddelande från en PHP-backend.</span><span class="sxs-lookup"><span data-stu-id="780a2-133">Now you can complete hello Get Started tutorial by sending hello notification from a PHP back-end.</span></span>

<span data-ttu-id="780a2-134">Initiera Notification Hubs-klienten (ersätta hello anslutning sträng och hubbens namn som finns beskrivet i hello [Get igång-kursen]):</span><span class="sxs-lookup"><span data-stu-id="780a2-134">Initialize your Notification Hubs client (substitute hello connection string and hub name as instructed in hello [Get started tutorial]):</span></span>

    $hub = new NotificationHub("connection string", "hubname");    

<span data-ttu-id="780a2-135">Lägg sedan till hello skicka kod beroende på din mobila målplattform.</span><span class="sxs-lookup"><span data-stu-id="780a2-135">Then add hello send code depending on your target mobile platform.</span></span>

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a><span data-ttu-id="780a2-136">Windows Store och Windows Phone 8.1 (utan Silverlight)</span><span class="sxs-lookup"><span data-stu-id="780a2-136">Windows Store and Windows Phone 8.1 (non-Silverlight)</span></span>
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a><span data-ttu-id="780a2-137">iOS</span><span class="sxs-lookup"><span data-stu-id="780a2-137">iOS</span></span>
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a><span data-ttu-id="780a2-138">Android</span><span class="sxs-lookup"><span data-stu-id="780a2-138">Android</span></span>
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a><span data-ttu-id="780a2-139">Windows Phone 8.0 och 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="780a2-139">Windows Phone 8.0 and 8.1 Silverlight</span></span>
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


### <a name="kindle-fire"></a><span data-ttu-id="780a2-140">Kindle Fire</span><span class="sxs-lookup"><span data-stu-id="780a2-140">Kindle Fire</span></span>
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

<span data-ttu-id="780a2-141">Köra koden PHP ska producera nu ett meddelande som visas på målenheten.</span><span class="sxs-lookup"><span data-stu-id="780a2-141">Running your PHP code should produce now a notification appearing on your target device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="780a2-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="780a2-142">Next Steps</span></span>
<span data-ttu-id="780a2-143">I det här avsnittet visar hur toocreate en enkel Java REST-klienten för Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="780a2-143">In this topic we showed how toocreate a simple Java REST client for Notification Hubs.</span></span> <span data-ttu-id="780a2-144">Härifrån kan du:</span><span class="sxs-lookup"><span data-stu-id="780a2-144">From here you can:</span></span>

* <span data-ttu-id="780a2-145">Hämta hello fullständig [PHP REST wrapper exempel], som innehåller alla hello koden ovan.</span><span class="sxs-lookup"><span data-stu-id="780a2-145">Download hello full [PHP REST wrapper sample], which contains all hello code above.</span></span>
* <span data-ttu-id="780a2-146">Fortsätta lära dig mer om Notification Hubs taggning funktionen i hello [bryta nyheter kursen]</span><span class="sxs-lookup"><span data-stu-id="780a2-146">Continue learning about Notification Hubs tagging feature in hello [Breaking News tutorial]</span></span>
* <span data-ttu-id="780a2-147">Lär dig mer om att skicka meddelanden tooindividual användare i [meddela användare självstudiekursen]</span><span class="sxs-lookup"><span data-stu-id="780a2-147">Learn about pushing notifications tooindividual users in [Notify Users tutorial]</span></span>

<span data-ttu-id="780a2-148">Mer information finns också hello [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="780a2-148">For more information, see also hello [PHP Developer Center](/develop/php/).</span></span>

[PHP REST wrapper exempel]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Get igång-kursen]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/

