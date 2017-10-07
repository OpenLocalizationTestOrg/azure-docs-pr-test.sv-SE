---
title: aaaConnect hallon Pi-tooAzure IoT Suite med simulerade telemetri Node.js | Microsoft Docs
description: "Använd hello startpaket för Microsoft Azure IoT för hello hallon Pi 3 och Azure IoT Suite. Använda Node.js tooconnect din hallon Pi toohello remote övervakningslösning, skicka simulerade telemetri toohello molnet och åtgärda toomethods anropas från hello lösning instrumentpanelen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: node.js
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: f65eeaa6e83fd89cdedae8fa8386a3e9ef8417d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-simulated-telemetry-using-nodejs"></a><span data-ttu-id="e2cdc-104">Ansluta din fjärranslutna övervakningslösning hallon Pi 3 toohello och skicka simulerade telemetri med Node.js</span><span class="sxs-lookup"><span data-stu-id="e2cdc-104">Connect your Raspberry Pi 3 toohello remote monitoring solution and send simulated telemetry using Node.js</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="e2cdc-105">Den här kursen visar hur toouse hello hallon Pi 3 toosimulate temperatur och fuktighet data toosend toohello cloud.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-105">This tutorial shows you how toouse hello Raspberry Pi 3 toosimulate temperature and humidity data toosend toohello cloud.</span></span> <span data-ttu-id="e2cdc-106">hello självstudiekursen används:</span><span class="sxs-lookup"><span data-stu-id="e2cdc-106">hello tutorial uses:</span></span>

- <span data-ttu-id="e2cdc-107">Raspbian OS hello Node.js programmeringsspråket och hello Microsoft Azure IoT SDK för Node.js tooimplement en exempel-enhet.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-107">Raspbian OS, hello Node.js programming language, and hello Microsoft Azure IoT SDK for Node.js tooimplement a sample device.</span></span>
- <span data-ttu-id="e2cdc-108">Hej IoT Suite fjärrövervaknings förkonfigurerade lösning som hello molnbaserade serverdel.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-108">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="e2cdc-109">hello fjärråtkomst övervakning lösning tillhandahåller en uppsättning Azure-tjänster i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-109">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="e2cdc-110">hello distribution visar en verklig enterprise-arkitektur.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-110">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="e2cdc-111">tooavoid onödiga Azure-förbrukningen kostnader, ta bort din instans av hello förkonfigurerade lösningen i azureiotsuite.com när du är klar med den.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-111">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="e2cdc-112">Om du behöver hello förkonfigurerade lösningen igen, kan du enkelt återskapa den.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-112">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="e2cdc-113">Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="e2cdc-113">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-hello-sample"></a><span data-ttu-id="e2cdc-114">Hämta och konfigurera hello-exempel</span><span class="sxs-lookup"><span data-stu-id="e2cdc-114">Download and configure hello sample</span></span>

<span data-ttu-id="e2cdc-115">Du kan nu hämta och konfigurera hello fjärråtkomst övervakning klientprogrammet på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-115">You can now download and configure hello remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="install-nodejs"></a><span data-ttu-id="e2cdc-116">Installera Node.js</span><span class="sxs-lookup"><span data-stu-id="e2cdc-116">Install Node.js</span></span>

<span data-ttu-id="e2cdc-117">Om du inte redan gjort det, kan du installera Node.js på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-117">If you haven't done so already, install Node.js on your Raspberry Pi.</span></span> <span data-ttu-id="e2cdc-118">Hej IoT SDK för Node.js kräver version 0.11.5 av Node.js eller senare.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-118">hello IoT SDK for Node.js requires version 0.11.5 of Node.js or later.</span></span> <span data-ttu-id="e2cdc-119">hello följande steg visar hur tooinstall Node.js v6.10.2 på din hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="e2cdc-119">hello following steps show you how tooinstall Node.js v6.10.2 on your Raspberry Pi:</span></span>

1. <span data-ttu-id="e2cdc-120">Använd följande kommando tooupdate hello hallon-Pi:</span><span class="sxs-lookup"><span data-stu-id="e2cdc-120">Use hello following command tooupdate your Raspberry Pi:</span></span>

    ```sh
    sudo apt-get update
    ```

