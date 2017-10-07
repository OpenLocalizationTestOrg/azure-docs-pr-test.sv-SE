---
title: aaaUse en IoT-gateway tooconnect enhet-tooAzure IoT-hubb | Microsoft Docs
description: "Lär dig hur toouse Intel NUC som en IoT-gateway tooconnect en TI SensorTag och skicka sensor data tooAzure IoT-hubb i hello cloud."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT-gatewayen ansluta enheten toocloud
ms.assetid: cb851648-018c-4a7e-860f-b62ed3b493a5
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: 418af34bf29992d46b76ae59ef548744808664c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-iot-gateway-tooconnect-things-toohello-cloud---sensortag-tooazure-iot-hub"></a><span data-ttu-id="6f60e-104">Använd IoT gateway tooconnect saker toohello cloud - SensorTag tooAzure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="6f60e-104">Use IoT gateway tooconnect things toohello cloud - SensorTag tooAzure IoT Hub</span></span>

> [!NOTE]
> <span data-ttu-id="6f60e-105">Innan du börjar den här självstudiekursen, kontrollera att du har slutfört [konfigurera Intel NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md).</span><span class="sxs-lookup"><span data-stu-id="6f60e-105">Before you start this tutorial, make sure you’ve completed [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md).</span></span> <span data-ttu-id="6f60e-106">I [konfigurera Intel NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), du ställa in hello Intel NUC enhet som en IoT-gateway.</span><span class="sxs-lookup"><span data-stu-id="6f60e-106">In [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), you set up hello Intel NUC device as an IoT gateway.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="6f60e-107">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="6f60e-107">What you will learn</span></span>

<span data-ttu-id="6f60e-108">Du lär dig hur toouse en Texas instrument SensorTag (CC2650STK) tooAzure IoT-hubb för tooconnect en IoT-gateway.</span><span class="sxs-lookup"><span data-stu-id="6f60e-108">You learn how toouse an IoT gateway tooconnect a Texas Instruments SensorTag (CC2650STK) tooAzure IoT Hub.</span></span> <span data-ttu-id="6f60e-109">Hej IoT gateway skickar temperatur och fuktighet data som samlas in från hello SensorTag tooAzure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6f60e-109">hello IoT gateway sends temperature and humidity data collected from hello SensorTag tooAzure IoT Hub.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="6f60e-110">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="6f60e-110">What you will do</span></span>

- <span data-ttu-id="6f60e-111">Skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6f60e-111">Create an IoT hub.</span></span>
- <span data-ttu-id="6f60e-112">Registrera en enhet i hello IoT-hubb för hello SensorTag.</span><span class="sxs-lookup"><span data-stu-id="6f60e-112">Register a device in hello IoT hub for hello SensorTag.</span></span>
- <span data-ttu-id="6f60e-113">Aktivera hello-anslutning mellan hello IoT-gateway och hello SensorTag.</span><span class="sxs-lookup"><span data-stu-id="6f60e-113">Enable hello connection between hello IoT gateway and hello SensorTag.</span></span>
- <span data-ttu-id="6f60e-114">Kör en TIVERA exempel programmet toosend SensorTag data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6f60e-114">Run a BLE sample application toosend SensorTag data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6f60e-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="6f60e-115">What you need</span></span>

- <span data-ttu-id="6f60e-116">Kursen [konfigurera Intel NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) slutförts i som du konfigurerar Intel NUC som en IoT-gateway.</span><span class="sxs-lookup"><span data-stu-id="6f60e-116">Tutorial [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) completed in which you set up Intel NUC as an IoT gateway.</span></span>
- * <span data-ttu-id="6f60e-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6f60e-117">An active Azure subscription.</span></span> <span data-ttu-id="6f60e-118">Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="6f60e-118">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
- <span data-ttu-id="6f60e-119">En SSH-klient som körs på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="6f60e-119">An SSH client that runs on your host computer.</span></span> <span data-ttu-id="6f60e-120">PuTTY rekommenderas i Windows.</span><span class="sxs-lookup"><span data-stu-id="6f60e-120">PuTTY is recommended on Windows.</span></span> <span data-ttu-id="6f60e-121">Linux- och macOS har redan en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="6f60e-121">Linux and macOS already come with an SSH client.</span></span>
- <span data-ttu-id="6f60e-122">hello IP-adress och hello användarnamn och lösenord tooaccess hello gateway från hello SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="6f60e-122">hello IP address and hello username and password tooaccess hello gateway from hello SSH client.</span></span>
- <span data-ttu-id="6f60e-123">En Internetanslutning.</span><span class="sxs-lookup"><span data-stu-id="6f60e-123">An Internet connection.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

