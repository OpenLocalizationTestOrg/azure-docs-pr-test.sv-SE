---
title: aaaConnect en hallon Pi tooAzure IoT Suite med Node.js toosupport firmware-uppdateringar | Microsoft Docs
description: "Använd hello startpaket för Microsoft Azure IoT för hello hallon Pi 3 och Azure IoT Suite. Använda Node.js tooconnect din hallon Pi toohello remote övervakningslösning, skicka telemetri från sensorer toohello molnet och utföra en fjärransluten firmware-uppdatering."
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
ms.openlocfilehash: 43bd3f16ee3d292cd9cffa8bfe7d4ca721e5c39c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-enable-remote-firmware-updates-using-nodejs"></a><span data-ttu-id="e24ab-104">Ansluta din fjärranslutna övervakningslösning hallon Pi 3 toohello och aktivera fjärråtkomst firmware-uppdateringar med hjälp av Node.js</span><span class="sxs-lookup"><span data-stu-id="e24ab-104">Connect your Raspberry Pi 3 toohello remote monitoring solution and enable remote firmware updates using Node.js</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="e24ab-105">Den här kursen visar hur toouse hello startpaket för Microsoft Azure IoT för hallon Pi 3:</span><span class="sxs-lookup"><span data-stu-id="e24ab-105">This tutorial shows you how toouse hello Microsoft Azure IoT Starter Kit for Raspberry Pi 3 to:</span></span>

* <span data-ttu-id="e24ab-106">Utveckla en läsare för temperatur- och fuktighetskonsekvens som kan kommunicera med hello molnet.</span><span class="sxs-lookup"><span data-stu-id="e24ab-106">Develop a temperature and humidity reader that can communicate with hello cloud.</span></span>
* <span data-ttu-id="e24ab-107">Aktivera och utföra ett klientprogram för fjärråtkomst firmware uppdatering tooupdate hello på hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="e24ab-107">Enable and perform a remote firmware update tooupdate hello client application on hello Raspberry Pi.</span></span>

<span data-ttu-id="e24ab-108">hello självstudiekursen används:</span><span class="sxs-lookup"><span data-stu-id="e24ab-108">hello tutorial uses:</span></span>

- <span data-ttu-id="e24ab-109">Raspbian OS hello Node.js programmeringsspråket och hello Microsoft Azure IoT SDK för Node.js tooimplement en exempel-enhet.</span><span class="sxs-lookup"><span data-stu-id="e24ab-109">Raspbian OS, hello Node.js programming language, and hello Microsoft Azure IoT SDK for Node.js tooimplement a sample device.</span></span>
- <span data-ttu-id="e24ab-110">Hej IoT Suite fjärrövervaknings förkonfigurerade lösning som hello molnbaserade serverdel.</span><span class="sxs-lookup"><span data-stu-id="e24ab-110">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="e24ab-111">Översikt</span><span class="sxs-lookup"><span data-stu-id="e24ab-111">Overview</span></span>

<span data-ttu-id="e24ab-112">Du har slutfört hello följa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="e24ab-112">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="e24ab-113">Distribuera en instans av enligt förkonfigurerade lösningen tooyour i hello fjärråtkomst övervakning Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e24ab-113">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="e24ab-114">Det här steget kan du automatiskt distribuerar och konfigurerar Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="e24ab-114">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="e24ab-115">Konfigurera din enhet och sensorer toocommunicate med datorn och hello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="e24ab-115">Set up your device and sensors toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="e24ab-116">Uppdatera hello exempel enhet kod tooconnect toohello remote övervakningslösning och skicka telemetri som kan visas på instrumentpanelen för hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="e24ab-116">Update hello sample device code tooconnect toohello remote monitoring solution, and send telemetry that you can view on hello solution dashboard.</span></span>
- <span data-ttu-id="e24ab-117">Använd hello enheten kod tooupdate hello klienten exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="e24ab-117">Use hello sample device code tooupdate hello client application.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="e24ab-118">hello fjärråtkomst övervakning lösning tillhandahåller en uppsättning Azure-tjänster i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e24ab-118">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="e24ab-119">hello distribution visar en verklig enterprise-arkitektur.</span><span class="sxs-lookup"><span data-stu-id="e24ab-119">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="e24ab-120">tooavoid onödiga Azure-förbrukningen kostnader, ta bort din instans av hello förkonfigurerade lösningen i azureiotsuite.com när du är klar med den.</span><span class="sxs-lookup"><span data-stu-id="e24ab-120">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="e24ab-121">Om du behöver hello förkonfigurerade lösningen igen, kan du enkelt återskapa den.</span><span class="sxs-lookup"><span data-stu-id="e24ab-121">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="e24ab-122">Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="e24ab-122">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-hello-sample"></a><span data-ttu-id="e24ab-123">Hämta och konfigurera hello-exempel</span><span class="sxs-lookup"><span data-stu-id="e24ab-123">Download and configure hello sample</span></span>

