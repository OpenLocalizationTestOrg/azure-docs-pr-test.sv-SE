---
title: meddelanden med iothub explorer aaaManage Azure IoT Hub moln enhet | Microsoft Docs
description: "Lär dig hur toouse hello iothub explorer CLI verktyget toomonitor toocloud (D2C) meddelanden och meddelanden moln toodevice (C2D) i Azure IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: iothub explorer moln enhet messaging iot-hubb molnet toodevice molnet toodevice meddelanden
ms.assetid: 04521658-35d3-4503-ae48-51d6ad3c62cc
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: 741383b5631714cc2fef3309541fd2117aa7824c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-iothub-explorer-toosend-and-receive-messages-between-your-device-and-iot-hub"></a>Använd iothub explorer toosend och ta emot meddelanden mellan din enhet och IoT-hubb

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[iothub explorer](https://github.com/azure/iothub-explorer) har en handfull kommandon som gör det enklare hantering av IoT-hubb. Den här självstudiekursen fokuserar på hur toouse iothub explorer toosend och ta emot meddelanden mellan enheten och din IoT-hubb.

## <a name="what-you-will-learn"></a>Vad får du lära dig

Du lär dig hur toouse iothub explorer toomonitor enhet till moln meddelanden och meddelanden som toosend moln till enhet. Meddelanden från enhet till moln kan vara sensordata att enheten samlar in och skickar sedan tooyour IoT-hubb. Moln till enhet meddelanden kan vara att din IoT-hubb skickar tooyour enhet tooblink en Indikator är anslutna tooyour enhet-kommandon.

## <a name="what-you-will-do"></a>Vad du ska göra

- Använd iothub explorer toomonitor meddelanden från enhet till moln.
- Använd iothub explorer toosend moln till enhet meddelanden.

## <a name="what-you-need"></a>Vad du behöver

- Kursen [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md) slutförts som omfattar hello följande krav:
  - En aktiv Azure-prenumeration.
  - En Azure IoT-hubb i din prenumeration.
  - Ett klientprogram som skickar meddelanden tooyour Azure IoT-hubb.
- iothub-explorer. ([Installera iothub explorer](https://github.com/azure/iothub-explorer))

## <a name="monitor-device-to-cloud-messages"></a>Övervaka meddelanden från enhet till moln

toomonitor meddelanden som skickas från din enhet tooyour IoT-hubb, Följ dessa steg:

1. Öppna ett konsolfönster.
1. Kör följande kommando hello:

   ```bash
   iothub-explorer monitor-events <device-id> --login "<IoTHubConnectionString>"
   ```

   > [!Note]
   > Hämta `<device-id>` och `<IoTHubConnectionString>` från IoT-hubb. Kontrollera att du är klar hello tidigare kursen. Eller så kan du försöka toouse `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` om du har `HostName`, `SharedAccessKeyName` och `SharedAccessKey`.

## <a name="send-cloud-to-device-messages"></a>Skicka meddelanden från moln till enhet

toosend ett meddelande från IoT-hubb tooyour enheten, Följ dessa steg:

1. Öppna ett konsolfönster.
1. Starta en session på din IoT-hubb genom att köra följande kommando hello:

   ```bash
   iothub-explorer login `<IoTHubConnectionString>`
   ```

1. Skicka ett meddelande tooyour enheten genom att köra följande kommando hello:

   ```bash
   iothub-explorer send <device-id> <message>
   ```

hello kommandot blinkar hello Indikator som är anslutna tooyour enhet och skickar hello meddelandet tooyour enhet.

> [!Note]
> Det behövs ingen hälsningspaket enheten toosend en separat ack kommandot tillbaka tooyour IoT-hubb vid mottagning av hello-meddelande.

## <a name="next-steps"></a>Nästa steg

Du har lärt dig hur toomonitor enhet till moln meddelanden och meddelanden moln till enhet mellan din IoT-enhet och Azure IoT Hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
