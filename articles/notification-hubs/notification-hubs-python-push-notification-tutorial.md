---
title: aaaHow toouse Meddelandehubbar med Python
description: "Lär dig hur toouse Azure Notification Hubs från en Python-backend."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 5640dd4a-a91e-4aa0-a833-93615bde49b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: python
ms.devlang: php
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 21d5aaf7fc24c9936fac8e0a8de640c66c51ab0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-notification-hubs-from-python"></a>Hur toouse Notification Hubs från Python
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Du kan använda alla funktioner i Notification Hubs från Java/PHP/Python/Ruby backend-gränssnittet hello Notification Hub REST enligt beskrivningen i avsnittet om MSDN hello [Notification Hub REST API: er](http://msdn.microsoft.com/library/dn223264.aspx).

> [!NOTE]
> Detta är ett exempel på referensimplementering för att implementera hello meddelande skickar i Python och är inte hello stöds officiellt meddelanden hubb Python SDK.
> 
> Det här exemplet har skrivits med Python 3.4.
> 
> 

I det här avsnittet visar vi hur du:

* Skapa ett REST-klient för Meddelandehubbar funktioner i Python.
* Skicka meddelanden med hello Python gränssnittet toohello Notification Hub REST API: er. 
* Hämta en dump av hello RESTEN av HTTP-begäran och svar för felsökning/utbildningssyfte syfte. 

Du kan följa hello [Get igång-kursen](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) för din mobila plattform väljer, implementera hello backend-delen i Python.

> [!NOTE]
> hello omfattning hello prov är endast begränsad toosend meddelanden och den gör inte alla registrering management.
> 
> 

## <a name="client-interface"></a>Klientgränssnitt
hello huvudsakliga klientgränssnittet kan ge hello samma metoder som är tillgängliga i hello [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx). Detta gör att du toodirectly översätta alla hello självstudier och exempel som är tillgängliga på den här platsen och tillhandahålls av hello community på hello internet.

Du hittar alla hello kod som är tillgängliga i hello [Python REST wrapper exempel].

Till exempel toocreate en klient:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

toosend Windows popup meddelande:

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

## <a name="implementation"></a>Implementering
Om du inte redan gjort det, Följ våra [Get igång-kursen] upp toohello sista avsnittet där du har tooimplement hello serverdel.

Alla hello information tooimplement hittar en fullständig REST-omslutning på [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). I det här avsnittet kommer vi beskriver hello Python implementering av hello Huvudsteg krävs tooaccess Notification Hub REST-slutpunkter och skicka meddelanden

1. Parsa anslutningssträngen för hello
2. Generera hello autentiseringstoken
3. Skicka ett meddelande med hjälp av HTTP-REST API

### <a name="parse-hello-connection-string"></a>Parsa anslutningssträngen för hello
Här är hello huvudsakliga klass som implementerar hello-klienten, vars konstruktorn Parsar hello anslutningssträngen:

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"

        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug

            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")

            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Skapa säkerhetstoken
hello information hello säkerhet token skapas finns [här](http://msdn.microsoft.com/library/dn495627.aspx).
hello följande metoder har lagts till toobe toohello **NotificationHub** klasstoken toocreate hello baserat på hello URI för hello aktuell begäran hello extraheras från hello anslutningssträngen.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>Skicka ett meddelande med hjälp av HTTP-REST API
Använd första, kan definiera en klass som representerar ett meddelande.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of hello following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")

            self.format = notification_format
            self.payload = payload

            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Den här klassen är en behållare för interna notification brödtext eller en uppsättning egenskaper vid ett mall-meddelande, en uppsättning huvuden som innehåller format (intern plattform eller mall) och plattformsspecifika egenskaper (till exempel Apple giltighetstid egenskapen och WNS rubriker).

Se toohello [Notification Hub REST API: er dokumentationen](http://msdn.microsoft.com/library/dn495827.aspx) och hello specifika meddelanden plattformar format för alla hello alternativ som är tillgängliga.

Nu med den här klassen vi kan skriva hello skicka Meddelandemetoder inuti hello **NotificationHub** klass.

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about hello PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add hello tags/tag expressions toohello headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers toohello headers collection that hello user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

hello ovanstående metoder för att skicka en HTTP POST-begäran toohello /messages slutpunkt i din meddelandehubb med hello rätt brödtext och rubriker toosend hello-meddelande.

### <a name="using-debug-property-tooenable-detailed-logging"></a>Med hjälp av debug egenskapen tooenable detaljerad loggning
Aktivera felsökning egenskapen vid initiering av hello Notification Hub skrivs ut detaljerad loggningsinformation om hello HTTP begäran och svar dump samt detaljerad meddelande skicka resultatet. Vi nyligen har lagt till den här egenskapen [Notification Hubs TestSend egenskapen](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) som returnerar detaljerad information om hello skicka meddelanderesultat. toouse den - initieras med hello följande:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

hello Notification Hub skicka begäran HTTP-URL hämtar läggas till med ett ”test” querystring därför. 

## <a name="complete-tutorial"></a>Fullständig hello självstudiekursen
Du kan nu slutföra hello komma igång-kursen genom att skicka hello-meddelande från en Python-backend.

Initiera Notification Hubs-klienten (ersätta hello anslutning sträng och hubbens namn som finns beskrivet i hello [Get igång-kursen]):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Lägg sedan till hello skicka kod beroende på din mobila målplattform. Det här exemplet lägger också till den högre nivån metoder tooenable skicka meddelanden utifrån hello plattform t.ex. send_windows_notification för windows. send_apple_notification (för apple) osv. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store och Windows Phone 8.1 (utan Silverlight)
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 och 8.1 Silverlight
    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS
    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

Köra Python-kod ska generera ett meddelande som visas på målenheten.

## <a name="examples"></a>Exempel:
### <a name="enabling-debug-property"></a>Aktivera felsökning egenskapen
När du aktiverar debug flaggan vid initiering av hello NotificationHub visas detaljerad dump för HTTP-begäran och svar samt NotificationOutcome som hello nedan där du kan förstå vilka HTTP-huvuden har skickats i hello begäran och vilka HTTP svar togs emot från hello Meddelandehubben:![][1]

Visas detaljerad Notification Hub resultatet t.ex. 

* hello-meddelande skickas när har toohello Push Notification Service. 
  
        <Outcome>hello Notification was successfully sent toohello Push Notification System</Outcome>
* Om det fanns inga mål hittades för push-meddelanden och sedan du förmodligen kommer toosee hello följande hello svar (vilket betyder att det inte fanns registreringar hitta toodeliver hello-meddelande förmodligen eftersom hello registreringar hade några Felmatchade taggar)
  
        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-toowindows"></a>Broadcast-tooWindows för popup-meddelande
Observera hello-huvuden som skickas ut när du skickar en tooWindows meddelandeklient broadcast popup-meddelande. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Skicka ett meddelande som anger en tagg (eller etikettuttrycket)
Meddelande hello taggar HTTP-huvudet som läggs toohello HTTP-begäran (i hello exemplet nedan vi skickar hello-meddelande endast tooregistrations med ”sport” nyttolast)

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Skicka ett meddelande som anger flera taggar
Observera hur hello taggar HTTP-huvudet ändras när flera taggar skickas. 

    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Mallbaserat meddelande
Observera att hello Format HTTP-huvudet ändringar och hello nyttolast brödtext skickas som en del av texten för hello HTTP-begäran:

**Klientsidans - registrerade mall**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";

**Server-side - skicka hello nyttolast**

        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]

## <a name="next-steps"></a>Nästa steg
I det här avsnittet visar hur toocreate enkel Python REST-klienten för Notification Hubs. Härifrån kan du:

* Hämta hello fullständig [Python REST wrapper exempel], som innehåller alla hello koden ovan.
* Fortsätta lära dig mer om Notification Hubs taggning funktionen i hello [bryter nyheter självstudiekursen]
* Fortsätta lära dig mer om Notification Hubs mallfunktionen i hello [lokalisera nyheter självstudiekursen]

<!-- URLs -->
[Python REST wrapper exempel]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Get igång-kursen]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[bryter nyheter självstudiekursen]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[lokalisera nyheter självstudiekursen]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png

