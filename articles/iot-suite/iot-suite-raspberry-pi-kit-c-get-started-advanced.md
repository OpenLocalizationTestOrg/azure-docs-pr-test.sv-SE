---
title: "Ansluta en hallon Pi till Azure IoT Suite med C för att stödja uppdateringar av inbyggd | Microsoft Docs"
description: "Använd Microsoft Azure IoT-startpaket för Raspberry Pi 3 och Azure IoT Suite. Använd C om du vill ansluta din hallon Pi till fjärranslutna övervakningslösning skicka telemetri från sensorer till molnet, och utför en fjärransluten firmware-uppdatering."
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
ms.openlocfilehash: f36f6512bb30e4b109b1bd1c3cdab10300f4edc9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-enable-remote-firmware-updates-using-c"></a><span data-ttu-id="5949d-104">Anslut din hallon Pi 3 till fjärranslutna övervakningslösning och aktivera fjärråtkomst firmware-uppdateringar med hjälp av C</span><span class="sxs-lookup"><span data-stu-id="5949d-104">Connect your Raspberry Pi 3 to the remote monitoring solution and enable remote firmware updates using C</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="5949d-105">Den här kursen visar hur du använder Microsoft Azure IoT-startpaket för hallon Pi 3:</span><span class="sxs-lookup"><span data-stu-id="5949d-105">This tutorial shows you how to use the Microsoft Azure IoT Starter Kit for Raspberry Pi 3 to:</span></span>

* <span data-ttu-id="5949d-106">Utveckla en läsare för temperatur- och fuktighetskonsekvens som kan kommunicera med molnet.</span><span class="sxs-lookup"><span data-stu-id="5949d-106">Develop a temperature and humidity reader that can communicate with the cloud.</span></span>
* <span data-ttu-id="5949d-107">Aktivera och utföra en fjärransluten inbyggd programvara update uppdatering klientprogrammet på hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="5949d-107">Enable and perform a remote firmware update to update the client application on the Raspberry Pi.</span></span>

<span data-ttu-id="5949d-108">I självstudiekursen används:</span><span class="sxs-lookup"><span data-stu-id="5949d-108">The tutorial uses:</span></span>

* <span data-ttu-id="5949d-109">Raspbian OS programmeringsspråket C och Microsoft Azure IoT-SDK för C att implementera en exempel-enhet.</span><span class="sxs-lookup"><span data-stu-id="5949d-109">Raspbian OS, the C programming language, and the Microsoft Azure IoT SDK for C to implement a sample device.</span></span>
* <span data-ttu-id="5949d-110">IoT Suite fjärråtkomst övervakning förkonfigurerade lösning som molnbaserade serverdelen.</span><span class="sxs-lookup"><span data-stu-id="5949d-110">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="5949d-111">Översikt</span><span class="sxs-lookup"><span data-stu-id="5949d-111">Overview</span></span>

<span data-ttu-id="5949d-112">I den här kursen kan du utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="5949d-112">In this tutorial, you complete the following steps:</span></span>

* <span data-ttu-id="5949d-113">Distribuera en instans av fjärråtkomst övervakning förkonfigurerade lösningen till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5949d-113">Deploy an instance of the remote monitoring preconfigured solution to your Azure subscription.</span></span> <span data-ttu-id="5949d-114">Det här steget kan du automatiskt distribuerar och konfigurerar Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="5949d-114">This step automatically deploys and configures multiple Azure services.</span></span>
* <span data-ttu-id="5949d-115">Konfigurera din enhet och sensorer för kommunikation med datorn och den fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="5949d-115">Set up your device and sensors to communicate with your computer and the remote monitoring solution.</span></span>
* <span data-ttu-id="5949d-116">Uppdatera enheten exempelkod för att ansluta till den fjärranslutna övervakningslösning och skicka telemetri som kan visas på instrumentpanelen för lösningen.</span><span class="sxs-lookup"><span data-stu-id="5949d-116">Update the sample device code to connect to the remote monitoring solution, and send telemetry that you can view on the solution dashboard.</span></span>
* <span data-ttu-id="5949d-117">Använda exempelkod för enheten för att uppdatera klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="5949d-117">Use the sample device code to update the client application.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="5949d-118">Fjärråtkomst övervakningslösning etablerar en mängd olika Azure-tjänster i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5949d-118">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="5949d-119">Distributionen visar en verklig enterprise-arkitektur.</span><span class="sxs-lookup"><span data-stu-id="5949d-119">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="5949d-120">Ta bort din instans av förkonfigurerade lösningen vid azureiotsuite.com för att undvika onödiga Azure-förbrukningen avgifter, när du är klar med den.</span><span class="sxs-lookup"><span data-stu-id="5949d-120">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="5949d-121">Om du behöver den förkonfigurerade lösningen, kan du enkelt återskapa den.</span><span class="sxs-lookup"><span data-stu-id="5949d-121">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="5949d-122">Mer information om hur du minskar användning när den fjärranslutna övervakningslösning körs finns [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="5949d-122">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a><span data-ttu-id="5949d-123">Hämta och konfigurera exemplet</span><span class="sxs-lookup"><span data-stu-id="5949d-123">Download and configure the sample</span></span>

