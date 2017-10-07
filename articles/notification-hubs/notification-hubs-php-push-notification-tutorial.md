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
# <a name="how-toouse-notification-hubs-from-php"></a>Hur toouse Notification Hubs från PHP
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Du kan använda alla funktioner i Notification Hubs från Java/PHP/Ruby backend-gränssnittet hello Notification Hub REST enligt beskrivningen i avsnittet om MSDN hello [Notification Hub REST API: er](http://msdn.microsoft.com/library/dn223264.aspx).

I det här avsnittet visar vi hur du:

* Skapa ett REST-klient för Notification Hubs funktionerna i PHP;
* Följ hello [Get igång-kursen](notification-hubs-ios-apple-push-notification-apns-get-started.md) för din mobila plattform väljer, implementera hello backend-delen i PHP.

## <a name="client-interface"></a>Klientgränssnitt
hello huvudsakliga klientgränssnittet kan ge hello samma metoder som är tillgängliga i hello [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx), då kan du toodirectly översätta alla hello självstudier och exempel som är tillgängliga på den här platsen och indatakanalen hello community på hello internet.

Du hittar alla hello kod som är tillgängliga i hello [PHP REST wrapper exempel].

Till exempel toocreate en klient:

    $hub = new NotificationHub("connection string", "hubname");    

toosend en intern iOS-meddelande:

    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Implementering
Om du inte redan gjort det, Följ våra [Get igång-kursen] upp toohello sista avsnittet där du har tooimplement hello serverdel.
Om du vill att du kan också använda hello kod från hello [PHP REST wrapper exempel] och gå direkt toohello [fullständig hello kursen](#complete-tutorial) avsnitt.

Alla hello information tooimplement hittar en fullständig REST-omslutning på [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). I det här avsnittet beskriver vi hello PHP implementering av hello Huvudsteg krävs tooaccess Notification Hub REST-slutpunkter:

1. Parsa anslutningssträngen för hello
2. Generera hello autentiseringstoken
3. Utföra hello HTTP-anrop

### <a name="parse-hello-connection-string"></a>Parsa anslutningssträngen för hello
Här är hello huvudsakliga klassen implementerar hello klienten, vars konstruktor som Parsar hello anslutningssträngen:

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


### <a name="create-security-token"></a>Skapa säkerhetstoken
hello information hello säkerhet token skapas finns [här](http://msdn.microsoft.com/library/dn495627.aspx).
hello följande metod har lagts till toobe toohello **NotificationHub** klasstoken toocreate hello baserat på hello URI för hello aktuell begäran hello extraheras från hello anslutningssträngen.

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

### <a name="send-a-notification"></a>Skicka ett meddelande
Låt oss först definiera en klass som representerar ett meddelande.

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

Den här klassen är en behållare för en intern notification text eller en uppsättning egenskaperna i fall av ett meddelande om mallen och en uppsättning huvuden som innehåller plattformsspecifika egenskaper (till exempel Apple giltighetstid egenskap och WNS och format (intern plattform eller mall) rubriker).

Se toohello [Notification Hub REST API: er dokumentationen](http://msdn.microsoft.com/library/dn495827.aspx) och hello specifika meddelanden plattformar format för alla hello alternativ som är tillgängliga.

Tillsammans med den här klassen, vi kan nu skriva hello skicka Meddelandemetoder inuti hello **NotificationHub** klass.

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

hello ovanstående metoder för att skicka en HTTP POST-begäran toohello /messages slutpunkt i din meddelandehubb med hello rätt brödtext och rubriker toosend hello-meddelande.

## <a name="complete-tutorial"></a>Fullständig hello självstudiekursen
Du kan nu slutföra hello komma igång-kursen genom att skicka hello-meddelande från en PHP-backend.

Initiera Notification Hubs-klienten (ersätta hello anslutning sträng och hubbens namn som finns beskrivet i hello [Get igång-kursen]):

    $hub = new NotificationHub("connection string", "hubname");    

Lägg sedan till hello skicka kod beroende på din mobila målplattform.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store och Windows Phone 8.1 (utan Silverlight)
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 och 8.1 Silverlight
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


### <a name="kindle-fire"></a>Kindle Fire
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

Köra koden PHP ska producera nu ett meddelande som visas på målenheten.

## <a name="next-steps"></a>Nästa steg
I det här avsnittet visar hur toocreate en enkel Java REST-klienten för Notification Hubs. Härifrån kan du:

* Hämta hello fullständig [PHP REST wrapper exempel], som innehåller alla hello koden ovan.
* Fortsätta lära dig mer om Notification Hubs taggning funktionen i hello [bryta nyheter kursen]
* Lär dig mer om att skicka meddelanden tooindividual användare i [meddela användare självstudiekursen]

Mer information finns också hello [PHP Developer Center](/develop/php/).

[PHP REST wrapper exempel]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Get igång-kursen]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/

