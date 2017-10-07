---
title: aaaUnderstand Azure IoT Hub direkt metoder | Microsoft Docs
description: "Utvecklarhandbok - Använd direkt metoder tooinvoke kod på dina enheter från en app service."
services: iot-hub
documentationcenter: .net
author: nberdy
manager: timlt
editor: 
ms.assetid: 9f0535f1-02e6-467a-9fc4-c0950702102d
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d15d44a0c3e1d1cda1669c1ed011c2f932e3d92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Förstå och anropa direkt metoder från IoT-hubb
## <a name="overview"></a>Översikt
IoT-hubb ger dig möjlighet tooinvoke direkt metoder på enheter från hello molnet. Direkta metoder representerar en request-reply-interaktion med en enhet liknande tooan HTTP anropa i att de lyckas eller misslyckas omedelbart (efter en användardefinierade timeout). Detta är användbart för scenarier där hello loppet av omedelbara åtgärder är olika beroende på om hello enheten var kan toorespond, till exempel skicka ett SMS-wake-up tooa enheten om en enhet är offline (SMS är dyrare än ett metodanrop).

Varje enhet metod riktar sig till en enda enhet. [Jobb] [ lnk-devguide-jobs] ger ett sätt tooinvoke direkt metoder på flera enheter och schemalägga metodanropet för frånkopplade enheter.

Alla med **tjänsten ansluta** behörigheter för IoT-hubb kan anropa en metod på en enhet.

### <a name="when-toouse"></a>När toouse
Direkta metoder följer ett mönster i begäran och svar och är avsedda för kommunikation som kräver omedelbar bekräftelse av deras resultat, vanligtvis interaktiv kontroll av hello enhet, till exempel tooturn på fläktar.

Se för[moln till enhet kommunikation vägledning] [ lnk-c2d-guidance] dirigera om osäkra mellan med hjälp av egenskaper, metoder eller moln till enhet meddelanden.

## <a name="method-lifecycle"></a>Metoden livscykel
Direkta metoder implementeras på hello enhet och kan kräva noll eller fler indata i hello metoden nyttolast toocorrectly instansiera. Du anropa en metod som har direkt via en tjänst-riktade URI (`{iot hub}/twins/{device id}/methods/`). En enhet tar emot direkt metoder igenom avsnittet enhetsspecifika MQTT (`$iothub/methods/POST/{method name}/`). Vi stöder direkt metoder på ytterligare enheter på klientsidan nätverksprotokoll i hello framtida.

> [!NOTE]
> När du anropar en direkt metod på en enhet, egenskapsnamn och värden kan endast innehålla US-ASCII utskrivbara alfanumeriskt, förutom eventuella i hello följande uppsättning: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

Direkta metoder är synkrona och antingen lyckas eller misslyckas efter hello tidsgränsen (standard: 30 sekunder, går in too3600 sekunder). Direkta metoder är användbara i interaktiva scenarier där du vill att en enhet tooact om hello enheten är online och ta emot kommandon, som att slå på en enstaka via telefonen. I så fall kan du toosee en omedelbar lyckad eller misslyckad så hello Molntjänsten kan fungera på hello resultatet så snart som möjligt. hello-enhet kan returnera vissa meddelandetexten på grund av hello-metoden, men det inte behövs för hello metoden toodo så. Det finns ingen garanti på sortering eller alla samtidighet semantik på metodanrop.

Direkta metoden är HTTP-only från hello molnet sida och MQTT endast från hello enhetens sida.

hello nyttolasten för metodbegäranden och svar är en JSON-dokumentet upp too8KB.

## <a name="reference-topics"></a>Referensinformation:
hello ger följande Referensinformation dig mer information om hur du använder direkta metoder.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Anropa en metod som är direkt från en backend-app
### <a name="method-invocation"></a>Metodanropet
Direkt metod anrop på en enhet är HTTP-anrop som omfattar:

* Hej *URI* specifika toohello enhet (`{iot hub}/twins/{device id}/methods/`)
* hello POST *metod*
* *Huvuden* som innehåller hello auktorisering, begär ID, content-type och Innehållskodning
* En transparent JSON *brödtext* i hello följande format:

```
{
    "methodName": "reboot",
    "responseTimeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

Tidsgränsen är i sekunder. Om tidsgränsen inte har angetts används som standard too30 sekunder.

### <a name="response"></a>Svar
hello backend-app tar emot ett svar som omfattar:

* *HTTP-statuskod*, som används för fel som kommer från hello IoT-hubb, inklusive ett 404-fel för enheter som för närvarande inte ansluten
* *Huvuden* som innehåller hello ETag, begär ID, content-type och Innehållskodning
* En JSON *brödtext* i hello följande format:

```
{
    "status" : 201,
    "payload" : {...}
}
```

   Båda `status` och `body` tillhandahålls av hello enheten och använda toorespond med hello enhetens egna statuskod och/eller beskrivning.

## <a name="handle-a-direct-method-on-a-device"></a>Hantera en direkt metod på en enhet
### <a name="method-invocation"></a>Metodanropet
Enheter får direkta metodbegäranden om hello MQTT avsnittet:`$iothub/methods/POST/{method name}/?$rid={request id}`

hello brödtext vilka hello enheten tar emot har hello följande format:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Metodbegäranden är QoS 0.

### <a name="response"></a>Svar
hello enheten skickar svar för`$iothub/methods/res/{status}/?$rid={request id}`, där:

* Hej `status` egenskapen är hello enheten angivna status för körning av metoden.
* Hej `$rid` egenskapen är hello begäran-ID från hello metodanropet togs emot från IoT-hubb.

hello brödtext har angetts av hello enheten och kan vara status.

## <a name="additional-reference-material"></a>Ytterligare referensmaterialet
Andra referensavsnitten i hello IoT-hubb Utvecklarhandbok inkluderar:

* [IoT-hubbslutpunkter] [ lnk-endpoints] beskriver hello olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.
* [Begränsning och kvoter] [ lnk-quotas] beskriver hello kvoter som gäller toohello IoT-hubb-tjänsten och hello bandbreddsbegränsning beteende tooexpect när du använder hello-tjänsten.
* [Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] visar hello olika språk SDK: er som du kan använda när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.
* [IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning] [ lnk-query] beskriver hello IoT-hubb frågespråk som du kan använda tooretrieve information från IoT-hubb om enheten twins och jobb.
* [Stöd för IoT-hubb MQTT] [ lnk-devguide-mqtt] ger mer information om stöd för IoT-hubb för hello MQTT protokollet.

## <a name="next-steps"></a>Nästa steg
Nu har du lärt dig hur toouse direkt metoder, du kan vara intresserad hello följande IoT-hubb developer guide ämne:

* [Schema-jobb på flera enheter][lnk-devguide-jobs]

Om du vill tootry titt på hello begrepp som beskrivs i den här artikeln får du är intresserad av hello följande IoT-hubb kursen:

* [Använda direct-metoder][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