> [!NOTE]
> <span data-ttu-id="6f60e-124">Här registrerar du enheten för dina SensorTag</span><span class="sxs-lookup"><span data-stu-id="6f60e-124">Here you register this new device for your SensorTag</span></span>

## <a name="enable-hello-connection-between-hello-iot-gateway-and-hello-sensortag"></a><span data-ttu-id="6f60e-125">Aktivera hello anslutning mellan hello IoT-gateway och hello SensorTag</span><span class="sxs-lookup"><span data-stu-id="6f60e-125">Enable hello connection between hello IoT gateway and hello SensorTag</span></span>

<span data-ttu-id="6f60e-126">I det här avsnittet kan du utföra följande uppgifter hello:</span><span class="sxs-lookup"><span data-stu-id="6f60e-126">In this section, you perform hello following tasks:</span></span>

- <span data-ttu-id="6f60e-127">Hämta hello MAC-adressen för hello SensorTag för Bluetooth-anslutning.</span><span class="sxs-lookup"><span data-stu-id="6f60e-127">Get hello MAC address of hello SensorTag for Bluetooth connection.</span></span>
- <span data-ttu-id="6f60e-128">Initiera en Bluetooth-anslutning från hello IoT gateway toohello SensorTag.</span><span class="sxs-lookup"><span data-stu-id="6f60e-128">Initiate a Bluetooth connection from hello IoT gateway toohello SensorTag.</span></span>

### <a name="get-hello-mac-address-of-hello-sensortag-for-bluetooth-connection"></a><span data-ttu-id="6f60e-129">Hämta hello MAC-adressen för hello SensorTag för Bluetooth-anslutning</span><span class="sxs-lookup"><span data-stu-id="6f60e-129">Get hello MAC address of hello SensorTag for Bluetooth connection</span></span>

1. <span data-ttu-id="6f60e-130">Kör hello SSH-klienten på värddatorn för hello och ansluta toohello IoT gateway.</span><span class="sxs-lookup"><span data-stu-id="6f60e-130">On hello host computer, run hello SSH client and connect toohello IoT gateway.</span></span>
1. <span data-ttu-id="6f60e-131">Tillåt Bluetooth genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="6f60e-131">Unblock Bluetooth by running hello following command:</span></span>

   ```bash
   sudo rfkill unblock bluetooth
   ```

1. <span data-ttu-id="6f60e-132">Starta hello Bluetooth-tjänsten på hello IoT gateway och ange en Bluetooth shell tooconfigure Bluetooth genom att köra hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="6f60e-132">Start hello Bluetooth service on hello IoT gateway and enter a Bluetooth shell tooconfigure Bluetooth by running hello following commands:</span></span>

   ```bash
   sudo systemctl start bluetooth
   bluetoothctl
   ```

