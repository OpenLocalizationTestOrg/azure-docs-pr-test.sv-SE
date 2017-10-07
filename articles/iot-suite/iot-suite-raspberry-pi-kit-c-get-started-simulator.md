---
title: aaaConnect hallon Pi-tooAzure IoT Suite med simulerade telemetri C | Microsoft Docs
description: "Använd hello startpaket för Microsoft Azure IoT för hello hallon Pi 3 och Azure IoT Suite. Använd C tooconnect din hallon Pi toohello remote övervakningslösning, skicka simulerade telemetri toohello molnet och åtgärda toomethods anropas från hello lösning instrumentpanelen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 3c4fa43b381385d1a7896cada34aa3aa0b8e5fb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-simulated-telemetry-using-c"></a>Ansluta din fjärranslutna övervakningslösning hallon Pi 3 toohello och skicka simulerade telemetri med hjälp av C

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

Den här kursen visar hur toouse hello hallon Pi 3 toosimulate temperatur och fuktighet data toosend toohello cloud. hello självstudiekursen används:

- Raspbian OS hello C programmeringsspråket och hello Microsoft Azure IoT SDK för C tooimplement en exempel-enhet.
- Hej IoT Suite fjärrövervaknings förkonfigurerade lösning som hello molnbaserade serverdel.

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> hello fjärråtkomst övervakning lösning tillhandahåller en uppsättning Azure-tjänster i din Azure-prenumeration. hello distribution visar en verklig enterprise-arkitektur. tooavoid onödiga Azure-förbrukningen kostnader, ta bort din instans av hello förkonfigurerade lösningen i azureiotsuite.com när du är klar med den. Om du behöver hello förkonfigurerade lösningen igen, kan du enkelt återskapa den. Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-hello-sample"></a>Hämta och konfigurera hello-exempel

Du kan nu hämta och konfigurera hello fjärråtkomst övervakning klientprogrammet på din hallon Pi.

### <a name="clone-hello-repositories"></a>Klona hello databaser

Om du inte redan har gjort krävs klona hello databaser genom att köra hello följande kommandon i en terminal på din Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a>Uppdatera anslutningssträngen för hello-enhet

Öppna hello exempel källfilen i hello **nano** redigeraren med hello följande kommando:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/remote_monitoring/remote_monitoring.c
```

Leta upp hello följande rader:

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

Ersätt hello platshållarvärdena med hello enhets- och IoT-hubb information du skapade och sparade hello början av den här kursen. Spara dina ändringar (**Ctrl-O**, **RETUR**) och avsluta hello editor (**Ctrl + X**).

## <a name="build-hello-sample"></a>Skapa hello-exempel

Installera nödvändiga hello-paket för hello Microsoft Azure IoT-enhet SDK för C genom att köra följande kommandon i en terminal på hello hallon Pi hello:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

Du kan nu skapa hello uppdateras exempellösning på hello hallon Pi:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
```

Du kan nu köra hello exempelprogrammet på hello hallon Pi. Ange hello-kommando:

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

hello är följande exempel på utdata ett exempel på hello utdata som du ser i hello kommandotolk på hello hallon Pi:

![Utdata från Raspberry Pi-app][img-raspberry-output]

Tryck på **Ctrl-C** tooexit hello program när som helst.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a>Nästa steg

Besök hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-simulator/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