<span data-ttu-id="e24ab-124">Du kan nu hämta och konfigurera hello fjärråtkomst övervakning klientprogrammet på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="e24ab-124">You can now download and configure hello remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="install-nodejs"></a><span data-ttu-id="e24ab-125">Installera Node.js</span><span class="sxs-lookup"><span data-stu-id="e24ab-125">Install Node.js</span></span>

<span data-ttu-id="e24ab-126">Om du inte redan gjort det, kan du installera Node.js på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="e24ab-126">If you haven't done so already, install Node.js on your Raspberry Pi.</span></span> <span data-ttu-id="e24ab-127">Hej IoT SDK för Node.js kräver version 0.11.5 av Node.js eller senare.</span><span class="sxs-lookup"><span data-stu-id="e24ab-127">hello IoT SDK for Node.js requires version 0.11.5 of Node.js or later.</span></span> <span data-ttu-id="e24ab-128">hello följande steg visar hur tooinstall Node.js v6.10.2 på din hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="e24ab-128">hello following steps show you how tooinstall Node.js v6.10.2 on your Raspberry Pi:</span></span>

1. <span data-ttu-id="e24ab-129">Använd följande kommando tooupdate hello hallon-Pi:</span><span class="sxs-lookup"><span data-stu-id="e24ab-129">Use hello following command tooupdate your Raspberry Pi:</span></span>

    ```sh
    sudo apt-get update
    ```

1. <span data-ttu-id="e24ab-130">Använd följande kommando toodownload hello Node.js binärfiler tooyour hallon Pi hello:</span><span class="sxs-lookup"><span data-stu-id="e24ab-130">Use hello following command toodownload hello Node.js binaries tooyour Raspberry Pi:</span></span>

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="e24ab-131">Använd följande kommando tooinstall hello binärfiler hello:</span><span class="sxs-lookup"><span data-stu-id="e24ab-131">Use hello following command tooinstall hello binaries:</span></span>

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="e24ab-132">Använd hello följande kommando tooverify som du har installerat Node.js v6.10.2 har:</span><span class="sxs-lookup"><span data-stu-id="e24ab-132">Use hello following command tooverify you have installed Node.js v6.10.2 successfully:</span></span>

    ```sh
    node --version
    ```

### <a name="clone-hello-repositories"></a><span data-ttu-id="e24ab-133">Klona hello databaser</span><span class="sxs-lookup"><span data-stu-id="e24ab-133">Clone hello repositories</span></span>

<span data-ttu-id="e24ab-134">Om du inte redan har gjort krävs klona hello databaser genom att köra hello följande kommandon på din Pi:</span><span class="sxs-lookup"><span data-stu-id="e24ab-134">If you haven't done so already, clone hello required repositories by running hello following commands on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a><span data-ttu-id="e24ab-135">Uppdatera anslutningssträngen för hello-enhet</span><span class="sxs-lookup"><span data-stu-id="e24ab-135">Update hello device connection string</span></span>

<span data-ttu-id="e24ab-136">Öppna hello exempelkonfigurationsfilen i hello **nano** redigeraren med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e24ab-136">Open hello sample configuration file in hello **nano** editor using hello following command:</span></span>

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

