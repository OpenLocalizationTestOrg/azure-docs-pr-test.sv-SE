---
title: aaaUnderstand Azure IoT Hub-meddelanden | Microsoft Docs
description: "Utvecklarhandbok - enhet till moln och moln till enhet meddelanden med IoT-hubben. Innehåller information om meddelandeformat och stöds kommunikationsprotokoll."
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
ms.openlocfilehash: a610741e23e243f392f1c042f9ab4a00d42f734f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a>Enhet till moln och moln till enhet meddelanden med IoT-hubb

Använd IoT-hubb messaging toocommunicate med dina enheter genom att:

* Skicka [enhet till moln] [ lnk-d2c] meddelanden från enheter tooyour lösningen serverdel.
* Skicka [moln till enhet] [ lnk-c2d] meddelanden från hello-lösningen avslutar tooyour enheter.

Grundläggande egenskaper för IoT-hubb meddelandefunktioner är hello tillförlitlighet och hållbarhet av meddelanden. De här egenskaperna aktivera återhämtning toointermittent anslutning på hello enheten sida och tooload ger spikar i diagrammet i händelsebearbetning på hello molnet sida. IoT-hubb implementerar *minst en gång* leverans garanterar för både enhet till moln och moln till enhet meddelanden.

En introduktion toohello funktionerna i IoT-hubb finns hello artiklar [Azure och Sakernas Internet] [ lnk-azure-iot] och [översikt över hello Azure IoT Hub service] [lnk-iot-hub-overview].

## <a name="when-toouse-iot-hub-messaging"></a>När meddelanden om toouse IoT-hubb

Använda meddelanden från enhet till moln för att skicka tid serien telemetri och aviseringar från din enhet och moln till enhet meddelanden för enkelriktade meddelanden tooyour enheten appen.

* Se för[enhet till moln kommunikation vägledning] [ lnk-d2c-guidance] om osäkra mellan med hjälp av meddelanden från enhet till moln, rapporterade egenskaper eller ladda upp filen.
* Se för[moln till enhet kommunikation vägledning] [ lnk-c2d-guidance] om osäkra mellan att använda moln till enhet meddelanden, önskade egenskaper eller direkt metoder.

## <a name="next-steps"></a>Nästa steg

* Lär dig mer om IoT-hubb [enhet till moln messaging][lnk-d2c].
* Lär dig mer om IoT-hubb [moln till enhet messaging][lnk-c2d].

[lnk-azure-iot]: iot-hub-what-is-azure-iot.md
[lnk-iot-hub-overview]: iot-hub-what-is-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md