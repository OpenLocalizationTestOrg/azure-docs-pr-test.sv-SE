---
title: Azure IoT-hubb kommunikationsprotokoll och portar | Microsoft Docs
description: "Utvecklarhandbok - beskriver stöds kommunikationsprotokoll enhet till moln och moln till enhet informations-och de portnummer som måste öppnas."
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
ms.openlocfilehash: 98004a48734e33f85eebf8f6213d9f0751dea843
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# Referera - Välj ett kommunikationsprotokoll

IoT-hubb kan användas på enheter [MQTT][lnk-mqtt], MQTT över WebSockets, [AMQP][lnk-amqp], AMQP över WebSockets och HTTP-protokoll för enheten på klientsidan kommunikation. Information om hur dessa protokoll stöder specifika IoT-hubb-funktioner finns i [enhet till moln kommunikation vägledning] [ lnk-d2c-guidance] och [moln till enhet kommunikation vägledning][lnk-c2d-guidance].

Följande tabell innehåller övergripande rekommendationer för ditt val av protokollet:

| Protokoll | När du ska välja det här protokollet |
| --- | --- |
| MQTT <br> MQTT över WebSocket |Använd på alla enheter som inte kräver för att ansluta flera enheter (var och en med sina egna autentiseringsuppgifter per enhet) via samma TLS-anslutning. |
| AMQP <br> AMQP över WebSocket |Använda på fältet och molnet gateways för att dra nytta av anslutningen multiplexering mellan enheter. |
| HTTP |Används för enheter som inte stöder andra protokoll. |

Tänk på följande när du väljer att protokollet för enheten på klientsidan kommunikation:

* **Moln till enhet mönster**. HTTP har inte ett effektivt sätt att implementera server push. Därför när du använder HTTP avsöker enheter IoT-hubb för moln till enhet meddelanden. Den här metoden är ineffektiv för både enheten och IoT-hubb. Under den aktuella HTTP riktlinjerna ska varje enhet avsöka för meddelanden var 25: e minut eller mer. Å andra sidan MQTT och AMQP stöd för server push när du tar emot meddelanden moln till enhet. De ger omedelbar push-meddelanden av meddelanden från IoT-hubb till enheten. Om leverans svarstiden är ett bekymmer, är MQTT eller AMQP de bästa protokoll som ska användas. För sällan anslutna enheter fungerar HTTP samt.
* **Fältet gateways**. När du använder MQTT och HTTP kan du inte kan ansluta flera enheter (var och en med sina egna autentiseringsuppgifter per enhet) med samma TLS-anslutning. Därför för [fältet gateway-scenarier][lnk-azure-gateway-guidance], dessa protokoll är något sämre eftersom de kräver en TLS-anslutning mellan fältet gateway- och IoT-hubb för varje enhet är ansluten till fältet gateway.
* **Lite resurs enheter**. MQTT och HTTP-bibliotek har en mindre utrymme än AMQP-bibliotek. Om enheten har begränsat resurser (till exempel, mindre än 1 MB RAM-minne), som sådana vara dessa protokoll den enda protokollimplementering.
* **Nätverk traversal**. AMQP standardprotokollet använder port 5671, medan MQTT lyssnar på port 8883, vilket kan orsaka problem i nätverk som har stängts till andra protokoll än HTTP. MQTT över WebSockets AMQP över WebSockets och HTTP kan användas i det här scenariot.
* **Nyttolastens storlek**. MQTT och AMQP är binär protokoll, vilket resulterar i mer komprimerade nyttolaster än HTTP.

> [!WARNING]
> När du använder HTTP måste ska varje enhet avsökas moln till enhet meddelanden var 25: e minut eller flera. Dock under utveckling är det acceptabelt att avsöka oftare än var 25: e minut.

## Portnummer

Enheter kan kommunicera med IoT-hubben i Azure med olika protokoll. Val av protokoll är normalt styrs av de särskilda kraven i lösningen. I följande tabell visas de utgående portar som måste vara öppna för en enhet för att kunna använda ett visst protokoll:

| Protokoll | Port |
| --- | --- |
| MQTT |8883 |
| MQTT över WebSockets |443 |
| AMQP |5671 |
| AMQP över WebSockets |443 |
| HTTP |443 |

När du har skapat en IoT-hubb i en Azure-region, har IoT-hubben samma IP-adress för livslängden för den IoT-hubben. Men om du vill upprätthålla tjänstkvaliteten, om Microsoft flyttar IoT-hubben till en annan skalningsenhet tilldelas den en ny IP-adress.


## Nästa steg

Mer information om hur IoT-hubb implementerar protokollet MQTT finns [kommunicera med din IoT-hubb med protokollet MQTT][lnk-mqtt-support].

[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-mqtt-support]: iot-hub-mqtt-support.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