1. <span data-ttu-id="6f60e-133">Slå på hello Bluetooth domänkontrollant genom att köra hello följande kommando på hello Bluetooth shell:</span><span class="sxs-lookup"><span data-stu-id="6f60e-133">Power on hello Bluetooth controller by running hello following command at hello Bluetooth shell:</span></span>

   ```bash
   power on
   ```

   ![Slå på hello Bluetooth-styrenheten på hello IoT-gateway med bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/8_power-on-bluetooth-controller-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="6f60e-135">Starta sökning efter närliggande Bluetooth-enheter genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="6f60e-135">Start scanning for nearby Bluetooth devices by running hello following command:</span></span>

   ```bash
   scan on
   ```

   ![Skanna Närliggande Bluetooth-enheter med bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/9_start-scan-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="6f60e-137">Tryck på hello länkning hello SensorTag-knappen.</span><span class="sxs-lookup"><span data-stu-id="6f60e-137">Press hello pairing button on hello SensorTag.</span></span> <span data-ttu-id="6f60e-138">hello grön LETT på hello SensorTag gånger.</span><span class="sxs-lookup"><span data-stu-id="6f60e-138">hello green LED on hello SensorTag flashes.</span></span>
1. <span data-ttu-id="6f60e-139">Du bör se hello SensorTag hittas på hello Bluetooth-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="6f60e-139">At hello Bluetooth shell, you should see hello SensorTag is found.</span></span> <span data-ttu-id="6f60e-140">Anteckna hello hello SensorTag MAC-adress.</span><span class="sxs-lookup"><span data-stu-id="6f60e-140">Make a note of hello MAC address of hello SensorTag.</span></span> <span data-ttu-id="6f60e-141">I det här exemplet hello MAC-adressen för hello SensorTag är `24:71:89:C0:7F:82`.</span><span class="sxs-lookup"><span data-stu-id="6f60e-141">In this example, hello MAC address of hello SensorTag is `24:71:89:C0:7F:82`.</span></span>
1. <span data-ttu-id="6f60e-142">Inaktivera hello genomsökning genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="6f60e-142">Turn off hello scan by running hello following command:</span></span>

   ```bash
   scan off
   ```

   ![Stoppa inläsning Närliggande Bluetooth-enheter med bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/10_stop-scanning-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

### <a name="initiate-a-bluetooth-connection-from-hello-iot-gateway-toohello-sensortag"></a><span data-ttu-id="6f60e-144">Initiera en bluetoothanslutning från hello IoT gateway toohello SensorTag</span><span class="sxs-lookup"><span data-stu-id="6f60e-144">Initiate a Bluetooth connection from hello IoT gateway toohello SensorTag</span></span>

1. <span data-ttu-id="6f60e-145">Anslut toohello SensorTag genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="6f60e-145">Connect toohello SensorTag by running hello following command:</span></span>

   ```bash
   connect <MAC address>
   ```

   ![Anslut toohello SensorTag med bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/11_connect-to-sensortag-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="6f60e-147">Koppla från hello SensorTag och avsluta hello Bluetooth-gränssnittet genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="6f60e-147">Disconnect from hello SensorTag and exit hello Bluetooth shell by running hello following commands:</span></span>

   ```bash
   disconnect
   exit
   ```

   ![Koppla från hello SensorTag med bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/12_disconnect-from-sensortag-at-bluetooth-shell-bluetoothctl.png)

<span data-ttu-id="6f60e-149">Du har aktiverat hello anslutning mellan hello SensorTag och hello IoT-gateway.</span><span class="sxs-lookup"><span data-stu-id="6f60e-149">You've successfully enabled hello connection between hello SensorTag and hello IoT gateway.</span></span>

## <a name="run-a-ble-sample-application-toosend-sensortag-data-tooyour-iot-hub"></a><span data-ttu-id="6f60e-150">Kör en TIVERA exempel programmet toosend SensorTag data tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="6f60e-150">Run a BLE sample application toosend SensorTag data tooyour IoT hub</span></span>

<span data-ttu-id="6f60e-151">hello exempelprogrammet Bluetooth låg energiförbrukning (TIVERA) tillhandahålls av Azure IoT kant.</span><span class="sxs-lookup"><span data-stu-id="6f60e-151">hello Bluetooth Low Energy (BLE) sample application is provided by Azure IoT Edge.</span></span> <span data-ttu-id="6f60e-152">hello exempelprogrammet samlar in data från TIVERA anslutning och skicka hello data tooyou IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6f60e-152">hello sample application collects data from BLE connection and send hello data tooyou IoT hub.</span></span> <span data-ttu-id="6f60e-153">toorun hello exempelprogrammet, måste du:</span><span class="sxs-lookup"><span data-stu-id="6f60e-153">toorun hello sample application, you need to:</span></span>

1. <span data-ttu-id="6f60e-154">Konfigurera hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="6f60e-154">Configure hello sample application.</span></span>
1. <span data-ttu-id="6f60e-155">Kör exempelprogrammet hello på hello IoT gateway.</span><span class="sxs-lookup"><span data-stu-id="6f60e-155">Run hello sample application on hello IoT gateway.</span></span>

### <a name="configure-hello-sample-application"></a><span data-ttu-id="6f60e-156">Konfigurera hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="6f60e-156">Configure hello sample application</span></span>

1. <span data-ttu-id="6f60e-157">Gå toohello mapp hello exempelprogrammet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="6f60e-157">Go toohello folder of hello sample application by running hello following command:</span></span>

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples/ble_gateway
   ```

1. <span data-ttu-id="6f60e-158">Öppna hello konfigurationsfilen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="6f60e-158">Open hello configuration file by running hello following command:</span></span>

   ```bash
   vi ble_gateway.json
   ```

1. <span data-ttu-id="6f60e-159">Fyll i hello följande värden i hello konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="6f60e-159">In hello configuration file, fill in hello following values:</span></span>

   <span data-ttu-id="6f60e-160">**IoTHubName**: hello namnet på din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6f60e-160">**IoTHubName**: hello name of your IoT hub.</span></span>

   <span data-ttu-id="6f60e-161">**IoTHubSuffix**: hämta IoTHubSuffix från hello primärnyckeln i anslutningssträngen för hello enheten som du antecknade ned.</span><span class="sxs-lookup"><span data-stu-id="6f60e-161">**IoTHubSuffix**: Get IoTHubSuffix from hello primary key of hello device connection string that you noted down.</span></span> <span data-ttu-id="6f60e-162">Se till att du hämta hello primär nyckel för anslutningssträngen för hello enheten inte hello primärnyckel för IoT-hubb anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="6f60e-162">Ensure that you get hello primary key of hello device connection string, not hello primary key of your IoT hub connection string.</span></span> <span data-ttu-id="6f60e-163">hello primärnyckeln för anslutningssträngen för hello enheten är i hello-format för `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.</span><span class="sxs-lookup"><span data-stu-id="6f60e-163">hello primary key of hello device connection string is in hello format of `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.</span></span>

   <span data-ttu-id="6f60e-164">**Transport**: hello standardvärdet är `amqp`.</span><span class="sxs-lookup"><span data-stu-id="6f60e-164">**Transport**: hello default value is `amqp`.</span></span> <span data-ttu-id="6f60e-165">Det här värdet visar hello protokollet under transpotation.</span><span class="sxs-lookup"><span data-stu-id="6f60e-165">This value shows hello protocol during transpotation.</span></span> <span data-ttu-id="6f60e-166">Det kan vara `http`, `amqp`, eller `mqtt`.</span><span class="sxs-lookup"><span data-stu-id="6f60e-166">It could be `http`, `amqp`, or `mqtt`.</span></span>

   <span data-ttu-id="6f60e-167">**macAddress**: hello hello SensorTag som du antecknade ned MAC-adress.</span><span class="sxs-lookup"><span data-stu-id="6f60e-167">**macAddress**: hello MAC address of hello SensorTag that you noted down.</span></span>

   <span data-ttu-id="6f60e-168">**deviceID**: ID för hello-enhet som du skapade i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6f60e-168">**deviceID**: ID of hello device that you created in your IoT hub.</span></span>

   <span data-ttu-id="6f60e-169">**deviceKey**: hello primärnyckeln i anslutningssträngen för hello enhet.</span><span class="sxs-lookup"><span data-stu-id="6f60e-169">**deviceKey**: hello primary key of hello device connection string.</span></span>

   ![Fullständig hello konfigurationsfilen för hello TIVERA exempelprogrammet](./media/iot-hub-iot-gateway-connect-device-to-cloud/13_edit-config-file-of-ble-sample.png)

1. <span data-ttu-id="6f60e-171">Tryck på `ESC` och skriv `:wq` toosave hello-filen.</span><span class="sxs-lookup"><span data-stu-id="6f60e-171">Press `ESC` and type `:wq` toosave hello file.</span></span>

### <a name="run-hello-sample-application"></a><span data-ttu-id="6f60e-172">Köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="6f60e-172">Run hello sample application</span></span>

1. <span data-ttu-id="6f60e-173">Se till att hello SensorTag är påslagen.</span><span class="sxs-lookup"><span data-stu-id="6f60e-173">Make sure hello SensorTag is powered on.</span></span>
1. <span data-ttu-id="6f60e-174">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="6f60e-174">Run hello following command:</span></span>

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a><span data-ttu-id="6f60e-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6f60e-175">Next steps</span></span>

[<span data-ttu-id="6f60e-176">Använd IoT-gateway för omvandling av sensor data med Azure IoT kant</span><span class="sxs-lookup"><span data-stu-id="6f60e-176">Use IoT gateway for sensor data transformation with Azure IoT Edge</span></span>](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)
