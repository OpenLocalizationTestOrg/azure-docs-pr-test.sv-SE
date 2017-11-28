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
# <a name="how-toouse-notification-hubs-from-python"></a><span data-ttu-id="d2f0c-103">Hur toouse Notification Hubs från Python</span><span class="sxs-lookup"><span data-stu-id="d2f0c-103">How toouse Notification Hubs from Python</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="d2f0c-104">Du kan använda alla funktioner i Notification Hubs från Java/PHP/Python/Ruby backend-gränssnittet hello Notification Hub REST enligt beskrivningen i avsnittet om MSDN hello [Notification Hub REST API: er](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="d2f0c-104">You can access all Notification Hubs features from a Java/PHP/Python/Ruby back-end using hello Notification Hub REST interface as described in hello MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="d2f0c-105">Detta är ett exempel på referensimplementering för att implementera hello meddelande skickar i Python och är inte hello stöds officiellt meddelanden hubb Python SDK.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-105">This is a sample reference implementation for implementing hello notification sends in Python and is not hello officially supported Notifications Hub Python SDK.</span></span>
> 
> <span data-ttu-id="d2f0c-106">Det här exemplet har skrivits med Python 3.4.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-106">This sample is written using Python 3.4.</span></span>
> 
> 

<span data-ttu-id="d2f0c-107">I det här avsnittet visar vi hur du:</span><span class="sxs-lookup"><span data-stu-id="d2f0c-107">In this topic we show how to:</span></span>

* <span data-ttu-id="d2f0c-108">Skapa ett REST-klient för Meddelandehubbar funktioner i Python.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-108">Build a REST client for Notification Hubs features in Python.</span></span>
* <span data-ttu-id="d2f0c-109">Skicka meddelanden med hello Python gränssnittet toohello Notification Hub REST API: er.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-109">Send notifications using hello Python interface toohello Notification Hub REST APIs.</span></span> 
* <span data-ttu-id="d2f0c-110">Hämta en dump av hello RESTEN av HTTP-begäran och svar för felsökning/utbildningssyfte syfte.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-110">Get a dump of hello HTTP REST request/response for debugging/educational purpose.</span></span> 

<span data-ttu-id="d2f0c-111">Du kan följa hello [Get igång-kursen](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) för din mobila plattform väljer, implementera hello backend-delen i Python.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-111">You can follow hello [Get started tutorial](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) for your mobile platform of choice, implementing hello back-end portion in Python.</span></span>

> [!NOTE]
> <span data-ttu-id="d2f0c-112">hello omfattning hello prov är endast begränsad toosend meddelanden och den gör inte alla registrering management.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-112">hello scope of hello sample is only limited toosend notifications and it doesn't do any registration management.</span></span>
> 
> 

## <a name="client-interface"></a><span data-ttu-id="d2f0c-113">Klientgränssnitt</span><span class="sxs-lookup"><span data-stu-id="d2f0c-113">Client interface</span></span>
<span data-ttu-id="d2f0c-114">hello huvudsakliga klientgränssnittet kan ge hello samma metoder som är tillgängliga i hello [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx).</span><span class="sxs-lookup"><span data-stu-id="d2f0c-114">hello main client interface can provide hello same methods that are available in hello [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx).</span></span> <span data-ttu-id="d2f0c-115">Detta gör att du toodirectly översätta alla hello självstudier och exempel som är tillgängliga på den här platsen och tillhandahålls av hello community på hello internet.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-115">This will allow you toodirectly translate all hello tutorials and samples currently available on this site, and contributed by hello community on hello internet.</span></span>

<span data-ttu-id="d2f0c-116">Du hittar alla hello kod som är tillgängliga i hello [Python REST wrapper exempel].</span><span class="sxs-lookup"><span data-stu-id="d2f0c-116">You can find all hello code available in hello [Python REST wrapper sample].</span></span>

<span data-ttu-id="d2f0c-117">Till exempel toocreate en klient:</span><span class="sxs-lookup"><span data-stu-id="d2f0c-117">For example, toocreate a client:</span></span>

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

