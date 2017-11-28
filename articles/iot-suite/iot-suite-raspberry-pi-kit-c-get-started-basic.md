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
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-c"></a><span data-ttu-id="6faf4-104">Anslut din fjärranslutna övervakningslösning hallon Pi 3 toohello och skicka telemetri från en verklig sensor med hjälp av C</span><span class="sxs-lookup"><span data-stu-id="6faf4-104">Connect your Raspberry Pi 3 toohello remote monitoring solution and send telemetry from a real sensor using C</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="6faf4-105">Den här kursen visar hur toouse hello startpaket för Microsoft Azure IoT för hallon Pi 3 toodevelop en läsare för temperatur- och fuktighetskonsekvens som kan kommunicera med hello molnet.</span><span class="sxs-lookup"><span data-stu-id="6faf4-105">This tutorial shows you how toouse hello Microsoft Azure IoT Starter Kit for Raspberry Pi 3 toodevelop a temperature and humidity reader that can communicate with hello cloud.</span></span> <span data-ttu-id="6faf4-106">hello självstudiekursen används:</span><span class="sxs-lookup"><span data-stu-id="6faf4-106">hello tutorial uses:</span></span>

- <span data-ttu-id="6faf4-107">Raspbian OS hello C programmeringsspråket och hello Microsoft Azure IoT SDK för C tooimplement en exempel-enhet.</span><span class="sxs-lookup"><span data-stu-id="6faf4-107">Raspbian OS, hello C programming language, and hello Microsoft Azure IoT SDK for C tooimplement a sample device.</span></span>
- <span data-ttu-id="6faf4-108">Hej IoT Suite fjärrövervaknings förkonfigurerade lösning som hello molnbaserade serverdel.</span><span class="sxs-lookup"><span data-stu-id="6faf4-108">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="6faf4-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="6faf4-109">Overview</span></span>

<span data-ttu-id="6faf4-110">Du har slutfört hello följa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="6faf4-110">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="6faf4-111">Distribuera en instans av enligt förkonfigurerade lösningen tooyour i hello fjärråtkomst övervakning Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6faf4-111">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="6faf4-112">Det här steget kan du automatiskt distribuerar och konfigurerar Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="6faf4-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="6faf4-113">Konfigurera din enhet och sensorer toocommunicate med datorn och hello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="6faf4-113">Set up your device and sensors toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="6faf4-114">Uppdatera hello exempel enhet kod tooconnect toohello remote övervakningslösning och skicka telemetri som kan visas på instrumentpanelen för hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="6faf4-114">Update hello sample device code tooconnect toohello remote monitoring solution, and send telemetry that you can view on hello solution dashboard.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="6faf4-115">hello fjärråtkomst övervakning lösning tillhandahåller en uppsättning Azure-tjänster i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6faf4-115">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="6faf4-116">hello distribution visar en verklig enterprise-arkitektur.</span><span class="sxs-lookup"><span data-stu-id="6faf4-116">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="6faf4-117">tooavoid onödiga Azure-förbrukningen kostnader, ta bort din instans av hello förkonfigurerade lösningen i azureiotsuite.com när du är klar med den.</span><span class="sxs-lookup"><span data-stu-id="6faf4-117">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="6faf4-118">Om du behöver hello förkonfigurerade lösningen igen, kan du enkelt återskapa den.</span><span class="sxs-lookup"><span data-stu-id="6faf4-118">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="6faf4-119">Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="6faf4-119">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-hello-sample"></a><span data-ttu-id="6faf4-120">Hämta och konfigurera hello-exempel</span><span class="sxs-lookup"><span data-stu-id="6faf4-120">Download and configure hello sample</span></span>

<span data-ttu-id="6faf4-121">Du kan nu hämta och konfigurera hello fjärråtkomst övervakning klientprogrammet på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="6faf4-121">You can now download and configure hello remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="clone-hello-repositories"></a><span data-ttu-id="6faf4-122">Klona hello databaser</span><span class="sxs-lookup"><span data-stu-id="6faf4-122">Clone hello repositories</span></span>

<span data-ttu-id="6faf4-123">Om du inte redan har gjort krävs klona hello databaser genom att köra hello följande kommandon i en terminal på din Pi:</span><span class="sxs-lookup"><span data-stu-id="6faf4-123">If you haven't already done so, clone hello required repositories by running hello following commands in a terminal on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
git clone --recursive https://github.com/WiringPi/WiringPi.git
```

### <a name="update-hello-device-connection-string"></a><span data-ttu-id="6faf4-124">Uppdatera anslutningssträngen för hello-enhet</span><span class="sxs-lookup"><span data-stu-id="6faf4-124">Update hello device connection string</span></span>

<span data-ttu-id="6faf4-125">Öppna hello exempel källfilen i hello **nano** redigeraren med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6faf4-125">Open hello sample source file in hello **nano** editor using hello following command:</span></span>

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/remote_monitoring/remote_monitoring.c
```

<span data-ttu-id="6faf4-126">Leta upp hello följande rader:</span><span class="sxs-lookup"><span data-stu-id="6faf4-126">Locate hello following lines:</span></span>

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

<span data-ttu-id="6faf4-127">Ersätt hello platshållarvärdena med hello enhets- och IoT-hubb information du skapade och sparade hello början av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="6faf4-127">Replace hello placeholder values with hello device and IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="6faf4-128">Spara dina ändringar (**Ctrl-O**, **RETUR**) och avsluta hello editor (**Ctrl + X**).</span><span class="sxs-lookup"><span data-stu-id="6faf4-128">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>

## <a name="build-hello-sample"></a><span data-ttu-id="6faf4-129">Skapa hello-exempel</span><span class="sxs-lookup"><span data-stu-id="6faf4-129">Build hello sample</span></span>

<span data-ttu-id="6faf4-130">Installera nödvändiga hello-paket för hello Microsoft Azure IoT-enhet SDK för C genom att köra följande kommandon i en terminal på hello hallon Pi hello:</span><span class="sxs-lookup"><span data-stu-id="6faf4-130">Install hello prerequisite packages for hello Microsoft Azure IoT Device SDK for C by running hello following commands in a terminal on hello Raspberry Pi:</span></span>

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

<span data-ttu-id="6faf4-131">Du kan nu skapa hello uppdateras exempellösning på hello hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="6faf4-131">You can now build hello updated sample solution on hello Raspberry Pi:</span></span>

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
```

<span data-ttu-id="6faf4-132">Du kan nu köra hello exempelprogrammet på hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="6faf4-132">You can now run hello sample program on hello Raspberry Pi.</span></span> <span data-ttu-id="6faf4-133">Ange hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="6faf4-133">Enter hello command:</span></span>

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

<span data-ttu-id="6faf4-134">hello är följande exempel på utdata ett exempel på hello utdata som du ser i hello kommandotolk på hello hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="6faf4-134">hello following sample output is an example of hello output you see at hello command prompt on hello Raspberry Pi:</span></span>

![Utdata från Raspberry Pi-app][img-raspberry-output]

<span data-ttu-id="6faf4-136">Tryck på **Ctrl-C** tooexit hello program när som helst.</span><span class="sxs-lookup"><span data-stu-id="6faf4-136">Press **Ctrl-C** tooexit hello program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a><span data-ttu-id="6faf4-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6faf4-137">Next steps</span></span>

<span data-ttu-id="6faf4-138">Besök hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="6faf4-138">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-basic/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