<span data-ttu-id="e24ab-137">Ersätt hello platshållarvärdena med hello enhets-id och IoT-hubb information du skapade och sparade hello början av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e24ab-137">Replace hello placeholder values with hello device id and IoT Hub information you created and saved at hello start of this tutorial.</span></span>

<span data-ttu-id="e24ab-138">När du är klar hello innehållet i hello deviceinfo fil ska se ut så hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e24ab-138">When you are done, hello contents of hello deviceinfo file should look like hello following example:</span></span>

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

<span data-ttu-id="e24ab-139">Spara dina ändringar (**Ctrl-O**, **RETUR**) och avsluta hello editor (**Ctrl + X**).</span><span class="sxs-lookup"><span data-stu-id="e24ab-139">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>

## <a name="run-hello-sample"></a><span data-ttu-id="e24ab-140">Kör hello-exempel</span><span class="sxs-lookup"><span data-stu-id="e24ab-140">Run hello sample</span></span>

<span data-ttu-id="e24ab-141">Kör hello följande kommandon tooinstall hello nödvändiga paket för hello exemplet:</span><span class="sxs-lookup"><span data-stu-id="e24ab-141">Run hello following commands tooinstall hello prerequisite packages for hello sample:</span></span>

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advance/1.0
npm install
```

<span data-ttu-id="e24ab-142">Du kan nu köra hello exempelprogrammet på hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="e24ab-142">You can now run hello sample program on hello Raspberry Pi.</span></span> <span data-ttu-id="e24ab-143">Ange hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="e24ab-143">Enter hello command:</span></span>

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/1.0/remote_monitoring.js
```

<span data-ttu-id="e24ab-144">hello är följande exempel på utdata ett exempel på hello utdata som du ser i hello kommandotolk på hello hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="e24ab-144">hello following sample output is an example of hello output you see at hello command prompt on hello Raspberry Pi:</span></span>

![Utdata från Raspberry Pi-app][img-raspberry-output]

<span data-ttu-id="e24ab-146">Tryck på **Ctrl-C** tooexit hello program när som helst.</span><span class="sxs-lookup"><span data-stu-id="e24ab-146">Press **Ctrl-C** tooexit hello program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-advanced.md)]

1. <span data-ttu-id="e24ab-147">I hello lösning instrumentpanelen, klickar du på **enheter** toovisit hello **enheter** sidan.</span><span class="sxs-lookup"><span data-stu-id="e24ab-147">In hello solution dashboard, click **Devices** toovisit hello **Devices** page.</span></span> <span data-ttu-id="e24ab-148">Välj din hallon Pi i hello **enhetslistan**.</span><span class="sxs-lookup"><span data-stu-id="e24ab-148">Select your Raspberry Pi in hello **Device List**.</span></span> <span data-ttu-id="e24ab-149">Välj **metoder**:</span><span class="sxs-lookup"><span data-stu-id="e24ab-149">Then choose **Methods**:</span></span>

    ![Lista över enheter i instrumentpanelen][img-list-devices]

1. <span data-ttu-id="e24ab-151">På hello **anropa metoden** väljer **InitiateFirmwareUpdate** i hello **metoden** listrutan.</span><span class="sxs-lookup"><span data-stu-id="e24ab-151">On hello **Invoke Method** page, choose **InitiateFirmwareUpdate** in hello **Method** dropdown.</span></span>

1. <span data-ttu-id="e24ab-152">I hello **FWPackageURI** anger **https://raw.githubusercontent.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit/master/advanced/2.0/raspberry.js**.</span><span class="sxs-lookup"><span data-stu-id="e24ab-152">In hello **FWPackageURI** field, enter **https://raw.githubusercontent.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit/master/advanced/2.0/raspberry.js**.</span></span> <span data-ttu-id="e24ab-153">Den här filen innehåller hello implementering av version 2.0 av hello inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="e24ab-153">This file contains hello implementation of version 2.0 of hello firmware.</span></span>