<span data-ttu-id="d2f0c-118">toosend Windows popup meddelande:</span><span class="sxs-lookup"><span data-stu-id="d2f0c-118">toosend a Windows toast notification:</span></span>

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

## <a name="implementation"></a><span data-ttu-id="d2f0c-119">Implementering</span><span class="sxs-lookup"><span data-stu-id="d2f0c-119">Implementation</span></span>
<span data-ttu-id="d2f0c-120">Om du inte redan gjort det, Följ våra [Get igång-kursen] upp toohello sista avsnittet där du har tooimplement hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-120">If you did not already, please follow our [Get started tutorial] up toohello last section where you have tooimplement hello back-end.</span></span>

<span data-ttu-id="d2f0c-121">Alla hello information tooimplement hittar en fullständig REST-omslutning på [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span><span class="sxs-lookup"><span data-stu-id="d2f0c-121">All hello details tooimplement a full REST wrapper can be found on [MSDN](http://msdn.microsoft.com/library/dn530746.aspx).</span></span> <span data-ttu-id="d2f0c-122">I det här avsnittet kommer vi beskriver hello Python implementering av hello Huvudsteg krävs tooaccess Notification Hub REST-slutpunkter och skicka meddelanden</span><span class="sxs-lookup"><span data-stu-id="d2f0c-122">In this section we will describe hello Python implementation of hello main steps required tooaccess Notification Hubs REST endpoints and send notifications</span></span>

1. <span data-ttu-id="d2f0c-123">Parsa anslutningssträngen för hello</span><span class="sxs-lookup"><span data-stu-id="d2f0c-123">Parse hello connection string</span></span>
2. <span data-ttu-id="d2f0c-124">Generera hello autentiseringstoken</span><span class="sxs-lookup"><span data-stu-id="d2f0c-124">Generate hello authorization token</span></span>
3. <span data-ttu-id="d2f0c-125">Skicka ett meddelande med hjälp av HTTP-REST API</span><span class="sxs-lookup"><span data-stu-id="d2f0c-125">Send a notification using HTTP REST API</span></span>

### <a name="parse-hello-connection-string"></a><span data-ttu-id="d2f0c-126">Parsa anslutningssträngen för hello</span><span class="sxs-lookup"><span data-stu-id="d2f0c-126">Parse hello connection string</span></span>
<span data-ttu-id="d2f0c-127">Här är hello huvudsakliga klass som implementerar hello-klienten, vars konstruktorn Parsar hello anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="d2f0c-127">Here is hello main class implementing hello client, whose constructor parses hello connection string:</span></span>

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


### <a name="create-security-token"></a><span data-ttu-id="d2f0c-128">Skapa säkerhetstoken</span><span class="sxs-lookup"><span data-stu-id="d2f0c-128">Create security token</span></span>
<span data-ttu-id="d2f0c-129">hello information hello säkerhet token skapas finns [här](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="d2f0c-129">hello details of hello security token creation are available [here](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
<span data-ttu-id="d2f0c-130">hello följande metoder har lagts till toobe toohello **NotificationHub** klasstoken toocreate hello baserat på hello URI för hello aktuell begäran hello extraheras från hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-130">hello following methods have toobe added toohello **NotificationHub** class toocreate hello token based on hello URI of hello current request and hello credentials extracted from hello connection string.</span></span>

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

### <a name="send-a-notification-using-http-rest-api"></a><span data-ttu-id="d2f0c-131">Skicka ett meddelande med hjälp av HTTP-REST API</span><span class="sxs-lookup"><span data-stu-id="d2f0c-131">Send a notification using HTTP REST API</span></span>
<span data-ttu-id="d2f0c-132">Använd första, kan definiera en klass som representerar ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-132">First, let use define a class representing a notification.</span></span>

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

<span data-ttu-id="d2f0c-133">Den här klassen är en behållare för interna notification brödtext eller en uppsättning egenskaper vid ett mall-meddelande, en uppsättning huvuden som innehåller format (intern plattform eller mall) och plattformsspecifika egenskaper (till exempel Apple giltighetstid egenskapen och WNS rubriker).</span><span class="sxs-lookup"><span data-stu-id="d2f0c-133">This class is a container for a native notification body or a set of properties in case of a template notification, a set of headers which contains format (native platform or template) and platform-specific properties (like Apple expiration property and WNS headers).</span></span>

<span data-ttu-id="d2f0c-134">Se toohello [Notification Hub REST API: er dokumentationen](http://msdn.microsoft.com/library/dn495827.aspx) och hello specifika meddelanden plattformar format för alla hello alternativ som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-134">Please refer toohello [Notification Hubs REST APIs documentation](http://msdn.microsoft.com/library/dn495827.aspx) and hello specific notification platforms' formats for all hello options available.</span></span>

<span data-ttu-id="d2f0c-135">Nu med den här klassen vi kan skriva hello skicka Meddelandemetoder inuti hello **NotificationHub** klass.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-135">Now with this class, we can write hello send notification methods inside of hello **NotificationHub** class.</span></span>

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

<span data-ttu-id="d2f0c-136">hello ovanstående metoder för att skicka en HTTP POST-begäran toohello /messages slutpunkt i din meddelandehubb med hello rätt brödtext och rubriker toosend hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-136">hello above methods send an HTTP POST request toohello /messages endpoint of your notification hub, with hello correct body and headers toosend hello notification.</span></span>

### <a name="using-debug-property-tooenable-detailed-logging"></a><span data-ttu-id="d2f0c-137">Med hjälp av debug egenskapen tooenable detaljerad loggning</span><span class="sxs-lookup"><span data-stu-id="d2f0c-137">Using debug property tooenable detailed logging</span></span>
<span data-ttu-id="d2f0c-138">Aktivera felsökning egenskapen vid initiering av hello Notification Hub skrivs ut detaljerad loggningsinformation om hello HTTP begäran och svar dump samt detaljerad meddelande skicka resultatet.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-138">Enabling debug property while initializing hello Notification Hub will write out detailed logging information about hello HTTP request and response dump as well as detailed Notification message send outcome.</span></span> <span data-ttu-id="d2f0c-139">Vi nyligen har lagt till den här egenskapen [Notification Hubs TestSend egenskapen](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) som returnerar detaljerad information om hello skicka meddelanderesultat.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-139">We recently added this property called [Notification Hubs TestSend property](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) which returns detailed information about hello notification send outcome.</span></span> <span data-ttu-id="d2f0c-140">toouse den - initieras med hello följande:</span><span class="sxs-lookup"><span data-stu-id="d2f0c-140">toouse it - initialize using hello following:</span></span>

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

<span data-ttu-id="d2f0c-141">hello Notification Hub skicka begäran HTTP-URL hämtar läggas till med ett ”test” querystring därför.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-141">hello Notification Hub Send request HTTP URL gets appended with a "test" querystring as a result.</span></span> 

## <span data-ttu-id="d2f0c-142"><a name="complete-tutorial"></a>Fullständig hello självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="d2f0c-142"><a name="complete-tutorial"></a>Complete hello tutorial</span></span>
<span data-ttu-id="d2f0c-143">Du kan nu slutföra hello komma igång-kursen genom att skicka hello-meddelande från en Python-backend.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-143">Now you can complete hello Get Started tutorial by sending hello notification from a Python back-end.</span></span>

<span data-ttu-id="d2f0c-144">Initiera Notification Hubs-klienten (ersätta hello anslutning sträng och hubbens namn som finns beskrivet i hello [Get igång-kursen]):</span><span class="sxs-lookup"><span data-stu-id="d2f0c-144">Initialize your Notification Hubs client (substitute hello connection string and hub name as instructed in hello [Get started tutorial]):</span></span>

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

<span data-ttu-id="d2f0c-145">Lägg sedan till hello skicka kod beroende på din mobila målplattform.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-145">Then add hello send code depending on your target mobile platform.</span></span> <span data-ttu-id="d2f0c-146">Det här exemplet lägger också till den högre nivån metoder tooenable skicka meddelanden utifrån hello plattform t.ex. send_windows_notification för windows. send_apple_notification (för apple) osv.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-146">This sample also adds higher level methods tooenable sending notifications based on hello platform e.g. send_windows_notification for windows; send_apple_notification (for apple) etc.</span></span> 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a><span data-ttu-id="d2f0c-147">Windows Store och Windows Phone 8.1 (utan Silverlight)</span><span class="sxs-lookup"><span data-stu-id="d2f0c-147">Windows Store and Windows Phone 8.1 (non-Silverlight)</span></span>
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a><span data-ttu-id="d2f0c-148">Windows Phone 8.0 och 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="d2f0c-148">Windows Phone 8.0 and 8.1 Silverlight</span></span>
    hub.send_mpns_notification(toast)

### <a name="ios"></a><span data-ttu-id="d2f0c-149">iOS</span><span class="sxs-lookup"><span data-stu-id="d2f0c-149">iOS</span></span>
    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a><span data-ttu-id="d2f0c-150">Android</span><span class="sxs-lookup"><span data-stu-id="d2f0c-150">Android</span></span>
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a><span data-ttu-id="d2f0c-151">Kindle Fire</span><span class="sxs-lookup"><span data-stu-id="d2f0c-151">Kindle Fire</span></span>
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a><span data-ttu-id="d2f0c-152">Baidu</span><span class="sxs-lookup"><span data-stu-id="d2f0c-152">Baidu</span></span>
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

<span data-ttu-id="d2f0c-153">Köra Python-kod ska generera ett meddelande som visas på målenheten.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-153">Running your Python code should produce a notification appearing on your target device.</span></span>

## <a name="examples"></a><span data-ttu-id="d2f0c-154">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d2f0c-154">Examples:</span></span>
### <a name="enabling-debug-property"></a><span data-ttu-id="d2f0c-155">Aktivera felsökning egenskapen</span><span class="sxs-lookup"><span data-stu-id="d2f0c-155">Enabling debug property</span></span>
<span data-ttu-id="d2f0c-156">När du aktiverar debug flaggan vid initiering av hello NotificationHub visas detaljerad dump för HTTP-begäran och svar samt NotificationOutcome som hello nedan där du kan förstå vilka HTTP-huvuden har skickats i hello begäran och vilka HTTP svar togs emot från hello Meddelandehubben:![][1]</span><span class="sxs-lookup"><span data-stu-id="d2f0c-156">When you enable debug flag while initializing hello NotificationHub then you will see detailed HTTP request and response dump as well as NotificationOutcome like hello following where you can understand what HTTP headers are passed in hello request and what HTTP response was received from hello Notification Hub: ![][1]</span></span>

<span data-ttu-id="d2f0c-157">Visas detaljerad Notification Hub resultatet t.ex.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-157">You will see detailed Notification Hub result e.g.</span></span> 

* <span data-ttu-id="d2f0c-158">hello-meddelande skickas när har toohello Push Notification Service.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-158">when hello message is successfully sent toohello Push Notification Service.</span></span> 
  
        <Outcome>hello Notification was successfully sent toohello Push Notification System</Outcome>
* <span data-ttu-id="d2f0c-159">Om det fanns inga mål hittades för push-meddelanden och sedan du förmodligen kommer toosee hello följande hello svar (vilket betyder att det inte fanns registreringar hitta toodeliver hello-meddelande förmodligen eftersom hello registreringar hade några Felmatchade taggar)</span><span class="sxs-lookup"><span data-stu-id="d2f0c-159">If there were no targets found for any push notification then you are likely going toosee hello following in hello response (which indicates that there were no registrations found toodeliver hello notification probably because hello registrations had some mismatched tags)</span></span>
  
        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-toowindows"></a><span data-ttu-id="d2f0c-160">Broadcast-tooWindows för popup-meddelande</span><span class="sxs-lookup"><span data-stu-id="d2f0c-160">Broadcast toast notification tooWindows</span></span>
<span data-ttu-id="d2f0c-161">Observera hello-huvuden som skickas ut när du skickar en tooWindows meddelandeklient broadcast popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-161">Notice hello headers that get sent out when you are sending a broadcast toast notification tooWindows client.</span></span> 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a><span data-ttu-id="d2f0c-162">Skicka ett meddelande som anger en tagg (eller etikettuttrycket)</span><span class="sxs-lookup"><span data-stu-id="d2f0c-162">Send notification specifying a tag (or tag expression)</span></span>
<span data-ttu-id="d2f0c-163">Meddelande hello taggar HTTP-huvudet som läggs toohello HTTP-begäran (i hello exemplet nedan vi skickar hello-meddelande endast tooregistrations med ”sport” nyttolast)</span><span class="sxs-lookup"><span data-stu-id="d2f0c-163">Notice hello Tags HTTP header which gets added toohello HTTP request (in hello example below, we are sending hello notification only tooregistrations with 'sports' payload)</span></span>

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a><span data-ttu-id="d2f0c-164">Skicka ett meddelande som anger flera taggar</span><span class="sxs-lookup"><span data-stu-id="d2f0c-164">Send notification specifying multiple tags</span></span>
<span data-ttu-id="d2f0c-165">Observera hur hello taggar HTTP-huvudet ändras när flera taggar skickas.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-165">Notice how hello Tags HTTP header changes when multiple tags are sent.</span></span> 

    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a><span data-ttu-id="d2f0c-166">Mallbaserat meddelande</span><span class="sxs-lookup"><span data-stu-id="d2f0c-166">Templated notification</span></span>
<span data-ttu-id="d2f0c-167">Observera att hello Format HTTP-huvudet ändringar och hello nyttolast brödtext skickas som en del av texten för hello HTTP-begäran:</span><span class="sxs-lookup"><span data-stu-id="d2f0c-167">Notice that hello Format HTTP header changes and hello payload body is sent as part of hello HTTP request body:</span></span>

<span data-ttu-id="d2f0c-168">**Klientsidans - registrerade mall**</span><span class="sxs-lookup"><span data-stu-id="d2f0c-168">**Client side - registered template**</span></span>

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";

<span data-ttu-id="d2f0c-169">**Server-side - skicka hello nyttolast**</span><span class="sxs-lookup"><span data-stu-id="d2f0c-169">**Server side - sending hello payload**</span></span>

        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]

## <a name="next-steps"></a><span data-ttu-id="d2f0c-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2f0c-170">Next Steps</span></span>
<span data-ttu-id="d2f0c-171">I det här avsnittet visar hur toocreate enkel Python REST-klienten för Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-171">In this topic we showed how toocreate a simple Python REST client for Notification Hubs.</span></span> <span data-ttu-id="d2f0c-172">Härifrån kan du:</span><span class="sxs-lookup"><span data-stu-id="d2f0c-172">From here you can:</span></span>

* <span data-ttu-id="d2f0c-173">Hämta hello fullständig [Python REST wrapper exempel], som innehåller alla hello koden ovan.</span><span class="sxs-lookup"><span data-stu-id="d2f0c-173">Download hello full [Python REST wrapper sample], which contains all hello code above.</span></span>
* <span data-ttu-id="d2f0c-174">Fortsätta lära dig mer om Notification Hubs taggning funktionen i hello [bryter nyheter självstudiekursen]</span><span class="sxs-lookup"><span data-stu-id="d2f0c-174">Continue learning about Notification Hubs tagging feature in hello [Breaking News tutorial]</span></span>
* <span data-ttu-id="d2f0c-175">Fortsätta lära dig mer om Notification Hubs mallfunktionen i hello [lokalisera nyheter självstudiekursen]</span><span class="sxs-lookup"><span data-stu-id="d2f0c-175">Continue learning about Notification Hubs Templates feature in hello [Localizing News tutorial]</span></span>

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

