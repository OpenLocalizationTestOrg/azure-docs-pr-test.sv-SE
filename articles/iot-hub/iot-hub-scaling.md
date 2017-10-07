---
title: aaaAzure IoT-hubb skalning | Microsoft Docs
description: "Hur tooscale din IoT-hubb toosupport din förväntade genomströmningen. Innehåller en sammanfattning av hello stöds genomflödet för varje nivå och alternativ för delning."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: e7bd4968-db46-46cf-865d-9c944f683832
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3b8bf6c44631c65b34b69752d9043c21db24bb01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-your-iot-hub-solution"></a>Skala din lösning för IoT-hubb
Azure IoT-hubb kan stödja tooa miljoner samtidigt anslutna enheter. Mer information finns i [IoT-hubb priser][lnk-pricing]. Varje IoT Hub-enhet kan ett visst antal dagliga meddelanden.

tooproperly skala din lösning bör du överväga att viss användningen av IoT-hubb. I synnerhet Tänk hello krävs belastning genomströmning för hello följande typer av åtgärder:

* Meddelanden från enheten till molnet
* Meddelanden moln till enhet
* Identitetsregisteråtgärder

I toothis genomströmning informationen, se [IoT-hubb kvoter och begränsningar] [ IoT Hub quotas and throttles] och därefter utforma din lösning.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Enhet till moln och moln till enhet genomströmningen
hello bästa sätt toosize en IoT-hubb-lösning är tooevaluate hello-trafik på grundval av per enhet.

Meddelanden från enhet till moln följer dessa riktlinjer för varaktigt dataflöde.

| Nivå | Varaktigt dataflöde | Varaktiga överföringshastighet |
| --- | --- | --- |
| S1 |Konfigurera too1111 KB per minut per enhet<br/>(1,5 GB/dag/unit) |Medelvärde för 278 meddelanden per minut per enhet<br/>(400 000 meddelanden/dag per enhet) |
| S2 |Konfigurera too16 MB per minut per enhet<br/>(22,8 GB/dag/unit) |Medelvärde för 4,167 meddelanden per minut per enhet<br/>(6 miljoner meddelanden/dag per enhet) |
| S3 |Konfigurera too814 MB per minut per enhet<br/>(1144.4 GB/dag/unit) |Medelvärde för 208,333 meddelanden per minut per enhet<br/>(300 miljoner meddelanden/dag per enhet) |

## <a name="identity-registry-operation-throughput"></a>Identitet registret åtgärden genomflöde
IoT-hubb identitet registret åtgärder är inte avsedda toobe körning åtgärder, som de är oftast relaterade toodevice etablering.

Specifika burst prestanda siffror finns [IoT-hubb kvoter och begränsningar][IoT Hub quotas and throttles].

## <a name="sharding"></a>Horisontell partitionering
När en enda IoT-hubb kan skala toomillions av enheter, kräver ibland din lösning specifika prestandaegenskaper som en enda IoT-hubb inte kan garantera. I så fall bör du partitionera dina enheter till flera IoT-hubbar. Flera IoT-hubbar Utjämna trafiken belastning och hämta hello krävs genomströmning eller åtgärden priser som krävs.

## <a name="next-steps"></a>Nästa steg
toofurther utforska hello funktionerna i IoT Hub, se:

* [Utvecklarhandbok för IoT-hubb][lnk-devguide]
* [Simulera en enhet med Azure IoT kant][lnk-iotedge]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
