---
title: "aaaUnderstand stöd för Azure IoT-hubb MQTT | Microsoft Docs"
description: "Utvecklare guide - stöd för enheter som ansluter tooan IoT-hubb enheten riktade slutpunkten med hello MQTT-protokollet. Innehåller information om stöd för inbyggda MQTT i hello SDK för Azure IoT-enhet."
services: iot-hub
documentationcenter: .net
author: kdotchkoff
manager: timlt
editor: 
ms.assetid: 1d71c27c-b466-4a40-b95b-d6550cf85144
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: kdotchko
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e461687963138987acdf1f4e0e34c453744ea191
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="communicate-with-your-iot-hub-using-hello-mqtt-protocol"></a>Kommunicera med din IoT-hubb med hello MQTT-protokollet

IoT-hubb kan enheter toocommunicate med hello IoT-hubb enheten slutpunkter med hello [MQTT v3.1.1] [ lnk-mqtt-org] -protokollet på port 8883 eller MQTT v3.1.1 via WebSocket-protokollet på port 443. IoT-hubb kräver att alla enheter kommunikation toobe kan skyddas med TLS/SSL (därför IoT-hubb stöder inte är säkra anslutningar via port 1883).

## <a name="connecting-tooiot-hub"></a>Ansluta tooIoT Hub

En enhet kan du använda hello MQTT protokollet tooconnect tooan IoT-hubb antingen genom att använda hello bibliotek i hello [Azure IoT SDK] [ lnk-device-sdks] eller via protokollet hello MQTT direkt.

## <a name="using-hello-device-sdks"></a>Använder hello enhet SDK

[Enheten SDK] [ lnk-device-sdks] som stöd hello MQTT protokollet är tillgängliga för Java, Node.js, C, C# och Python. hello SDK enhetsanvändning hello standard IoT-hubb anslutning sträng tooestablish anslutning tooan IoT-hubb. toouse hello MQTT protokollet hello klienten protokollet parametern måste anges för**MQTT**. Som standard ansluter hello enheten SDK tooan IoT-hubb med hello **CleanSession** flaggan för**0** och använda **QoS 1** för utbyte av meddelanden med hello IoT-hubben.

När en enhet är ansluten tooan IoT-hubb kan ange hello enheten SDK metoder som gör att enheten toosend hälsningsmeddelande tooand ta emot meddelanden från en IoT-hubb.

hello följande tabell innehåller länkar toocode prov för varje språk som stöds och anger hello parametern toouse tooestablish en anslutning tooIoT hubb med hello MQTT-protokollet.

