---
title: "Azure IoT-hubb - komma igång ansluter IoT-enheter toohello molntjänster | Microsoft Docs"
description: "Lär dig hur tooconnect din IoT-kort och starter kits tooAzure IoT-hubb. Enheterna kan skicka telemetri tooIoT NAV- och IoT-hubb kan övervaka och hantera dina enheter."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: "självstudiekurs för Azure iot-hubb"
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: 6dc956308009091532019ff84aec881f042f0104
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-get-started-tutorials"></a>Azure IoT-hubb komma igång Självstudier

Du kan använda Azure IoT Hub och hello Azure IoT-enhet SDK toobuild Sakernas Internet (IoT) lösningar:

* Azure IoT Hub är en helt hanterad tjänst i hello moln som ansluter, övervakar och hanterar IoT-enheter på ett säkert sätt. Använd hello Azure IoT-enhet SDK tooimplement IoT-enheter.
* Använd en IoT-gateway i mer komplexa IoT-scenarier. Till exempel där du behöver tooconsider faktorer, till exempel äldre enheter, kostnader för bandbredd, principer för säkerhet och sekretess eller edge databearbetning. I så fall kan använda du Azure IoT kant tooimplement en gateway som ansluter enheter tooyour IoT-hubb.

## <a name="what-hello-tutorials-cover"></a>Hello handledningar täcker

Dessa självstudiekurser införa tooAzure IoT-hubb och hello enhet SDK: er. hello självstudier beskriver vanliga IoT scenarier toodemonstrate hello funktionerna i IoT-hubb. hello självstudier också illustrerar hur toocombine IoT-hubb med andra Azure-tjänster och verktyg toobuild mer kraftfulla IoT-lösningar. I hello självstudier, kan du välja toouse simulerad eller verkliga IoT-enheter. Dessutom kan du lära dig hur toouse en gateway tooenable enheter tooconnect tooyour IoT-hubb.

## <a name="set-up-your-device"></a>Konfigurera enheten

Ansluta en IoT-enhet eller gateway tooAzure IoT-hubb. Du kan välja en fysisk eller simulerade enheten tooget igång:

| IoT-enhet                       | Programmeringsspråk |
|----------------------------------|----------------------|
| Raspberry Pi                     | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IoT DevKit                       | [Arduino i VSCode][DevKit]     |
| Intel Edison                     | [Node.js][Ed_Nd], [C][Ed_C]    |
| Adafruit ludd HUZZAH ESP8266  | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 sak Dev       | [Arduino][Th_Ard]              |
| Adafruit ludd M0              | [Arduino][M0_Ard]              |
| Simulerade enhet på datorn           | [.NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth] |
| Online enheten simulatorn         | [Raspberry Pi (Node.js)][Ol_Sim] |

Dessutom kan du använda en IoT-Edge gateway tooenable enheter tooconnect tooyour IoT-hubb:

| Gateway-enhet               | Programmeringsspråk | Plattform         |
|------------------------------|----------------------|------------------|
| Intel NUC (model DE3815TYKE) | C                    | [Vind floden Linux][NUC_Lnx] |
| Simulerade gateway            | C                    | [Linux][Sim_Lnx], [Windows][Sim_Win] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[Sim_NET]: iot-hub-csharp-csharp-getstarted.md
[Sim_Jav]: iot-hub-java-java-getstarted.md
[Sim_Nd]: iot-hub-node-node-getstarted.md
[Sim_Pyth]: iot-hub-python-getstarted.md
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
[Sim_Lnx]: iot-hub-linux-iot-edge-get-started.md
[Sim_Win]: iot-hub-windows-iot-edge-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md
