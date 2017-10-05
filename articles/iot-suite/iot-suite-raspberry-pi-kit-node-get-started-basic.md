---
title: Ansluta en hallon Pi till Azure IoT Suite med verkliga sensorn Node.js | Microsoft Docs
description: "Använd Microsoft Azure IoT-startpaket för Raspberry Pi 3 och Azure IoT Suite. Använd Node.js för att ansluta din hallon Pi till fjärranslutna övervakningslösning skicka telemetri från sensorer till molnet och svara på metoderna som anropas från instrumentpanelen lösning."
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
ms.openlocfilehash: 91546157cc8eabf68706391ce706038d8dc5f82d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-nodejs"></a><span data-ttu-id="d8948-104">Anslut din hallon Pi 3 till fjärranslutna övervakningslösning och skicka telemetri från en verklig sensor med Node.js</span><span class="sxs-lookup"><span data-stu-id="d8948-104">Connect your Raspberry Pi 3 to the remote monitoring solution and send telemetry from a real sensor using Node.js</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="d8948-105">Den här kursen visar hur du använder Microsoft Azure IoT-startpaket för hallon Pi 3 för att utveckla en läsare för temperatur- och fuktighetskonsekvens som kan kommunicera med molnet.</span><span class="sxs-lookup"><span data-stu-id="d8948-105">This tutorial shows you how to use the Microsoft Azure IoT Starter Kit for Raspberry Pi 3 to develop a temperature and humidity reader that can communicate with the cloud.</span></span> <span data-ttu-id="d8948-106">I självstudiekursen används:</span><span class="sxs-lookup"><span data-stu-id="d8948-106">The tutorial uses:</span></span>

- <span data-ttu-id="d8948-107">Raspbian OS programmeringsspråket Node.js och Microsoft Azure IoT-SDK för Node.js att implementera en exempel-enhet.</span><span class="sxs-lookup"><span data-stu-id="d8948-107">Raspbian OS, the Node.js programming language, and the Microsoft Azure IoT SDK for Node.js to implement a sample device.</span></span>
- <span data-ttu-id="d8948-108">IoT Suite fjärråtkomst övervakning förkonfigurerade lösning som molnbaserade serverdelen.</span><span class="sxs-lookup"><span data-stu-id="d8948-108">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="d8948-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="d8948-109">Overview</span></span>

<span data-ttu-id="d8948-110">I den här kursen kan du utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8948-110">In this tutorial, you complete the following steps:</span></span>

- <span data-ttu-id="d8948-111">Distribuera en instans av fjärråtkomst övervakning förkonfigurerade lösningen till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d8948-111">Deploy an instance of the remote monitoring preconfigured solution to your Azure subscription.</span></span> <span data-ttu-id="d8948-112">Det här steget kan du automatiskt distribuerar och konfigurerar Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="d8948-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="d8948-113">Konfigurera din enhet och sensorer för kommunikation med datorn och den fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="d8948-113">Set up your device and sensors to communicate with your computer and the remote monitoring solution.</span></span>
- <span data-ttu-id="d8948-114">Uppdatera enheten exempelkod för att ansluta till den fjärranslutna övervakningslösning och skicka telemetri som kan visas på instrumentpanelen för lösningen.</span><span class="sxs-lookup"><span data-stu-id="d8948-114">Update the sample device code to connect to the remote monitoring solution, and send telemetry that you can view on the solution dashboard.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="d8948-115">Fjärråtkomst övervakningslösning etablerar en mängd olika Azure-tjänster i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d8948-115">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="d8948-116">Distributionen visar en verklig enterprise-arkitektur.</span><span class="sxs-lookup"><span data-stu-id="d8948-116">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="d8948-117">Ta bort din instans av förkonfigurerade lösningen vid azureiotsuite.com för att undvika onödiga Azure-förbrukningen avgifter, när du är klar med den.</span><span class="sxs-lookup"><span data-stu-id="d8948-117">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="d8948-118">Om du behöver den förkonfigurerade lösningen, kan du enkelt återskapa den.</span><span class="sxs-lookup"><span data-stu-id="d8948-118">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="d8948-119">Mer information om hur du minskar användning när den fjärranslutna övervakningslösning körs finns [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="d8948-119">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a><span data-ttu-id="d8948-120">Hämta och konfigurera exemplet</span><span class="sxs-lookup"><span data-stu-id="d8948-120">Download and configure the sample</span></span>

<span data-ttu-id="d8948-121">Du kan nu hämta och konfigurera fjärråtkomst övervakning klientprogrammet på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="d8948-121">You can now download and configure the remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="install-nodejs"></a><span data-ttu-id="d8948-122">Installera Node.js</span><span class="sxs-lookup"><span data-stu-id="d8948-122">Install Node.js</span></span>