| Språk | Protokoll-parameter |
| --- | --- |
| [Node.js][lnk-sample-node] |Azure-iot-enhet – mqtt |
| [Java][lnk-sample-java] |IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] |MQTT_Protocol |
| [C#][lnk-sample-csharp] |TransportType.Mqtt |
| [Python][lnk-sample-python] |IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-toomqtt"></a>Migrera en enhetsapp från AMQP tooMQTT

Om du använder hello [enheten SDK][lnk-device-sdks], växlar från att använda AMQP kräver tooMQTT ändring hello protocol-parametern i hello Klientinitiering som nämnts tidigare anger.

När du gör det gör att toocheck hello följande objekt:

* AMQP returnerar fel för många villkor när MQTT avbryter hello-anslutning. Din undantagshantering logik kan därför kräva några ändringar.
* MQTT stöder inte hello *avvisa* åtgärder när du tar emot [moln till enhet meddelanden][lnk-messaging]. Om backend-app måste tooreceive svar från hello enhetsapp, bör du använda [direkt metoder][lnk-methods].

## <a name="using-hello-mqtt-protocol-directly"></a>Protokollet hello MQTT direkt
Om en enhet inte kan använda hello enheten SDK: er, kan den fortfarande ansluta toohello offentliga enheter slutpunkter med hello MQTT protokollet på port 8883. I hello **Anslut** paket hello enheten ska använda hello följande värden:

* För hello **ClientId** , ska du använda hello **deviceId**.
* För hello **användarnamn** , ska du använda `{iothubhostname}/{device_id}/api-version=2016-11-14`, där {iothubhostname} är hello fullständig CName för hello IoT-hubb.

    Till exempel om hello namnet på din IoT-hubb är **contoso.azure devices.net** och hello namnet på din enhet är **MyDevice01**, hello fullständig **användarnamn** fältet ska innehålla `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.
* För hello **lösenord** , ska du använda en SAS-token. hello format hello SAS-token är hello samma för både hello HTTP och AMQP protokoll:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    Mer information om hur toogenerate SAS-token i avsnittet hello enhet av [med IoT-hubb säkerhetstoken][lnk-sas-tokens].

    När du testar, du kan också använda hello [enheten explorer] [ lnk-device-explorer] verktyget tooquickly Generera en SAS-token som du kan kopiera och klistra in i din egen kod:

  1. Gå toohello **Management** fliken i **enheten Explorer**.
  2. Klicka på **SAS-Token** (övre högra).
  3. På **SASTokenForm**, Välj din enhet i hello **DeviceID** listrutan. Ange din **TTL**.
  4. Klicka på **generera** toocreate din token.

     hello SAS-token som skapas har den här strukturen: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

     en del av den här token toouse som hello hello **lösenord** fältet tooconnect med MQTT är: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

MQTT ansluta och koppla från paket, IoT-hubb utfärdar en händelse på hello **Operations Monitoring** kanalen med ytterligare information som kan hjälpa dig att tootroubleshoot anslutningsproblem.

### <a name="sending-device-to-cloud-messages"></a>Skicka meddelanden från enhet till moln

När du har gjort en lyckad anslutning en enhet kan skicka meddelanden tooIoT hubb med `devices/{device_id}/messages/events/` eller `devices/{device_id}/messages/events/{property_bag}` som en **avsnittsnamn**. Hej `{property_bag}` -elementet låter hälsningsmeddelande enheten toosend med ytterligare egenskaper i en url-kodat format. Exempel:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> Detta `{property_bag}` element använder hello samma kodning som frågesträngar i hello HTTP-protokollet.
>
>

Hej enhetsapp kan också använda `devices/{device_id}/messages/events/{property_bag}` som hello **kommer avsnittsnamn** toodefine *kommer meddelanden* toobe vidarebefordras som meddelandet telemetri.

- IoT-hubben har inte stöd för QoS-2-meddelanden. Om en enhetsapp publicerar ett meddelande med **QoS 2**, IoT-hubb stängs hello nätverksanslutning.
- IoT-hubb sparas inte behålla meddelanden. Om en enhet skickar ett meddelande med hello **behålla** too1-flaggan, IoT-hubb läggs hello **x-opt-behålla** egenskapen toohello-meddelande. I så fall behålla meddelandet i stället för att spara hello, IoT-hubb skickar den toohello backend-app.
- IoT-hubb stöder endast en aktiv MQTT-anslutning per enhet. En ny anslutning MQTT uppdrag hello samma enhets-ID orsakar IoT-hubb toodrop hello befintlig anslutning.

Mer information finns i [Messaging Utvecklingsguide][lnk-messaging].

### <a name="receiving-cloud-to-device-messages"></a>Ta emot meddelanden moln till enhet

tooreceive meddelanden från IoT-hubb, en enhet ska prenumerera med `devices/{device_id}/messages/devicebound/#` som en **avsnittet Filter**. Hej med flera nivåer med jokertecken  **#**  i hello avsnittet Filter används bara för tooallow hello ytterligare egenskaper för enhet tooreceive i hello avsnittsnamn. IoT-hubb tillåter inte hello användning av hello  **#**  eller **?** jokertecken för filtrering av underordnade ämnen. Eftersom IoT-hubb som inte är förhandlare generella pub-sub meddelanden stöder endast hello dokumenterade avsnittsnamn och avsnittet filter.

hello enheten inte tar emot meddelanden från IoT-hubb förrän den har prenumererar tooits enhetsspecifika slutpunkt, som representeras av hello `devices/{device_id}/messages/devicebound/#` avsnittet filter. Efter lyckad prenumeration har upprättats startar hello enheten tar emot endast moln till enhet meddelanden som har skickats tooit stund hello hello prenumeration. Hello enhet ansluter med **CleanSession** flaggan för**0**, hello prenumeration är beständig mellan olika sessioner. I det här fallet hello nästa gång datorn ansluts med **CleanSession 0** hello enheten tar emot utestående meddelanden som har skickats tooit när den har kopplats från. Om hello enhet använder **CleanSession** flaggan för**1** men den inte tar emot meddelanden från IoT-hubb tills den prenumererar tooits enhet – slutpunkt.

IoT-hubb levererar meddelanden med hello **avsnittsnamn** `devices/{device_id}/messages/devicebound/`, eller `devices/{device_id}/messages/devicebound/{property_bag}` om det inte finns några egenskaper för meddelande. `{property_bag}`innehåller url-kodade nyckel/värde-par för meddelandeegenskaper. Endast egenskaper för program och användaren går Systemegenskaper (exempelvis **messageId** eller **correlationId**) ingår i hello egenskapsuppsättning. Egenskapsnamn för systemet har hello prefix  **$** , programegenskaper använda hello ursprungliga egenskapsnamn med inget prefix.

När en enhetsapp prenumererar tooa ämne med **QoS 2**, IoT-hubb ger maximal QoS nivå 1 i hello **SUBACK** paket. Därefter ger IoT-hubb meddelanden toohello enhet med hjälp av QoS-1.

### <a name="retrieving-a-device-twins-properties"></a>När en enhet dubbla egenskaper

Först måste en enhet prenumererar för`$iothub/twin/res/#`, tooreceive hello åtgärden svar. Sedan skickar den ett tomt meddelande tootopic `$iothub/twin/GET/?$rid={request id}`, med en ifylld värde för **id för förfrågan**. hello tjänsten skickar sedan ett svarsmeddelande med hello enheten identiska data om avsnittet `$iothub/twin/res/{status}/?$rid={request id}`, med hjälp av hello samma  **id för förfrågan** som hello-begäran.

Id för förfrågan kan vara ett giltigt värde för ett meddelande egenskapsvärde enligt [IoT-hubb messaging Utvecklingsguide][lnk-messaging], och status har verifierats som ett heltal.
hello svarstexten innehåller hello properties-avsnittet för hello enheten dubbla:

hello brödtext hello identitet registerposten begränsad toohello ”egenskaper” medlem, till exempel:

        {
            "properties": {
                "desired": {
                    "telemetrySendFrequency": "5m",
                    "$version": 12
                },
                "reported": {
                    "telemetrySendFrequency": "5m",
                    "batteryLevel": 55,
                    "$version": 123
                }
            }
        }

hello statuskoder är:

|Status | Beskrivning |
| ----- | ----------- |
| 200 | Lyckades |
| 429 | För många förfrågningar (begränsas), enligt [IoT-hubb begränsning][lnk-quotas] |
| 5** | Serverfel |

Mer information finns i [utvecklarhandboken för enheten twins][lnk-devguide-twin].

### <a name="update-device-twins-reported-properties"></a>Uppdatera enheten dubbla rapporterade egenskaper

hello följande anvisningar beskriver hur en enhet uppdaterar hello rapporterade egenskaper i hello enheten dubbla i IoT-hubb:

1. En enhet måste först prenumerera toohello `$iothub/twin/res/#` avsnittet tooreceive hello åtgärdens svar från IoT-hubb.

1. En enhet skickar ett meddelande som innehåller hello enheten dubbla uppdatering toohello `$iothub/twin/PATCH/properties/reported/?$rid={request id}` avsnittet. Detta meddelande innehåller en **id för förfrågan** värde.

1. Hej tjänsten och sedan skickar ett svarsmeddelande som innehåller hello nytt ETag-värde för hello rapporterade egenskapssamlingen om avsnittet `$iothub/twin/res/{status}/?$rid={request id}`. Den här svarsmeddelande använder hello samma **id för förfrågan** som hello-begäran.

meddelandetexten för hello begäran innehåller ett JSON-dokument som innehåller nya värden för rapporterade egenskaper (ingen egenskap eller metadata inte kan ändras).
Varje medlem i hello JSON-dokumentet uppdateringar eller Lägg till hello motsvarande medlem i hello enheten dubbla dokumentet. Ange en medlem för`null`, borttagningar hello medlem från hello som innehåller objektet. Exempel:

        {
            "telemetrySendFrequency": "35m",
            "batteryLevel": 60
        }

hello statuskoder är:

|Status | Beskrivning |
| ----- | ----------- |
| 200 | Lyckades |
| 400 | Felaktig begäran. Felaktig JSON |
| 429 | För många förfrågningar (begränsas), enligt [IoT-hubb begränsning][lnk-quotas] |
| 5** | Serverfel |

Mer information finns i [utvecklarhandboken för enheten twins][lnk-devguide-twin].

### <a name="receiving-desired-properties-update-notifications"></a>Ta emot meddelanden om uppdateringar egenskaper

När en enhet är ansluten, IoT-hubb skickar meddelanden toohello avsnittet `$iothub/twin/PATCH/properties/desired/?$version={new version}`, som innehåller hello innehåll hello uppdatering utförs av hello lösningens serverdel. Exempel:

        {
            "telemetrySendFrequency": "5m",
            "route": null
        }

För egenskapen uppdateringar `null` värden innebär att hello JSON objekt medlemmen tas bort.


> [!IMPORTANT] 
> IoT-hubb genererar ändringsmeddelanden endast när enheterna är anslutna. Se till att tooimplement hello [enheten återanslutning flödet] [ lnk-devguide-twin-reconnection] tookeep hello önskade egenskaper mellan IoT-hubb och hello enhetsapp.

Mer information finns i [utvecklarhandboken för enheten twins][lnk-devguide-twin].

### <a name="respond-tooa-direct-method"></a>Svara tooa direkt metod

Först måste en enhet har toosubscribe för`$iothub/methods/POST/#`. IoT-hubb skickar metoden begäranden toohello avsnittet `$iothub/methods/POST/{method name}/?$rid={request id}`, med en giltig JSON eller en brödtext.

toorespond, hello enheten skickar ett meddelande med en giltig JSON eller brödtext toohello avsnittet `$iothub/methods/res/{status}/?$rid={request id}`, där **id för förfrågan** har toomatch hello i hello begärandemeddelandet och **status** har toobe ett heltal .

Mer information finns i [direkt metod Utvecklarhandbok][lnk-methods].

### <a name="additional-considerations"></a>Annat som är bra att tänka på

Som en slutlig faktor om du behöver toocustomize hello MQTT protokollbeteendena på hello molnet sida, bör du granska hello [Azure IoT-protokollgatewayen] [ lnk-azure-protocol-gateway] som du kan använda toodeploy hög prestanda anpassade protokollet gateway som direkt med IoT-hubb-gränssnitt. hello Azure IoT-protokollet gateway aktiverar du toocustomize hello enheten protokollet tooaccommodate brownfield MQTT distributioner eller andra anpassade protokoll. Den här metoden kräver dock att du kör och använda en anpassad protocol-gateway.

## <a name="next-steps"></a>Nästa steg

toolearn mer om hello MQTT protokoll finns hello [MQTT dokumentationen][lnk-mqtt-docs].

toolearn mer information om hur du planerar din IoT-hubb-distribution, se:

* [Azure certifierad för katalogen för IoT-enhet][lnk-devices]
* [Stöd för fler protokoll][lnk-protocols]
* [Jämför med Händelsehubbar][lnk-compare]
* [Skalning, hög tillgänglighet och Katastrofåterställning][lnk-scaling]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Utvecklarhandbok för IoT-hubb][lnk-devguide]
* [Simulera en enhet med Azure IoT kant][lnk-iotedge]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-sas-tokens]: iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md

[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-twin-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow
[lnk-devguide-twin]: iot-hub-devguide-device-twins.md
