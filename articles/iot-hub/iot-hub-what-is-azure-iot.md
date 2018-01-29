---
title: "Azure-lösningar för Sakernas Internet (IoT Suite) | Microsoft Docs"
description: "Översikt över ett exempel på IoT-lösningsarkitektur och hur den relaterar till enheter, Azure IoT Hub-tjänsten, Azure IoT-enhetens SDK:er, Azure IoT-tjänstens SDK:er och övriga Azure-tjänster."
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
ms.date: 09/15/2017
ms.author: dobett
ms.openlocfilehash: 417ca4b6ecc39cbdafd8e12b5360b370d0ce79fa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Nästa steg

Azure IoT Hub är en Azure-tjänst som möjliggör säker och tillförlitlig dubbelriktad kommunikation mellan serverdelen för din lösning och flera miljoner enheter. Det gör att lösningens serverdel kan:

* Ta emot telemetri i stor skala från dina enheter.
* Vidarebefordra data från dina enheter till en dataström-händelseprocessor.
* Ta emot filöverföringar från enheter.
* Skicka meddelanden från moln till enhet för specifika enheter.

Du kan använda IoT Hub för att implementera din egen backend-lösning. IoT Hub tillhandahåller även ett identitetsregister som används för att etablera enheter och deras säkerhetsreferenser, samt deras behörigheter för anslutning till IoT-hubben. Läs mer om IoT Hub i [Vad är IoT Hub][lnk-iot-hub].

Information om hur Azure IoT Hub tillhandahåller standardbaserad enhetshantering som hjälper dig att hantera enheter finns i [Översikt över enhetshantering med IoT Hub][lnk-device-management].

Om du vill implementera klientprogram på många olika enhetsspecifika maskinvaruplattformar och operativsystem kan du använda enhets-SDK:er för Azure IoT. Enhets-SDK:erna innehåller bibliotek som gör det lätt att skicka telemetri till en IoT-hubb och att ta emot meddelanden från molnet till enheten. När du använder enhets-SDK:erna kan du välja mellan olika nätverksprotokoll för att kommunicera med IoT Hub. Mer information finns i [informationen om enhets-SDK:er][lnk-device-sdks].

Information om hur du börjar skriva kod och hur du kan köra några exempel finns i självstudien [Komma igång med IoT Hub][lnk-getstarted].

Du kanske också är intresserad av [Azure IoT Suite][lnk-iot-suite], som är en samling förkonfigurerade lösningar. Med IoT Suite kan du snabbt komma igång och skala IoT-projekt för att hantera vanliga IoT-scenarier – till exempel fjärrövervakning, tillgångshantering och förebyggande underhåll.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md