<span data-ttu-id="d8948-123">Installera Node.js på din Raspberry Pi.</span><span class="sxs-lookup"><span data-stu-id="d8948-123">Install Node.js on your Raspberry Pi.</span></span> <span data-ttu-id="d8948-124">IoT-SDK för Node.js kräver version 0.11.5 av Node.js eller senare.</span><span class="sxs-lookup"><span data-stu-id="d8948-124">The IoT SDK for Node.js requires version 0.11.5 of Node.js or later.</span></span> <span data-ttu-id="d8948-125">Följande steg visar hur du installerar Node.js v6.10.2 på din hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="d8948-125">The following steps show you how to install Node.js v6.10.2 on your Raspberry Pi:</span></span>

1. <span data-ttu-id="d8948-126">Använd följande kommando för att uppdatera din hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="d8948-126">Use the following command to update your Raspberry Pi:</span></span>

    ```sh
    sudo apt-get update
    ```

1. <span data-ttu-id="d8948-127">Använd följande kommando för att ladda ned Node.js-binärfiler till din hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="d8948-127">Use the following command to download the Node.js binaries to your Raspberry Pi:</span></span>

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="d8948-128">Använd följande kommando för att installera binärfilerna:</span><span class="sxs-lookup"><span data-stu-id="d8948-128">Use the following command to install the binaries:</span></span>

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="d8948-129">Använd följande kommando för att verifiera att du har installerat Node.js v6.10.2 har:</span><span class="sxs-lookup"><span data-stu-id="d8948-129">Use the following command to verify you have installed Node.js v6.10.2 successfully:</span></span>

    ```sh
    node --version
    ```

### <a name="clone-the-repositories"></a><span data-ttu-id="d8948-130">Klona databaser</span><span class="sxs-lookup"><span data-stu-id="d8948-130">Clone the repositories</span></span>

<span data-ttu-id="d8948-131">Om du inte redan har gjort klona krävs databaser genom att köra följande kommandon på din Pi:</span><span class="sxs-lookup"><span data-stu-id="d8948-131">If you haven't already done so, clone the required repositories by running the following commands on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git`
```

### <a name="update-the-device-connection-string"></a><span data-ttu-id="d8948-132">Uppdatera anslutningssträngen enhet</span><span class="sxs-lookup"><span data-stu-id="d8948-132">Update the device connection string</span></span>

<span data-ttu-id="d8948-133">Öppna källfilen exempel i den **nano** redigeraren med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d8948-133">Open the sample source file in the **nano** editor using the following command:</span></span>

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

<span data-ttu-id="d8948-134">Hitta raden:</span><span class="sxs-lookup"><span data-stu-id="d8948-134">Find the line:</span></span>

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

<span data-ttu-id="d8948-135">Ersätta platshållarvärdena med enheten och IoT-hubb information du skapade och sparade i början av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="d8948-135">Replace the placeholder values with the device and IoT Hub information you created and saved at the start of this tutorial.</span></span> <span data-ttu-id="d8948-136">Spara dina ändringar (**Ctrl-O**, **RETUR**) och avsluta redigeraren (**Ctrl + X**).</span><span class="sxs-lookup"><span data-stu-id="d8948-136">Save your changes (**Ctrl-O**, **Enter**) and exit the editor (**Ctrl-X**).</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="d8948-137">Köra exemplet</span><span class="sxs-lookup"><span data-stu-id="d8948-137">Run the sample</span></span>

<span data-ttu-id="d8948-138">Kör följande kommandon för att installera de nödvändiga paketen för:</span><span class="sxs-lookup"><span data-stu-id="d8948-138">Run the following commands to install the prerequisite packages for the sample:</span></span>

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic
npm install
```

<span data-ttu-id="d8948-139">Du kan nu köra det här exempelprogrammet på hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="d8948-139">You can now run the sample program on the Raspberry Pi.</span></span> <span data-ttu-id="d8948-140">Ange kommandot:</span><span class="sxs-lookup"><span data-stu-id="d8948-140">Enter the command:</span></span>

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

<span data-ttu-id="d8948-141">Följande exempel på utdata är ett exempel på utdata som du ser i Kommandotolken på hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="d8948-141">The following sample output is an example of the output you see at the command prompt on the Raspberry Pi:</span></span>

![Utdata från Raspberry Pi-app][img-raspberry-output]

<span data-ttu-id="d8948-143">Tryck på **Ctrl-C** avsluta programmet när som helst.</span><span class="sxs-lookup"><span data-stu-id="d8948-143">Press **Ctrl-C** to exit the program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a><span data-ttu-id="d8948-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d8948-144">Next steps</span></span>

<span data-ttu-id="d8948-145">Besök den [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="d8948-145">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-basic/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
