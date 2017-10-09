---
title: aaaUnderstand Azure IoT Hub slutpunkter | Microsoft Docs
description: "Utvecklarhandbok - referensinformation om IoT-hubb riktade enheten och tjänsten riktade slutpunkter."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 57ba52ae-19c6-43e4-bc6c-d8a5c2476e95
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 8647f15d2f2a050ad5799ea82f4d2d46db0dbec1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reference---iot-hub-endpoints"></a>Referens - slutpunkter för IoT-hubb

## <a name="iot-hub-names"></a>IoT-hubb namn

Du kan hitta hello namnet på hello IoT-hubb som är värd för dina slutpunkter i hello portal på hello **översikt** bladet. Som standard hello DNS-namnet för en IoT-hubb ser ut: `{your iot hub name}.azure-devices.net`.

Du kan använda Azure DNS toocreate en anpassad DNS-namn för din IoT-hubb. Mer information finns i [Använd Azure DNS tooprovide anpassad domän-inställningarna för en Azure-tjänst](../dns/dns-custom-domain.md#azure-iot).

## <a name="list-of-built-in-iot-hub-endpoints"></a>Lista över inbyggda IoT-hubb-slutpunkter

Azure IoT Hub är en tjänst med flera klienter som visar dess funktioner toovarious aktörer. hello följande diagram visar hello olika slutpunkter som visar IoT-hubb.

![IoT Hub-slutpunkter][img-endpoints]

hello följande lista beskrivs hello slutpunkter:

* **Resursprovidern**. Hej IoT-hubb resursprovidern Exponerar en [Azure Resource Manager] [ lnk-arm] gränssnitt. Det här gränssnittet aktiverar Azure-prenumeration ägare toocreate och ta bort IoT-hubbar och tooupdate IoT-hubb egenskaper. IoT-hubb egenskaper styr [hubb nivå säkerhetsprinciper][lnk-accesscontrol], i motsats toodevice resursnivå och funktionella alternativ för moln till enhet och enheten till molnet. Hej resursprovidern för IoT-hubb kan du också för[exportera enheten identiteter][lnk-importexport].
* **Enheten Identitetshantering**. Varje IoT-hubb uppvisar en uppsättning HTTP-REST-slutpunkter toomanage enheten identiteter (skapa, hämta, uppdatera och ta bort). [Enheten identiteter] [ lnk-device-identities] används för autentisering och åtkomstkontroll för enheten.
* **Dubbla enhetshantering**. Varje IoT-hubb uppvisar en uppsättning service-riktade tooquery för HTTP-REST-slutpunkt och uppdatera [enhet twins] [ lnk-twins] (uppdatering taggar och egenskaper).
* **Jobb management**. Varje IoT-hubb uppvisar en uppsättning service-riktade HTTP-REST endpoint tooquery och hantera [jobb][lnk-jobs].
* **Enheten slutpunkter**. För varje enhet i hello identitetsregistret visar IoT-hubb en uppsättning slutpunkter:

  * *Skicka meddelanden från enhet till moln*. En enhet använder den här slutpunkten för[skicka meddelanden från enhet till moln][lnk-d2c].
  * *Ta emot meddelanden moln till enhet*. En enhet använder den här slutpunkten tooreceive riktade [moln till enhet meddelanden][lnk-c2d].
  * *Initierar filöverföringar*. En enhet använder den här slutpunkten tooreceive ett Azure Storage SAS-URI från IoT-hubb för[överför en fil][lnk-upload].
  * *Hämta och uppdatera egenskaper för enhet dubbla*. En enhet använder den här slutpunkten tooaccess dess [enheten dubbla][lnk-twins]'s egenskaper.
  * *Ta emot begäranden om direkta metoden*. En enhet använder den här slutpunkten toolisten för [direkt metod][lnk-methods]'s begäranden.

    Dessa slutpunkter som exponeras med hjälp av [MQTT v3.1.1][lnk-mqtt], HTTP 1.1 och [AMQP 1.0] [ lnk-amqp] protokoll. AMQP är också tillgänglig via [WebSockets] [ lnk-websockets] på port 443.

    hello enheten twins och metoder slutpunkter är bara tillgängliga när du använder hello [MQTT v3.1.1] [ lnk-mqtt] protokoll.

* **Tjänstens slutpunkter**. Varje IoT-hubb uppvisar en uppsättning slutpunkter för din lösning serverdel toocommunicate med dina enheter. Med ett undantag dessa slutpunkter är bara tillgängliga med hello [AMQP] [ lnk-amqp] protokoll. hello metoden anrop slutpunkten exponeras över hello HTTP-protokollet.
  
  * *Ta emot meddelanden från enhet till moln*. Den här slutpunkten är kompatibel med [Azure Event Hubs][lnk-event-hubs]. En backend tjänst kan använda den tooread hello [meddelanden från enhet till moln] [ lnk-d2c] skickas av dina enheter. Du kan skapa anpassade slutpunkter på din IoT-hubb i tillägg toothis inbyggd slutpunkt.
  * *Skicka meddelanden moln till enhet och ta emot bekräftelser för leverans av*. Dessa slutpunkter aktivera din lösning serverdel toosend tillförlitliga [moln till enhet meddelanden][lnk-c2d], och tooreceive hello motsvarande bekräftelser för leverans eller upphör att gälla.
  * *Ta emot meddelanden i filen*. Den här asynkrona slutpunkten kan du tooreceive meddelanden om när dina enheter har överfört en fil. 
  * *Dirigera metodanropet*. Den här slutpunkten kan en backend-tjänst tooinvoke en [direkt metod] [ lnk-methods] på en enhet.
  * *Ta emot operations händelseövervakning*. Den här slutpunkten kan du tooreceive operations övervaka händelser om din IoT hub har konfigurerats tooemit dem. Mer information finns i [IoT-hubb operations övervakning][lnk-operations-mon].

Hej [Azure IoT SDK] [ lnk-sdks] artikeln hello olika sätt tooaccess dessa slutpunkter.

Alla IoT-hubbslutpunkter använder hello [TLS] [ lnk-tls] protokoll och ingen slutpunkt exponeras aldrig på okrypterade/oskyddad kanaler.

## <a name="custom-endpoints"></a>Anpassade slutpunkter

Du kan länka befintliga Azure-tjänster i din prenumeration tooyour IoT-hubb tooact som slutpunkter för routning av meddelanden. Dessa slutpunkter fungerar som slutpunkter och används som sänkor för meddelandevägar. Enheter kan inte skriva direkt toohello ytterligare slutpunkter. toolearn mer om meddelandevägar finns hello developer guide på [skicka och ta emot meddelanden med IoT-hubb][lnk-devguide-messaging].

IoT-hubb stöder för närvarande hello följande Azure-tjänster som ytterligare slutpunkter:

* Händelsehubbar
* Service Bus-köer
* Avsnitt om Service Bus

IoT-hubb behöver skrivåtkomst toothese slutpunkter för routning toowork för meddelandet. Om du konfigurerar dina slutpunkter via hello Azure-portalen, läggs hello behörighet för dig. Kontrollera att du konfigurerar dina tjänster toosupport hello förväntades genomflöde. När du först konfigurera din IoT-lösning kan du behöver toomonitor din ytterligare slutpunkter och gör eventuella ändringar för hello faktiska belastningen.

Om ett meddelande matchar flera vägar som alla pekar toohello samma slutpunkt, IoT-hubb levererar meddelandet toothat slutpunkt bara en gång. Du bör därför inte behöver tooconfigure deduplicering på din Service Bus-kö eller ett ämne. I den partitionerade köer garanterar partition tillhörighet meddelandet ordning.

> [!NOTE]
> Service Bus-köer och ämnen som används som IoT-hubbslutpunkter inte får ha **sessioner** eller **dubblettidentifiering** aktiverat. Om något av dessa alternativ är aktiverade hello ändpunkt som **inte åtkomlig** i hello Azure-portalen.

Hello begränsning hello antalet slutpunkter som du kan lägga till, se [kvoter och begränsning][lnk-devguide-quotas].

## <a name="field-gateways"></a>Fältet gateways

I en IoT-lösningen en *fältet gateway* placeras mellan dina enheter och slutpunkter för din IoT-hubb. Det är normalt sett Stäng tooyour enheter. Dina enheter kommunicerar direkt med hello fältet gateway med hjälp av ett protokoll som stöds av hello-enheter. hello fältet gateway ansluter tooan IoT-hubb slutpunkten med hjälp av ett protokoll som stöds av IoT-hubb. En gateway för fältet kan vara en särskild maskinvaruenhet eller en låg energiförbrukning-dator som kör anpassade gateway-programvaran.

Du kan använda [Azure IoT kant] [ lnk-iot-edge] tooimplement fältet gateway. IoT-Edge erbjuder funktioner, till exempel multiplexering kommunikation från flera enheter på hello samma IoT-hubb-anslutning.

## <a name="next-steps"></a>Nästa steg

Andra referensavsnitten i den här IoT-hubb Utvecklarhandbok inkluderar:

* [IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning][lnk-devguide-query]
* [Kvoter och begränsning][lnk-devguide-quotas]
* [IoT-hubb MQTT stöd][lnk-devguide-mqtt]

[lnk-iot-edge]: https://github.com/Azure/iot-edge

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[lnk-operations-mon]: iot-hub-operations-monitoring.md
