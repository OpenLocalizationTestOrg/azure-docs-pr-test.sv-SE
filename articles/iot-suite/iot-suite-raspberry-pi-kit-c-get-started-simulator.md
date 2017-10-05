---
title: Ansluta en hallon Pi till Azure IoT Suite med simulerade telemetri C | Microsoft Docs
description: "Använd Microsoft Azure IoT-startpaket för Raspberry Pi 3 och Azure IoT Suite. Använd C om du vill ansluta din hallon Pi till fjärranslutna övervakningslösning skicka telemetri om simulerad till molnet och svara på metoderna som anropas från instrumentpanelen lösning."
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
ms.openlocfilehash: 43b82e5fb6a309283979f23d8c87af600595bc55
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-simulated-telemetry-using-c"></a><span data-ttu-id="f1e12-104">Ansluta din hallon Pi 3 till fjärranslutna övervakningslösning och skicka simulerade telemetri med hjälp av C</span><span class="sxs-lookup"><span data-stu-id="f1e12-104">Connect your Raspberry Pi 3 to the remote monitoring solution and send simulated telemetry using C</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="f1e12-105">Den här kursen visar hur du använder hallon Pi 3 för att simulera temperatur- och fuktighetskonsekvens data som ska skickas till molnet.</span><span class="sxs-lookup"><span data-stu-id="f1e12-105">This tutorial shows you how to use the Raspberry Pi 3 to simulate temperature and humidity data to send to the cloud.</span></span> <span data-ttu-id="f1e12-106">I självstudiekursen används:</span><span class="sxs-lookup"><span data-stu-id="f1e12-106">The tutorial uses:</span></span>

- <span data-ttu-id="f1e12-107">Raspbian OS programmeringsspråket C och Microsoft Azure IoT-SDK för C att implementera en exempel-enhet.</span><span class="sxs-lookup"><span data-stu-id="f1e12-107">Raspbian OS, the C programming language, and the Microsoft Azure IoT SDK for C to implement a sample device.</span></span>
- <span data-ttu-id="f1e12-108">IoT Suite fjärråtkomst övervakning förkonfigurerade lösning som molnbaserade serverdelen.</span><span class="sxs-lookup"><span data-stu-id="f1e12-108">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="f1e12-109">Fjärråtkomst övervakningslösning etablerar en mängd olika Azure-tjänster i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f1e12-109">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="f1e12-110">Distributionen visar en verklig enterprise-arkitektur.</span><span class="sxs-lookup"><span data-stu-id="f1e12-110">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="f1e12-111">Ta bort din instans av förkonfigurerade lösningen vid azureiotsuite.com för att undvika onödiga Azure-förbrukningen avgifter, när du är klar med den.</span><span class="sxs-lookup"><span data-stu-id="f1e12-111">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="f1e12-112">Om du behöver den förkonfigurerade lösningen, kan du enkelt återskapa den.</span><span class="sxs-lookup"><span data-stu-id="f1e12-112">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="f1e12-113">Mer information om hur du minskar användning när den fjärranslutna övervakningslösning körs finns [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="f1e12-113">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-the-sample"></a><span data-ttu-id="f1e12-114">Hämta och konfigurera exemplet</span><span class="sxs-lookup"><span data-stu-id="f1e12-114">Download and configure the sample</span></span>

<span data-ttu-id="f1e12-115">Du kan nu hämta och konfigurera fjärråtkomst övervakning klientprogrammet på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="f1e12-115">You can now download and configure the remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="clone-the-repositories"></a><span data-ttu-id="f1e12-116">Klona databaser</span><span class="sxs-lookup"><span data-stu-id="f1e12-116">Clone the repositories</span></span>

<span data-ttu-id="f1e12-117">Om du inte redan har gjort klona krävs databaser genom att köra följande kommandon i en terminal på din Pi:</span><span class="sxs-lookup"><span data-stu-id="f1e12-117">If you haven't already done so, clone the required repositories by running the following commands in a terminal on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a><span data-ttu-id="f1e12-118">Uppdatera anslutningssträngen enhet</span><span class="sxs-lookup"><span data-stu-id="f1e12-118">Update the device connection string</span></span>

<span data-ttu-id="f1e12-119">Öppna källfilen exempel i den **nano** redigeraren med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f1e12-119">Open the sample source file in the **nano** editor using the following command:</span></span>

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/remote_monitoring/remote_monitoring.c
```

<span data-ttu-id="f1e12-120">Leta upp följande rader:</span><span class="sxs-lookup"><span data-stu-id="f1e12-120">Locate the following lines:</span></span>

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

<span data-ttu-id="f1e12-121">Ersätta platshållarvärdena med enheten och IoT-hubb information du skapade och sparade i början av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="f1e12-121">Replace the placeholder values with the device and IoT Hub information you created and saved at the start of this tutorial.</span></span> <span data-ttu-id="f1e12-122">Spara dina ändringar (**Ctrl-O**, **RETUR**) och avsluta redigeraren (**Ctrl + X**).</span><span class="sxs-lookup"><span data-stu-id="f1e12-122">Save your changes (**Ctrl-O**, **Enter**) and exit the editor (**Ctrl-X**).</span></span>

## <a name="build-the-sample"></a><span data-ttu-id="f1e12-123">Bygga exemplet</span><span class="sxs-lookup"><span data-stu-id="f1e12-123">Build the sample</span></span>

<span data-ttu-id="f1e12-124">Installera de nödvändiga paketen för Microsoft Azure IoT-enhet SDK för C genom att köra följande kommandon i en terminal på hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="f1e12-124">Install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by running the following commands in a terminal on the Raspberry Pi:</span></span>

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

<span data-ttu-id="f1e12-125">Du kan nu skapa exempellösningen uppdaterade på hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="f1e12-125">You can now build the updated sample solution on the Raspberry Pi:</span></span>

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
```

<span data-ttu-id="f1e12-126">Du kan nu köra det här exempelprogrammet på hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="f1e12-126">You can now run the sample program on the Raspberry Pi.</span></span> <span data-ttu-id="f1e12-127">Ange kommandot:</span><span class="sxs-lookup"><span data-stu-id="f1e12-127">Enter the command:</span></span>

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

<span data-ttu-id="f1e12-128">Följande exempel på utdata är ett exempel på utdata som du ser i Kommandotolken på hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="f1e12-128">The following sample output is an example of the output you see at the command prompt on the Raspberry Pi:</span></span>

![Utdata från Raspberry Pi-app][img-raspberry-output]

<span data-ttu-id="f1e12-130">Tryck på **Ctrl-C** avsluta programmet när som helst.</span><span class="sxs-lookup"><span data-stu-id="f1e12-130">Press **Ctrl-C** to exit the program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a><span data-ttu-id="f1e12-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1e12-131">Next steps</span></span>

<span data-ttu-id="f1e12-132">Besök den [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="f1e12-132">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-simulator/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
