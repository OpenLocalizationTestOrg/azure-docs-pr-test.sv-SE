---
title: aaaAzure IoT-enhetshantering med iothub explorer | Microsoft Docs
description: "Verktyget hello iothub explorer CLI för Azure IoT Hub enhetshantering med hello direkt metoder och hanteringsalternativ för hello dubbla egenskaper."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Azure iot enhetshantering, azure iot-hubb enhetshantering, device management iot, enhetshantering för iot-hubb"
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: xshi
ms.openlocfilehash: e0a5e6120db5c4fb12f7f8b605a56e0e4aad9217
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-iothub-explorer-for-azure-iot-hub-device-management"></a>Använda iothub explorer för hantering av Azure IoT Hub-enheter

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[iothub explorer](https://github.com/azure/iothub-explorer) är ett CLI-verktyg som du kör på en värd datorn toomanage enheten identiteter i din IoT-hubb registret. Den innehåller alternativ som du kan använda tooperform olika uppgifter.

| Hanteringsalternativ          | Aktivitet                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Direkta metoder             | Gör en enhet som fungerar som startar eller stoppar skickar meddelanden eller starta om hello enhet.                                        |
| Dubbla önskade egenskaper    | Placera en enhet i vissa lägen, till exempel ange en Indikator toogreen eller ange hello telemetri skicka too30 minuter.         |
| Dubbla rapporterade egenskaper   | Hämta hello rapporterade tillståndet för en enhet. Till exempel rapporterar hello enheten hello Indikator blinkar nu.                                    |
| Dubbla taggar                  | Lagra enhetsspecifika metadata i hello molnet. Till exempel hello distribution platsen för en Varuautomat.                         |
| Meddelanden moln till enhet   | Skicka meddelanden tooa enhet. Till exempel ”är det mycket troligt toorain idag. Glöm inte toobring ett samlingsnamn ”.              |
| Enheten dubbla frågor        | Fråga alla enheten twins tooretrieve dem med valfri villkor, till exempel identifiera hello-enheter som är tillgängliga för användning. |

Mer detaljerad förklaring på hello skillnader och vägledning om hur du använder dessa alternativ finns [enhet till moln kommunikation vägledning](iot-hub-devguide-d2c-guidance.md) och [moln till enhet kommunikation vägledning](iot-hub-devguide-c2d-guidance.md).

> [!NOTE]
> Enhetstvillingar är JSON-dokument som lagrar information om enhetstillstånd (metadata, konfigurationer och villkor). IoT-hubb kvarstår en enhet dubbla för varje enhet som ansluter tooit. Läs mer om enheten twins [Kom igång med enheten twins](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Detta får du får lära dig

Du lär dig med olika alternativ på utvecklingsdatorn iothub explorer.

## <a name="what-you-do"></a>Vad du gör

Kör iothub explorer med olika hanteringsalternativ för.

## <a name="what-you-need"></a>Vad du behöver

- Kursen [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md) slutförts som omfattar hello följande krav:
  - En aktiv Azure-prenumeration.
  - En Azure IoT-hubb i din prenumeration.
  - Ett klientprogram som skickar meddelanden tooyour Azure IoT-hubb.
- Kontrollera att enheten kör med hello klientprogrammet under den här kursen.
- iothub-explorer [installera iothub explorer](https://github.com/azure/iothub-explorer) på utvecklingsdatorn.

## <a name="connect-tooyour-iot-hub"></a>Ansluta tooyour IoT-hubb

Anslut tooyour IoT-hubb genom att köra följande kommando hello:

```bash
iothub-explorer login <your IoT hub connection string>
```

## <a name="use-iothub-explorer-with-direct-methods"></a>Använd iothub explorer med direkta metoder

Anropa hello `start` metod i hello enheten app toosend meddelanden tooyour IoT-hubb genom att köra följande kommando hello:

```bash
iothub-explorer device-method <your device Id> start
```

Anropa hello `stop` metod i hello enheten app toostop skickar meddelanden tooyour IoT-hubb genom att köra följande kommando hello:

```bash
iothub-explorer device-method <your device Id> stop
```

## <a name="use-iothub-explorer-with-twins-desired-properties"></a>Använd iothub explorer med dubblas egenskaper

Ange ett intervall önskade egenskapen = 3000 genom att köra följande kommando hello:

```bash
iothub-explorer update-twin <your device id> {\"properties\":{\"desired\":{\"interval\":3000}}}
```

Den här egenskapen kan läsas av enheten.

## <a name="use-iothub-explorer-with-twins-reported-properties"></a>Använd iothub explorer med dubblas rapporterade egenskaper

Hämta hello rapporterade egenskaperna för hello enhet genom att köra hello följande kommando:

```bash
iothub-explorer get-twin <your device id>
```

En av hello egenskaper är $metadata. $lastUpdated som visar hello senast enheten skickar och tar emot ett meddelande.

## <a name="use-iothub-explorer-with-twins-tags"></a>Använd iothub explorer med dubblas taggar

Visa hello taggar och egenskaperna för hello enhet genom att köra följande kommando hello:

```bash
iothub-explorer get-twin <your device id>
```

Lägga till en roll för fältet = temperatur & fuktighet toohello enheten genom att köra följande kommando hello:

```bash
iothub-explorer update-twin <your device id> "{\"tags\":{\"role\":\"temperature&humidity\"}}"

```

## <a name="use-iothub-explorer-with-cloud-to-device-messages"></a>Använda iothub explorer med meddelanden moln till enhet

Skicka en ”Hello World” meddelande toohello enhet genom att köra följande kommando hello:

```bash
iothub-explorer send <device-id> "Hello World"
```

Se [använder iothub explorer toosend och ta emot meddelanden mellan din enhet och IoT-hubb](iot-hub-explorer-cloud-device-messaging.md) verkliga scenarier för att använda det här kommandot.

## <a name="use-iothub-explorer-with-device-twins-queries"></a>Använda iothub explorer med enheten twins frågor

Fråga enheter med en tagg av rollen = 'temperatur- och fuktighetskonsekvens' genom att köra följande kommando hello:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Fråga alla enheter utom de som har en tagg av rollen = 'temperatur- och fuktighetskonsekvens' genom att köra följande kommando hello:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Nästa steg

Du har lärt dig hur toouse iothub-explorer med olika hanteringsalternativ för.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