1. <span data-ttu-id="e2cdc-121">Använd följande kommando toodownload hello Node.js binärfiler tooyour hallon Pi hello:</span><span class="sxs-lookup"><span data-stu-id="e2cdc-121">Use hello following command toodownload hello Node.js binaries tooyour Raspberry Pi:</span></span>

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="e2cdc-122">Använd följande kommando tooinstall hello binärfiler hello:</span><span class="sxs-lookup"><span data-stu-id="e2cdc-122">Use hello following command tooinstall hello binaries:</span></span>

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="e2cdc-123">Använd hello följande kommando tooverify som du har installerat Node.js v6.10.2 har:</span><span class="sxs-lookup"><span data-stu-id="e2cdc-123">Use hello following command tooverify you have installed Node.js v6.10.2 successfully:</span></span>

    ```sh
    node --version
    ```

### <a name="clone-hello-repositories"></a><span data-ttu-id="e2cdc-124">Klona hello databaser</span><span class="sxs-lookup"><span data-stu-id="e2cdc-124">Clone hello repositories</span></span>

<span data-ttu-id="e2cdc-125">Om du inte redan har gjort krävs klona hello databaser genom att köra hello följande kommandon i en terminal på din Pi:</span><span class="sxs-lookup"><span data-stu-id="e2cdc-125">If you haven't already done so, clone hello required repositories by running hello following commands in a terminal on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a><span data-ttu-id="e2cdc-126">Uppdatera anslutningssträngen för hello-enhet</span><span class="sxs-lookup"><span data-stu-id="e2cdc-126">Update hello device connection string</span></span>

<span data-ttu-id="e2cdc-127">Öppna hello exempel källfilen i hello **nano** redigeraren med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e2cdc-127">Open hello sample source file in hello **nano** editor using hello following command:</span></span>

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

<span data-ttu-id="e2cdc-128">Hitta hello rad:</span><span class="sxs-lookup"><span data-stu-id="e2cdc-128">Find hello line:</span></span>

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

<span data-ttu-id="e2cdc-129">Ersätt hello platshållarvärdena med hello enhets- och IoT-hubb information du skapade och sparade hello början av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-129">Replace hello placeholder values with hello device and IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="e2cdc-130">Spara dina ändringar (**Ctrl-O**, **RETUR**) och avsluta hello editor (**Ctrl + X**).</span><span class="sxs-lookup"><span data-stu-id="e2cdc-130">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>

## <a name="run-hello-sample"></a><span data-ttu-id="e2cdc-131">Kör hello-exempel</span><span class="sxs-lookup"><span data-stu-id="e2cdc-131">Run hello sample</span></span>

<span data-ttu-id="e2cdc-132">Kör hello följande kommandon tooinstall hello nödvändiga paket för hello exemplet:</span><span class="sxs-lookup"><span data-stu-id="e2cdc-132">Run hello following commands tooinstall hello prerequisite packages for hello sample:</span></span>

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator
npm install
```

<span data-ttu-id="e2cdc-133">Du kan nu köra hello exempelprogrammet på hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-133">You can now run hello sample program on hello Raspberry Pi.</span></span> <span data-ttu-id="e2cdc-134">Ange hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="e2cdc-134">Enter hello command:</span></span>

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

<span data-ttu-id="e2cdc-135">hello är följande exempel på utdata ett exempel på hello utdata som du ser i hello kommandotolk på hello hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="e2cdc-135">hello following sample output is an example of hello output you see at hello command prompt on hello Raspberry Pi:</span></span>

![Utdata från Raspberry Pi-app][img-raspberry-output]

<span data-ttu-id="e2cdc-137">Tryck på **Ctrl-C** tooexit hello program när som helst.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-137">Press **Ctrl-C** tooexit hello program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a><span data-ttu-id="e2cdc-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e2cdc-138">Next steps</span></span>

<span data-ttu-id="e2cdc-139">Besök hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="e2cdc-139">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-simulator/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