<span data-ttu-id="5949d-124">Du kan nu hämta och konfigurera fjärråtkomst övervakning klientprogrammet på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="5949d-124">You can now download and configure the remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="clone-the-repositories"></a><span data-ttu-id="5949d-125">Klona databaser</span><span class="sxs-lookup"><span data-stu-id="5949d-125">Clone the repositories</span></span>

<span data-ttu-id="5949d-126">Om du inte redan har gjort klona krävs databaser genom att köra följande kommandon på din Pi:</span><span class="sxs-lookup"><span data-stu-id="5949d-126">If you haven't done so already, clone the required repositories by running the following commands on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a><span data-ttu-id="5949d-127">Uppdatera anslutningssträngen enhet</span><span class="sxs-lookup"><span data-stu-id="5949d-127">Update the device connection string</span></span>

<span data-ttu-id="5949d-128">Öppna exempelkonfigurationsfilen i den **nano** redigeraren med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5949d-128">Open the sample configuration file in the **nano** editor using the following command:</span></span>

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

<span data-ttu-id="5949d-129">Ersätt platshållarvärdena med ID- och IoT-hubb enhetsinformationen du skapade och sparade i början av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="5949d-129">Replace the placeholder values with the device ID and IoT Hub information you created and saved at the start of this tutorial.</span></span>

<span data-ttu-id="5949d-130">När du är klar innehållet i filen deviceinfo bör se ut som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="5949d-130">When you are done, the contents of the deviceinfo file should look like the following example:</span></span>

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

<span data-ttu-id="5949d-131">Spara dina ändringar (**Ctrl-O**, **RETUR**) och avsluta redigeraren (**Ctrl + X**).</span><span class="sxs-lookup"><span data-stu-id="5949d-131">Save your changes (**Ctrl-O**, **Enter**) and exit the editor (**Ctrl-X**).</span></span>

## <a name="build-the-sample"></a><span data-ttu-id="5949d-132">Bygga exemplet</span><span class="sxs-lookup"><span data-stu-id="5949d-132">Build the sample</span></span>

<span data-ttu-id="5949d-133">Om du inte redan har gjort det, ska du installera de nödvändiga paketen för Microsoft Azure IoT-enhet SDK för C genom att köra följande kommandon i en terminal på hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="5949d-133">If you have not already done so, install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by running the following commands in a terminal on the Raspberry Pi:</span></span>

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

