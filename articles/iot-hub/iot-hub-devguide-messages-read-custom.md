---
title: "anpassade slutpunkter för aaaUnderstand Azure IoT Hub | Microsoft Docs"
description: "Utvecklarhandbok - med hjälp av Routning regler tooroute meddelanden från enhet till moln toocustom slutpunkter."
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
ms.openlocfilehash: daa9cfb35d0853e316bbf469b034d4dadbd4e85d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Använd meddelandevägar och anpassade slutpunkter för meddelanden från enhet till moln

IoT-hubb kan du tooroute [meddelanden från enhet till moln] [ lnk-device-to-cloud] tooIoT hubb service-riktade slutpunkter baserat på Egenskaper. Routning regler ger du hello flexibilitet toosend måste där de behöver toogo utan hello-meddelanden för ytterligare tjänster tooprocess meddelanden eller toowrite ytterligare kod. Varje regel för vidarebefordran av du konfigurerar har hello följande egenskaper:

| Egenskap      | Beskrivning |
| ------------- | ----------- |
| **Namn**      | hello unika namn som identifierar hello regeln. |
| **Källa**    | hello ursprung hello strömma toobe efterlevts. Till exempel enhetstelemetrin. |
| **Villkor** | hello frågeuttrycket för hello routningsregel som körs mot hello-meddelande sidhuvuden och brödtext och används toodetermine om det finns en matchning för hello slutpunkt. Mer information om hur du skapar en väg villkoret finns hello [referens - frågespråket för jobb och enheten twins][lnk-devguide-query-language]. |
| **Slutpunkt**  | hello namnet på hello slutpunkt där IoT-hubb skickar meddelanden som matchar hello villkor. Slutpunkter måste vara i hello samma region som hello IoT-hubb annars du kanske att debiteras för cross-region skriver. |

Ett enda meddelande kan matcha hello tillståndet på flera regler för routning, som ger fallet IoT-hubb hello meddelandet toohello slutpunkt som är associerade med varje matchade regel. IoT-hubb deduplicates också automatiskt meddelandeleverans, så om ett meddelande matchar flera regler som har hello samma mål den bara skrivs toothat mål en gång.

En IoT-hubb har standard [inbyggd slutpunkt][lnk-built-in]. Du kan skapa anpassade slutpunkter tooroute meddelanden tooby länkning av andra tjänster i din prenumeration toohello hubb. IoT-hubb stöder för närvarande Händelsehubbar, Service Bus-köer och Service Bus-ämnen som anpassade slutpunkter.

> [!WARNING]
> Service Bus-köer och ämnen med **sessioner** eller **dubblettidentifiering** aktiverat som anpassade slutpunkter stöds inte.

Mer information om hur du skapar anpassade slutpunkter i IoT-hubb finns [IoT-hubbslutpunkter][lnk-devguide-endpoints].

För mer information om läsning från anpassade slutpunkter, se:

* Läsning från [Händelsehubbar][lnk-getstarted-eh].
* Läsning från [Service Bus-köer][lnk-getstarted-queue].
* Läsning från [Service Bus-ämnen][lnk-getstarted-topic].

### <a name="next-steps"></a>Nästa steg

Läs mer om IoT-hubbslutpunkter [IoT-hubbslutpunkter][lnk-devguide-endpoints].

Mer information om hello frågespråket du använder regler för routning av toodefine, se [IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning][lnk-devguide-query-language].

Hej [processen IoT Hub-enhet till moln meddelanden med hjälp av vägar] [ lnk-d2c-tutorial] kursen visar hur toouse routning regler och anpassade slutpunkter.

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
