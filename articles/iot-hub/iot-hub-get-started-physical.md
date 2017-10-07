---
title: "Komma igång med att ansluta fysiska enheter tooAzure IoT-hubb | Microsoft Docs"
description: "Lär dig hur tooconnect fysiska enheter och anslagstavlor tooAzure IoT-hubb. Enheterna kan skicka telemetri tooIoT NAV- och IoT-hubb kan övervaka och hantera dina enheter."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: "självstudiekurs för Azure iot-hubb"
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: 47ce289c438b2f495d499d724c38ddc4b3307425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-get-started-with-physical-devices-tutorials"></a>Azure IoT-hubb Kom igång med fysiska enheter självstudier

Dessa självstudiekurser införa tooAzure IoT-hubb och hello enhet SDK: er. hello självstudier beskriver vanliga IoT scenarier toodemonstrate hello funktionerna i IoT-hubb. hello självstudier också illustrerar hur toocombine IoT-hubb med andra Azure-tjänster och verktyg toobuild mer kraftfulla IoT-lösningar. hello självstudier som anges i hello följande tabell visar du hur toocreate fysiska IoT-enheter.

| IoT-enhet                       | Programmeringsspråk |
|---------------------------------|----------------------|
| Raspberry Pi                    | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IoT DevKit                      | [Arduino i VSCode][DevKit]     |
| Intel Edison                    | [Node.js][Ed_Nd], [C][Ed_C]           |
| Adafruit ludd HUZZAH ESP8266 | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 sak Dev      | [Arduino][Th_Ard]              |
| Adafruit ludd M0             | [Arduino][M0_Ard]              |

Dessutom kan du använda en IoT-Edge gateway tooenable enheter tooconnect tooyour IoT-hubb.

| Gateway-enhet               | Programmeringsspråk | Plattform         |
|------------------------------|----------------------|------------------|
| Intel NUC (model DE3815TYKE) | C                    | [Vind floden Linux][NUC_Lnx] |

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
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
