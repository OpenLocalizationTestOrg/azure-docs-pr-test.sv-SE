---
title: aaaUnderstand Azure IoT Hub-enhet till moln messaging | Microsoft Docs
description: "Utvecklarhandbok - hur toouse enhet till moln meddelanden med IoT-hubben. Innehåller information om hur du skickar data både telemetri och icke-telemtry vidarebefordra toodeliver meddelanden."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 07dc8a6be747365c7efbc528ab2762b0d9790758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-device-to-cloud-messages-tooiot-hub"></a>Skicka meddelanden från enhet till moln tooIoT Hub

toosend tidsserier telemetri och varningar från dina enheter tooyour lösningens serverdel, skicka meddelanden från enhet till moln från din enhet tooyour IoT-hubb. En beskrivning av andra enhet till moln-alternativ som stöds av IoT-hubb finns [enhet till moln kommunikation vägledning][lnk-d2c-guidance].

Du skickar meddelanden från enhet till moln via en enhet riktade slutpunkt (**/devices/ {deviceId} / meddelanden/händelser**). Routningsregler och sedan dirigera dina meddelanden tooone hello service-riktade slutpunkter på din IoT-hubb. Regler för Routning använder hello sidhuvuden och brödtext hälsningsmeddelande enhet till moln som passerar genom din hubb toodetermine där tooroute dem. Som standard är meddelanden routade toohello inbyggd service-riktade slutpunkt (**meddelanden/händelser**), som är kompatibel med [Händelsehubbar][lnk-event-hubs]. Därför kan du använda standard [Händelsehubbar integrering och SDK] [ lnk-compatible-endpoint] tooreceive meddelanden från enhet till moln i din lösning serverdel.

IoT-hubb implementerar enhet till moln-meddelanden med hjälp av en strömmande meddelandemönster används. IoT-hubb meddelanden från enhet till moln är mer som [Händelsehubbar] [ lnk-event-hubs] *händelser* än [Service Bus] [ lnk-servicebus] *meddelanden* att en stor volym med händelser som passerar genom hello-tjänst som kan läsas av flera läsare.

Enhet till moln meddelanden med IoT-hubben har hello följande egenskaper:

* Meddelanden från enhet till moln är beständiga och behållas i en IoT-hubb standard **meddelanden/händelser** slutpunkt för in tooseven dagar.
* Meddelanden från enhet till moln kan innehålla högst 256 KB och kan grupperas i batchar toooptimize skickar. Batchar kan innehålla högst 256 KB.
* Enligt beskrivningen i hello [kontroll åtkomst tooIoT hubb] [ lnk-devguide-security] avsnittet, IoT-hubb kan per enhet autentisering och åtkomstkontroll.
* IoT-hubb kan toocreate av too10 anpassade slutpunkter. Meddelanden levereras toohello slutpunkter baserat på vägar som konfigurerats på din IoT-hubb. Mer information finns i [regler för routning](#routing-rules).
* IoT-hubb kan miljontals samtidigt anslutna enheter (se [kvoter och begränsning][lnk-quotas]).
* IoT-hubb kan inte godtycklig partitionering. Meddelanden från enhet till moln partitioneras baserat på deras ursprung **deviceId**.

Mer information om hello skillnader mellan hello IoT-hubb och Händelsehubbar tjänster finns [jämförelse av Azure IoT Hub och Azure Event Hubs][lnk-comparison].

## <a name="send-non-telemetry-traffic"></a>Skicka ej telemetri trafik

Ofta dessutom tootelemetry datapunkter, enheterna skickar meddelanden och förfrågningar som kräver separat utförande och hantering i hello lösningens serverdel. Till exempel serverdel kritiska aviseringar som skall utlösa en specifik åtgärd i hello. Du kan enkelt skriva en [routningsregel] [ lnk-devguide-custom] toosend dessa typer av meddelanden tooan slutpunkten dedicated tootheir bearbetning baserat på antingen ett sidhuvud på hello-meddelande eller ett värde i hello meddelandetexten.

Mer information om hello bästa sätt tooprocess kind det här meddelandet finns hello [Självstudier: hur tooprocess IoT-hubb meddelanden från enhet till moln] [ lnk-d2c-tutorial] kursen.

## <a name="route-device-to-cloud-messages"></a>Vidarebefordra meddelanden från enhet till moln

Har du två alternativ för routning meddelanden från enhet till moln tooyour backend-appar:

* Använd hello inbyggda [Event Hub-kompatibel endpoint] [ lnk-compatible-endpoint] tooenable backend-appar tooread hello enhet till moln har tagits emot av hello hubb. toolearn om hello inbyggd Event Hub-kompatibel slutpunkt finns [läsa meddelanden från enhet till moln från hello inbyggd slutpunkt][lnk-devguide-builtin].
* Använd Routning regler toosend meddelanden toocustom slutpunkter i din IoT-hubb. Anpassade slutpunkter aktivera din serverdel appar tooread enhet till moln-meddelanden med Händelsehubbar, Service Bus-köer eller Service Bus-ämnen. toolearn om Routning och anpassade slutpunkter, se [använda anpassade slutpunkter och routningsregler för meddelanden från enhet till moln][lnk-devguide-custom].

## <a name="anti-spoofing-properties"></a>Skydd mot förfalskning egenskaper

tooavoid enhet förfalskning i meddelanden från enhet till moln, IoT-hubb stämplar alla meddelanden med hello följande egenskaper:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

hello först två innehåller hello **deviceId** och **generationId** av hello kommer enheten enligt [identitet enhetsegenskaper][lnk-device-properties].

Hej **ConnectionAuthMethod** -egenskapen innehåller ett serialiserat JSON-objekt med hello följande egenskaper:

```json
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Nästa steg

Information om hello SDK kan du använda toosend meddelanden från enhet till moln, finns [Azure IoT SDK][lnk-sdks].

Hej [Kom igång] [ lnk-get-started] självstudiekurser visar hur toosend enhet till moln meddelanden från både simulerade och fysiska enheter. Mer information finns i hello [processen IoT Hub-enhet till moln meddelanden med hjälp av vägar] [ lnk-d2c-tutorial] kursen.

[lnk-devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[lnk-devguide-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-comparison]: iot-hub-compare-event-hubs.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-get-started]: iot-hub-get-started.md

[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-compatible-endpoint]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