<span data-ttu-id="5949d-134">Du kan nu skapa provlösningen på hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="5949d-134">You can now build the sample solution on the Raspberry Pi:</span></span>

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
```

<span data-ttu-id="5949d-135">Du kan nu köra det här exempelprogrammet på hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="5949d-135">You can now run the sample program on the Raspberry Pi.</span></span> <span data-ttu-id="5949d-136">Ange kommandot:</span><span class="sxs-lookup"><span data-stu-id="5949d-136">Enter the command:</span></span>

  ```sh
  sudo ~/cmake/remote_monitoring/remote_monitoring
  ```

<span data-ttu-id="5949d-137">Följande exempel på utdata är ett exempel på utdata som du ser i Kommandotolken på hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="5949d-137">The following sample output is an example of the output you see at the command prompt on the Raspberry Pi:</span></span>

![Utdata från Raspberry Pi-app][img-raspberry-output]

<span data-ttu-id="5949d-139">Tryck på **Ctrl-C** avsluta programmet när som helst.</span><span class="sxs-lookup"><span data-stu-id="5949d-139">Press **Ctrl-C** to exit the program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-advanced.md)]

1. <span data-ttu-id="5949d-140">I lösningen instrumentpanelen, klickar du på **enheter** besöka den **enheter** sidan.</span><span class="sxs-lookup"><span data-stu-id="5949d-140">In the solution dashboard, click **Devices** to visit the **Devices** page.</span></span> <span data-ttu-id="5949d-141">Välj din Raspberry Pi i den **enhetslistan**.</span><span class="sxs-lookup"><span data-stu-id="5949d-141">Select your Raspberry Pi in the **Device List**.</span></span> <span data-ttu-id="5949d-142">Välj **metoder**:</span><span class="sxs-lookup"><span data-stu-id="5949d-142">Then choose **Methods**:</span></span>

    ![Lista över enheter i instrumentpanelen][img-list-devices]

1. <span data-ttu-id="5949d-144">På den **anropa metoden** väljer **InitiateFirmwareUpdate** i den **metoden** listrutan.</span><span class="sxs-lookup"><span data-stu-id="5949d-144">On the **Invoke Method** page, choose **InitiateFirmwareUpdate** in the **Method** dropdown.</span></span>

1. <span data-ttu-id="5949d-145">I den **FWPackageURI** anger **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**.</span><span class="sxs-lookup"><span data-stu-id="5949d-145">In the **FWPackageURI** field, enter **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**.</span></span> <span data-ttu-id="5949d-146">Det här arkivet innehåller implementeringen av version 2.0 av den inbyggda programvaran.</span><span class="sxs-lookup"><span data-stu-id="5949d-146">This archive file contains the implementation of version 2.0 of the firmware.</span></span>

1. <span data-ttu-id="5949d-147">Välj **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="5949d-147">Choose **InvokeMethod**.</span></span> <span data-ttu-id="5949d-148">Appen på hallon Pi skickar en bekräftelse tillbaka till instrumentpanelen lösning.</span><span class="sxs-lookup"><span data-stu-id="5949d-148">The app on the Raspberry Pi sends an acknowledgment back to the solution dashboard.</span></span> <span data-ttu-id="5949d-149">Därefter startar den inbyggda programvara uppdateringen genom att hämta den nya versionen av den inbyggda programvaran:</span><span class="sxs-lookup"><span data-stu-id="5949d-149">It then starts the firmware update process by downloading the new version of the firmware:</span></span>

    ![Visa historiken för metoden][img-method-history]

## <a name="observe-the-firmware-update-process"></a><span data-ttu-id="5949d-151">Se den inbyggda programvaran uppdatera process</span><span class="sxs-lookup"><span data-stu-id="5949d-151">Observe the firmware update process</span></span>

<span data-ttu-id="5949d-152">Du kan se den inbyggda programvaran uppdatera process som körs på enheten och genom att visa rapporterade egenskaperna i instrumentpanelen för lösningen:</span><span class="sxs-lookup"><span data-stu-id="5949d-152">You can observe the firmware update process as it runs on the device and by viewing the reported properties in the solution dashboard:</span></span>

1. <span data-ttu-id="5949d-153">Du kan visa förloppet på för uppdateringsprocessen på hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="5949d-153">You can view the progress in of the update process on the Raspberry Pi:</span></span>

    ![Visa förlopp för uppdatering][img-update-progress]

    > [!NOTE]
    > <span data-ttu-id="5949d-155">Fjärråtkomst övervakning appen startar om tyst när uppdateringen är klar.</span><span class="sxs-lookup"><span data-stu-id="5949d-155">The remote monitoring app restarts silently when the update completes.</span></span> <span data-ttu-id="5949d-156">Använd kommandot `ps -ef` att verifiera att den körs.</span><span class="sxs-lookup"><span data-stu-id="5949d-156">Use the command `ps -ef` to verify it is running.</span></span> <span data-ttu-id="5949d-157">Om du vill avsluta processen använder den `kill` med process-id.</span><span class="sxs-lookup"><span data-stu-id="5949d-157">If you want to terminate the process, use the `kill` command with the process id.</span></span>

1. <span data-ttu-id="5949d-158">Du kan visa statusen för firmware-uppdatering som rapporteras av enheten i lösningen portal.</span><span class="sxs-lookup"><span data-stu-id="5949d-158">You can view the status of the firmware update, as reported by the device, in the solution portal.</span></span> <span data-ttu-id="5949d-159">Följande skärmbild visar status och varaktighet för varje fas av uppdateringen och den nya versionen av inbyggd programvara:</span><span class="sxs-lookup"><span data-stu-id="5949d-159">The following screenshot shows the status and duration of each stage of the update process, and the new firmware version:</span></span>

    ![Visa jobbstatus][img-job-status]

    <span data-ttu-id="5949d-161">Om du gå tillbaka till instrumentpanelen kan du kontrollera att enheten är fortfarande skicka telemetri följande firmware-uppdatering.</span><span class="sxs-lookup"><span data-stu-id="5949d-161">If you navigate back to the dashboard, you can verify the device is still sending telemetry following the firmware update.</span></span>

> [!WARNING]
> <span data-ttu-id="5949d-162">Om du lämnar den fjärranslutna övervakningslösning som körs i ditt Azure-konto, debiteras du för den tid som den körs.</span><span class="sxs-lookup"><span data-stu-id="5949d-162">If you leave the remote monitoring solution running in your Azure account, you are billed for the time it runs.</span></span> <span data-ttu-id="5949d-163">Mer information om hur du minskar användning när den fjärranslutna övervakningslösning körs finns [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="5949d-163">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="5949d-164">Ta bort den förkonfigurerade lösningen från ditt Azure-konto när du är klar.</span><span class="sxs-lookup"><span data-stu-id="5949d-164">Delete the preconfigured solution from your Azure account when you have finished using it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5949d-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5949d-165">Next steps</span></span>

<span data-ttu-id="5949d-166">Besök den [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="5949d-166">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md