---
title: aaaConnect hallon Pi-tooAzure IoT Suite med verkliga sensorer C | Microsoft Docs
description: "Använd hello startpaket för Microsoft Azure IoT för hello hallon Pi 3 och Azure IoT Suite. Använd C tooconnect din hallon Pi toohello remote övervakningslösning, skicka telemetri från sensorer toohello molnet och åtgärda toomethods anropas från hello lösning instrumentpanelen."
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
ms.openlocfilehash: 7dac55ae5fde4c6f8e3ea6a7debf9a6822dc07ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-c"></a>Anslut din fjärranslutna övervakningslösning hallon Pi 3 toohello och skicka telemetri från en verklig sensor med hjälp av C

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

Den här kursen visar hur toouse hello startpaket för Microsoft Azure IoT för hallon Pi 3 toodevelop en läsare för temperatur- och fuktighetskonsekvens som kan kommunicera med hello molnet. hello självstudiekursen används:

- Raspbian OS hello C programmeringsspråket och hello Microsoft Azure IoT SDK för C tooimplement en exempel-enhet.
- Hej IoT Suite fjärrövervaknings förkonfigurerade lösning som hello molnbaserade serverdel.

## <a name="overview"></a>Översikt

Du har slutfört hello följa stegen i den här självstudiekursen:

- Distribuera en instans av enligt förkonfigurerade lösningen tooyour i hello fjärråtkomst övervakning Azure-prenumeration. Det här steget kan du automatiskt distribuerar och konfigurerar Azure-tjänster.
- Konfigurera din enhet och sensorer toocommunicate med datorn och hello remote övervakningslösning.
- Uppdatera hello exempel enhet kod tooconnect toohello remote övervakningslösning och skicka telemetri som kan visas på instrumentpanelen för hello-lösning.

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> hello fjärråtkomst övervakning lösning tillhandahåller en uppsättning Azure-tjänster i din Azure-prenumeration. hello distribution visar en verklig enterprise-arkitektur. tooavoid onödiga Azure-förbrukningen kostnader, ta bort din instans av hello förkonfigurerade lösningen i azureiotsuite.com när du är klar med den. Om du behöver hello förkonfigurerade lösningen igen, kan du enkelt återskapa den. Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-hello-sample"></a>Hämta och konfigurera hello-exempel

Du kan nu hämta och konfigurera hello fjärråtkomst övervakning klientprogrammet på din hallon Pi.

### <a name="clone-hello-repositories"></a>Klona hello databaser

Om du inte redan har gjort krävs klona hello databaser genom att köra hello följande kommandon i en terminal på din Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
git clone --recursive https://github.com/WiringPi/WiringPi.git
```

### <a name="update-hello-device-connection-string"></a>Uppdatera anslutningssträngen för hello-enhet

Öppna hello exempel källfilen i hello **nano** redigeraren med hello följande kommando:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/remote_monitoring/remote_monitoring.c
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
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
```

Du kan nu köra hello exempelprogrammet på hello hallon Pi. Ange hello-kommando:

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

hello är följande exempel på utdata ett exempel på hello utdata som du ser i hello kommandotolk på hello hallon Pi:

![Utdata från Raspberry Pi-app][img-raspberry-output]

Tryck på **Ctrl-C** tooexit hello program när som helst.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a>Nästa steg

Besök hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-basic/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