1. <span data-ttu-id="e24ab-154">Välj **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="e24ab-154">Choose **InvokeMethod**.</span></span> <span data-ttu-id="e24ab-155">hello-appen på hello hallon Pi skickar en bekräftelse tillbaka toohello lösning instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="e24ab-155">hello app on hello Raspberry Pi sends an acknowledgment back toohello solution dashboard.</span></span> <span data-ttu-id="e24ab-156">Därefter startar hello firmware-uppdateringen genom att hämta hello ny version av inbyggd programvara hello:</span><span class="sxs-lookup"><span data-stu-id="e24ab-156">It then starts hello firmware update process by downloading hello new version of hello firmware:</span></span>

    ![Visa historiken för metoden][img-method-history]

## <a name="observe-hello-firmware-update-process"></a><span data-ttu-id="e24ab-158">Se hello uppdateringsprocessen för inbyggd programvara</span><span class="sxs-lookup"><span data-stu-id="e24ab-158">Observe hello firmware update process</span></span>

<span data-ttu-id="e24ab-159">Du kan se hello firmware uppdateringsprocessen när den körs på hello enhet och rapporterade egenskaper i hello lösning instrumentpanelen genom att visa hello:</span><span class="sxs-lookup"><span data-stu-id="e24ab-159">You can observe hello firmware update process as it runs on hello device and by viewing hello reported properties in hello solution dashboard:</span></span>

1. <span data-ttu-id="e24ab-160">Du kan visa hello förlopp i hello uppdateringsprocessen på hello hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="e24ab-160">You can view hello progress in of hello update process on hello Raspberry Pi:</span></span>

    ![Visa förlopp för uppdatering][img-update-progress]

    > [!NOTE]
    > <span data-ttu-id="e24ab-162">hello övervakning fjärrprogram startar om tyst när hello uppdateringen är klar.</span><span class="sxs-lookup"><span data-stu-id="e24ab-162">hello remote monitoring app restarts silently when hello update completes.</span></span> <span data-ttu-id="e24ab-163">Kommandot hello `ps -ef` tooverify körs.</span><span class="sxs-lookup"><span data-stu-id="e24ab-163">Use hello command `ps -ef` tooverify it is running.</span></span> <span data-ttu-id="e24ab-164">Om du vill tooterminate hello processen, Använd hello `kill` med hello process-id.</span><span class="sxs-lookup"><span data-stu-id="e24ab-164">If you want tooterminate hello process, use hello `kill` command with hello process id.</span></span>

1. <span data-ttu-id="e24ab-165">Du kan visa hello status för hello firmware-uppdatering som rapporteras av hello-enhet i hello lösning portal.</span><span class="sxs-lookup"><span data-stu-id="e24ab-165">You can view hello status of hello firmware update, as reported by hello device, in hello solution portal.</span></span> <span data-ttu-id="e24ab-166">hello följande skärmbild visar hello status och varaktighet för varje steg i hello uppdateringsprocessen och hello nya version:</span><span class="sxs-lookup"><span data-stu-id="e24ab-166">hello following screenshot shows hello status and duration of each stage of hello update process, and hello new firmware version:</span></span>

    ![Visa jobbstatus][img-job-status]

    <span data-ttu-id="e24ab-168">Om du navigerar bakåt toohello instrumentpanelen kan du kontrollera hello enheten skickat fortfarande telemetri följande hello firmware-uppdatering.</span><span class="sxs-lookup"><span data-stu-id="e24ab-168">If you navigate back toohello dashboard, you can verify hello device is still sending telemetry following hello firmware update.</span></span>

> [!WARNING]
> <span data-ttu-id="e24ab-169">Om du lämnar hello remote övervakningslösning som körs i ditt Azure-konto, debiteras du för hello gång den körs.</span><span class="sxs-lookup"><span data-stu-id="e24ab-169">If you leave hello remote monitoring solution running in your Azure account, you are billed for hello time it runs.</span></span> <span data-ttu-id="e24ab-170">Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="e24ab-170">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="e24ab-171">Ta bort hello förkonfigurerade lösningen från ditt Azure-konto när du är klar.</span><span class="sxs-lookup"><span data-stu-id="e24ab-171">Delete hello preconfigured solution from your Azure account when you have finished using it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e24ab-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e24ab-172">Next steps</span></span>

<span data-ttu-id="e24ab-173">Besök hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="e24ab-173">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
