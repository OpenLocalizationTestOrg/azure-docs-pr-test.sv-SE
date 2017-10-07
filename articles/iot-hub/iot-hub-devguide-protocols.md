---
title: aaaAzure IoT-hubb kommunikation protokoll och portar | Microsoft Docs
description: "Utvecklarhandbok - beskriver hello stöds kommunikationsprotokoll enhet till moln och moln till enhet informations-och hello-portnummer som måste öppnas."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 31cb948f1d30edd87edb13ad0dd859c02bcc3239
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Referera - Välj ett kommunikationsprotokoll

IoT-hubb kan enheter toouse [MQTT][lnk-mqtt], MQTT över WebSockets, [AMQP][lnk-amqp], AMQP över WebSockets och HTTP-protokoll för enheten på klientsidan kommunikation. Information om hur dessa protokoll stöder specifika IoT-hubb-funktioner finns i [enhet till moln kommunikation vägledning] [ lnk-d2c-guidance] och [moln till enhet kommunikation vägledning][lnk-c2d-guidance].

hello innehåller följande tabell hello övergripande rekommendationer för ditt val av protokollet:

| Protokoll | När du ska välja det här protokollet |
| --- | --- |
| MQTT <br> MQTT över WebSocket |På alla enheter som inte kräver tooconnect använder flera enheter (var och en med sina egna autentiseringsuppgifter per enhet) över hello samma TLS-anslutning. |
| AMQP <br> AMQP över WebSocket |Använd på fältet och molnet gateways tootake utnyttjar anslutningen multiplexering mellan enheter. |
| HTTP |Används för enheter som inte stöder andra protokoll. |

Överväg följande punkter när du väljer att protokollet för enheten på klientsidan kommunikation hello:

* **Moln till enhet mönster**. HTTP har inte ett effektivt sätt tooimplement server push. Därför när du använder HTTP avsöker enheter IoT-hubb för moln till enhet meddelanden. Den här metoden är ineffektiv för både hello enheten och IoT-hubb. Under den aktuella HTTP riktlinjerna ska varje enhet avsöka för meddelanden var 25: e minut eller mer. Hej på andra sidan, MQTT och AMQP stöder server push när du tar emot meddelanden moln till enhet. De ger omedelbar push-meddelanden av meddelanden från IoT-hubb toohello enhet. Om överföringen svarstiden är ett bekymmer, är MQTT eller AMQP hello bästa protokoll toouse. För sällan anslutna enheter fungerar HTTP samt.
* **Fältet gateways**. När du använder MQTT och HTTP kan du inte kan ansluta flera enheter (var och en med sina egna autentiseringsuppgifter per enhet) med hjälp av hello samma TLS-anslutning. Därför för [fältet gateway-scenarier][lnk-azure-gateway-guidance], dessa protokoll är något sämre eftersom de kräver en TLS-anslutning mellan hello fältet gateway- och IoT-hubb för varje enhet ansluten toohello fältet gateway.
* **Lite resurs enheter**. Hej MQTT och HTTP-biblioteken har en mindre utrymme än hello AMQP bibliotek. Som exempel, om hello enheten begränsade resurser (till exempel, mindre än 1 MB RAM-minne), dessa protokoll kan vara hello endast protokollimplementering tillgängliga.
* **Nätverk traversal**. Hej AMQP standardprotokoll använder port 5671, medan MQTT lyssnar på port 8883, vilket kan orsaka problem i nätverk som är stängd toonon HTTP-protokoll. MQTT över WebSockets AMQP över WebSockets och HTTP är tillgängliga toobe som används i det här scenariot.
* **Nyttolastens storlek**. MQTT och AMQP är binär protokoll, vilket resulterar i mer komprimerade nyttolaster än HTTP.

> [!WARNING]
> När du använder HTTP måste ska varje enhet avsökas moln till enhet meddelanden var 25: e minut eller flera. Dock under utveckling är det godkända toopoll oftare än var 25: e minut.

## Portnummer

Enheter kan kommunicera med IoT-hubben i Azure med olika protokoll. Normalt drivs hello val av protokoll av hello specifika krav på hello lösningen. hello visas följande tabell hello utgående portar som måste vara öppna för en enhet toobe kan toouse ett visst protokoll:

| Protokoll | Port |
| --- | --- |
| MQTT |8883 |
| MQTT över WebSockets |443 |
| AMQP |5671 |
| AMQP över WebSockets |443 |
| HTTP |443 |

När du har skapat en IoT-hubb i en Azure-region, hello hello IoT-hubb behåller samma IP-adress för hello livstid som IoT-hubb. Toomaintain tjänstkvalitet, om Microsoft flyttas då hello IoT-hubb tooa annan skala enhet tilldelas en ny IP-adress.


## Nästa steg

toolearn mer information om hur IoT-hubb implementerar hello MQTT protokoll finns [kommunicera med din IoT-hubb med hello MQTT protokollet][lnk-mqtt-support].

[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-mqtt-support]: iot-hub-mqtt-support.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
