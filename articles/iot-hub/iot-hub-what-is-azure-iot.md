---
title: "aaaAzure lösningar för Sakernas Internet (IoT Suite) | Microsoft Docs"
description: "Översikt över ett exempel IoT-lösningsarkitektur och hur det relaterar toodevices, hello Azure IoT Hub-tjänsten, SDK: er för Azure IoT-enhet, SDK: er för Azure IoT-tjänsten och andra Azure-tjänster."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a859e379-dca7-42fa-bdf6-1125c86ad140
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 2d934e3f988c530de6a242869c021712d2aa1576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Nästa steg

Azure IoT Hub är en Azure-tjänst som möjliggör säker och tillförlitlig dubbelriktad kommunikation mellan serverdelen för din lösning och flera miljoner enheter. Hello lösningens serverdel så kan:

* Ta emot telemetri i stor skala från dina enheter.
* Vidarebefordra data från dina enheter tooa dataströmmen Händelseprocessorn.
* Ta emot filöverföringar från enheter.
* Skicka meddelanden moln till enhet toospecific enheter.

Du kan använda IoT-hubb tooimplement din egen lösning tillbaka avslutas. IoT-hubb innehåller dessutom en identitet registret används tooprovision enheter, deras säkerhetsreferenser och deras rättigheter tooconnect toohello IoT-hubb. toolearn mer om IoT Hub, se [vad är IoT-hubb?] [lnk-iot-hub].

toolearn hur Azure IoT Hub aktiverar standardbaserad enhetshantering för du tooremotely hantera, konfigurera och uppdatera dina enheter finns i [översikt över hantering av enheter med IoT-hubben][lnk-device-management].

Du kan använda hello Azure IoT-enhet SDK tooimplement klientprogram på flera olika plattformar för enheter maskinvara och operativsystem. hello enhet SDK innehåller bibliotek som underlättar skicka telemetri tooan IoT-hubb och ta emot moln till enhet meddelanden. När du använder hello enheten SDK: er, du kan välja mellan flera nätverk protokoll toocommunicate med IoT-hubben. toolearn finns fler hello [information om enhet SDK][lnk-device-sdks].

tooget startade skriva kod och köra några exempel finns hello [Kom igång med IoT-hubb] [ lnk-getstarted] kursen.

Du kanske också är intresserad av [Azure IoT Suite][lnk-iot-suite], som är en samling förkonfigurerade lösningar. IoT Suite kan tooget igång snabbt och skala IoT projekt tooaddress vanliga IoT scenarier – till exempel fjärråtkomst övervakning, hantering och förutsägande underhåll.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md
