---
title: aaaConnect hallon Pi-tooAzure IoT Suite med verkliga sensorn Node.js | Microsoft Docs
description: "Använd hello startpaket för Microsoft Azure IoT för hello hallon Pi 3 och Azure IoT Suite. Använda Node.js tooconnect din hallon Pi toohello remote övervakningslösning, skicka telemetri från sensorer toohello molnet och åtgärda toomethods anropas från hello lösning instrumentpanelen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 7ffb4a7a8c04b424a1f29170f4739d89f39a2429
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-nodejs"></a>Anslut din fjärranslutna övervakningslösning hallon Pi 3 toohello och skicka telemetri från en verklig sensor med Node.js

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

Den här kursen visar hur toouse hello startpaket för Microsoft Azure IoT för hallon Pi 3 toodevelop en läsare för temperatur- och fuktighetskonsekvens som kan kommunicera med hello molnet. hello självstudiekursen används:

- Raspbian OS hello Node.js programmeringsspråket och hello Microsoft Azure IoT SDK för Node.js tooimplement en exempel-enhet.
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

### <a name="install-nodejs"></a>Installera Node.js

Installera Node.js på din Raspberry Pi. Hej IoT SDK för Node.js kräver version 0.11.5 av Node.js eller senare. hello följande steg visar hur tooinstall Node.js v6.10.2 på din hallon Pi:

1. Använd följande kommando tooupdate hello hallon-Pi:

    ```sh
    sudo apt-get update
    ```

1. Använd följande kommando toodownload hello Node.js binärfiler tooyour hallon Pi hello:

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. Använd följande kommando tooinstall hello binärfiler hello:

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. Använd hello följande kommando tooverify som du har installerat Node.js v6.10.2 har:

    ```sh
    node --version
    ```

### <a name="clone-hello-repositories"></a>Klona hello databaser

Om du inte redan har gjort krävs klona hello databaser genom att köra hello följande kommandon på din Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git`
```

### <a name="update-hello-device-connection-string"></a>Uppdatera anslutningssträngen för hello-enhet

Öppna hello exempel källfilen i hello **nano** redigeraren med hello följande kommando:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

Hitta hello rad:

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

Ersätt hello platshållarvärdena med hello enhets- och IoT-hubb information du skapade och sparade hello början av den här kursen. Spara dina ändringar (**Ctrl-O**, **RETUR**) och avsluta hello editor (**Ctrl + X**).

## <a name="run-hello-sample"></a>Kör hello-exempel

Kör hello följande kommandon tooinstall hello nödvändiga paket för hello exemplet:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic
npm install
```

Du kan nu köra hello exempelprogrammet på hello hallon Pi. Ange hello-kommando:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

hello är följande exempel på utdata ett exempel på hello utdata som du ser i hello kommandotolk på hello hallon Pi:

![Utdata från Raspberry Pi-app][img-raspberry-output]

Tryck på **Ctrl-C** tooexit hello program när som helst.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a>Nästa steg

Besök hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-basic/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
