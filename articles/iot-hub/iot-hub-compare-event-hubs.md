---
title: "aaaCompare Azure IoT Hub tooAzure Händelsehubbar | Microsoft Docs"
description: "En jämförelse av hello IoT-hubb och Event Hubs Azure services syntaxmarkering funktionella skillnader och användningsfall. hello jämförelse innehåller protokoll som stöds, hantering, övervakning, och filöverföringar."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: aeddea62-8302-48e2-9aad-c5a0e5f5abe9
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: e5f546b54e29860498d540abfc86a41c4662c0df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Jämförelse mellan Azure IoT-hubb och Azure Event Hubs
En av hello huvudsakliga användningsområden för IoT-hubben är toogather telemetri från enheter. Därför IoT-hubb som ofta jämförs för[Azure Event Hubs][Azure Event Hubs]. Händelsehubbar är en tjänst som gör det händelse- och telemetriingång för händelsebearbetning som IoT-hubb toohello molnet i massiv skala med kort svarstid och hög tillförlitlighet.

Hello-tjänster har emellertid många skillnader som beskrivs i följande tabell hello:

| Område | IoT Hub | Händelsehubbar |
| --- | --- | --- |
| Kommunikationsmönster | Aktiverar [enhet till moln kommunikation] [ lnk-d2c-guidance] (messaging filen överföringar och rapporterade egenskaper) och [moln till enhet kommunikation] [ lnk-c2d-guidance] (direct metoder, egenskaper, messaging). |Aktiverar endast händelse ingång (vanligtvis betraktas för scenarier med enhet till moln). |
| Information om enhetens tillstånd | [Enheten twins] [ lnk-twins] kan lagra och hämta information om enhetens tillstånd. | Ingen statusinformation för enheten kan inte lagras. |
| Enheten protokollstöd |Stöder MQTT MQTT över WebSockets, AMQP, AMQP över WebSockets och HTTP. Dessutom IoT-hubb fungerar med hello [Azure IoT-protokollgatewayen][lnk-azure-protocol-gateway], en anpassningsbara protocol gateway implementering toosupport anpassade protokoll. |Stöder AMQP AMQP över WebSockets och HTTP. |
| Säkerhet |Ger identitet per enhet och återkallningsbar åtkomstkontroll. Se hello [Säkerhetsavsnittet i hello IoT-hubb Utvecklarhandbok]. |Ger Event Hubs hela [delade åtkomstprinciper][Event Hubs - security], med begränsad återkallade stöder via [utgivarens principer][Event Hubs publisher policies]. IoT-lösningar är ofta krävs tooimplement en anpassad lösning toosupport per enhet autentiseringsuppgifter och skydd mot förfalskning mått. |
| Övervakning av åtgärder |Gör det möjligt för IoT-lösningar toosubscribe tooa omfattande uppsättning enheten identity management och anslutningar händelser som enskild enhet autentiseringsfel, begränsning och felaktigt format undantag. Dessa händelser kan du tooquickly identifiera problem med nätverksanslutningen på hello enskild enhetsnivå. |Visar endast sammanställd mått. |
| Skala |Är optimerad toosupport miljoner samtidigt anslutna enheter. |Mätare hello anslutningar enligt [Händelsehubbar i Azure-kvoter][Azure Event Hubs quotas]. Hej på andra sidan, Event Hubs kan du toospecify hello partition för varje meddelande som skickas. |
| Enheten SDK |Ger [enheten SDK] [ Azure IoT SDKs] för en stor mängd olika plattformar och språk i tillägg toodirect MQTT, AMQP och HTTP-APIs. |Stöds på .NET, Java och C, i tillägget tooAMQP och HTTP send-gränssnitt. |
| Ladda upp filen |Aktiverar IoT-lösningar tooupload filer från enheter toohello molnet. Innehåller en fil aviseringsslutpunkten för arbetsflödet integrering och ett åtgärder övervakning kategori för felsökning stöd. | Stöds inte. |
| Väg meddelanden toomultiple slutpunkter | Konfigurera too10 stöds anpassade slutpunkter. Reglerna bestämmer hur meddelanden är routade toocustom slutpunkter. Mer information finns i [skicka och ta emot meddelanden med IoT-hubben][lnk-devguide-messaging]. | Kräver ytterligare kod toobe skrivs och är värd för meddelandesändning. |

Även om hello endast användningsfall är enhet till moln telemetriingång Sammanfattningsvis ger en tjänst som är avsedd för IoT-enhetsanslutning IoT-hubb. Den fortsätter tooexpand hello värderingsförslag för dessa scenarier med IoT-specifika funktioner. Händelsehubbar är utformad för händelsen ingång i massiv skala både hello gäller bland datacentret och intra-datacenter scenarier.

Det är inte ovanligt toouse både IoT-hubb och Händelsehubbar i hello samma lösning. IoT-hubb hanterar hello enhet till moln kommunikation och Händelsehubbar hanterar senare skede händelse ingång till realtidsbearbetning motorer.

### <a name="next-steps"></a>Nästa steg
toolearn mer information om hur du planerar distributionen IoT-hubb finns [skalning, hög tillgänglighet och Katastrofåterställning][lnk-scaling].

toofurther utforska hello funktionerna i IoT Hub, se:

* [Utvecklarhandbok för IoT-hubb][lnk-devguide]
* [Simulera en enhet med IoT kant][lnk-iotedge]

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[Säkerhetsavsnittet i hello IoT-hubb Utvecklarhandbok]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-features.md#event-publishers
[Azure Event Hubs quotas]: ../event-hubs/event-hubs-quotas.md
[Azure IoT SDKs]: https://github.com/Azure/azure-iot-sdks
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
