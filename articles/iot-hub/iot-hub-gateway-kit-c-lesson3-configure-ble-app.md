---
title: "SensorTag enhet & Azure IoT-Gateway - lektion 3: kör exempelappen | Microsoft Docs"
description: "Kör en TIVERA exempel tooreceive programdata från TIVERA SensorTag och från din IoT-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Aktivera appen, sensor övervaka app, insamling av sensor, data från sensorer, sensor data toocloud"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: b33e53a1-1df7-4412-ade1-45185aec5bef
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4a8acdeadd402ffc82d3b766e1ec03a77ddcebb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-run-a-ble-sample-application"></a><span data-ttu-id="b9011-104">Konfigurera och köra ett TIVERA exempelprogram</span><span class="sxs-lookup"><span data-stu-id="b9011-104">Configure and run a BLE sample application</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="b9011-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="b9011-105">What you will do</span></span>

- <span data-ttu-id="b9011-106">Klona hello exempel databas.</span><span class="sxs-lookup"><span data-stu-id="b9011-106">Clone hello sample repository.</span></span> 
- <span data-ttu-id="b9011-107">Ställ in hello anslutningen mellan SensorTag och Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="b9011-107">Set up hello connectivity between SensorTag and Intel NUC.</span></span> 
- <span data-ttu-id="b9011-108">Använd hello Azure CLI tooget din IoT-hubb och SensorTag information för ett exempelprogram TIVERA (Bluetooth låg energi).</span><span class="sxs-lookup"><span data-stu-id="b9011-108">Use hello Azure CLI tooget your IoT hub and SensorTag information for a BLE(Bluetooth Low Energy) sample application.</span></span> <span data-ttu-id="b9011-109">Konfigurera och köra hello TIVERA exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="b9011-109">And configure and run hello BLE sample application.</span></span> 

<span data-ttu-id="b9011-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="b9011-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="b9011-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="b9011-111">What you will learn</span></span>

<span data-ttu-id="b9011-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="b9011-112">In this article, you will learn:</span></span>

- <span data-ttu-id="b9011-113">Hur tooconfigure och kör hello TIVERA exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="b9011-113">How tooconfigure and run hello BLE sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="b9011-114">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="b9011-114">What you need</span></span>

<span data-ttu-id="b9011-115">Du måste ha slutfört</span><span class="sxs-lookup"><span data-stu-id="b9011-115">You must have successfully completed</span></span>

- [<span data-ttu-id="b9011-116">Skapa en IoT-hubb och registrera SensorTag</span><span class="sxs-lookup"><span data-stu-id="b9011-116">Create an IoT hub and register SensorTag</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="clone-hello-sample-repository-toohello-host-computer"></a><span data-ttu-id="b9011-117">Klona hello exempel databasen toohello värddatorn</span><span class="sxs-lookup"><span data-stu-id="b9011-117">Clone hello sample repository toohello host computer</span></span>

<span data-ttu-id="b9011-118">tooclone hello exempel lagringsplatsen, Följ dessa steg på värddatorn för hello:</span><span class="sxs-lookup"><span data-stu-id="b9011-118">tooclone hello sample repository, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="b9011-119">Öppna en kommandotolk i Windows eller en terminal i macOS eller Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="b9011-119">Open a Command Prompt window in Windows or open a terminal in macOS or Ubuntu.</span></span>
2. <span data-ttu-id="b9011-120">Kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="b9011-120">Run hello following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="set-up-hello-connectivity-between-sensortag-and-intel-nuc"></a><span data-ttu-id="b9011-121">Konfigurera hello anslutningar mellan SensorTag och Intel NUC</span><span class="sxs-lookup"><span data-stu-id="b9011-121">Set up hello connectivity between SensorTag and Intel NUC</span></span>

<span data-ttu-id="b9011-122">tooset upp hello anslutningar, Följ dessa steg på värddatorn för hello:</span><span class="sxs-lookup"><span data-stu-id="b9011-122">tooset up hello connectivity, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="b9011-123">Initiera hello konfigurationsfilen genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="b9011-123">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

2. <span data-ttu-id="b9011-124">Öppna `config-gateway.json` i Visual Studio-koden genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="b9011-124">Open `config-gateway.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

3. <span data-ttu-id="b9011-125">Leta upp följande kodrad hello och ersätta `[device hostname or IP address]` med hello IP-adressen eller värdnamnet namnet Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="b9011-125">Locate hello following line of code and replace `[device hostname or IP address]` with hello IP address or host name of Intel NUC.</span></span>
   <span data-ttu-id="b9011-126">![Skärmbild av config-gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span><span class="sxs-lookup"><span data-stu-id="b9011-126">![screenshot of config gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span></span>

4. <span data-ttu-id="b9011-127">Installera helper verktyg på Intel NUC genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="b9011-127">Install helper tools on Intel NUC by running hello following command:</span></span>

   ```bash
   gulp install-tools
   ```

5. <span data-ttu-id="b9011-128">Aktivera SensorTag genom att trycka på strömknappen hello som hello följande bild och hello grön Indikator blinka.</span><span class="sxs-lookup"><span data-stu-id="b9011-128">Turn on SensorTag by pressing hello power button as hello following picture, and hello green LED should blink.</span></span>

   ![Aktivera Sensor Tag](media/iot-hub-gateway-kit-lessons/lesson3/turn on_off sensortag.jpg)

6. <span data-ttu-id="b9011-130">Sök igenom SensorTag-enheter genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="b9011-130">Scan SensorTag devices by running hello following commands:</span></span>

   ```bash
   gulp discover-sensortag
   ```

7. <span data-ttu-id="b9011-131">Testa hello anslutningen mellan hello SensorTag och Intel NUC genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="b9011-131">Test hello connectivity between hello SensorTag and Intel NUC by running hello following command:</span></span>

   ```bash
   gulp test-connectivity --mac {mac address}
   ```

   <span data-ttu-id="b9011-132">Ersätt `{mac address}` med hello MAC-adress som du fick i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="b9011-132">Replace `{mac address}` with hello MAC address that you obtained in hello previous step.</span></span>

## <a name="get-hello-connection-string-of-sensortag"></a><span data-ttu-id="b9011-133">Hämta hello anslutningssträngen för SensorTag</span><span class="sxs-lookup"><span data-stu-id="b9011-133">Get hello connection string of SensorTag</span></span>

<span data-ttu-id="b9011-134">tooget hello Azure IoT hub-anslutningssträngen för SensorTag, kör följande kommando på värddatorn för hello hello:</span><span class="sxs-lookup"><span data-stu-id="b9011-134">tooget hello Azure IoT hub connection string of SensorTag, run hello following command on hello host computer:</span></span>

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

<span data-ttu-id="b9011-135">`{IoT hub name}`är hello IoT-hubbnamn som du använde.</span><span class="sxs-lookup"><span data-stu-id="b9011-135">`{IoT hub name}` is hello IoT hub name that you used.</span></span> <span data-ttu-id="b9011-136">Använd iot-gateway som hello värde för `{resource group name}` och använda mydevice som hello värdet för `{device id}` om du inte ändrar hello värdet i lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="b9011-136">Use iot-gateway as hello value of `{resource group name}` and use mydevice as hello value of `{device id}` if you didn't change hello value in Lesson 2.</span></span>

## <a name="configure-hello-ble-sample-application"></a><span data-ttu-id="b9011-137">Konfigurera hello TIVERA exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="b9011-137">Configure hello BLE sample application</span></span>

<span data-ttu-id="b9011-138">tooconfigure och kör hello TIVERA exempelprogrammet, Följ dessa steg på värddatorn för hello:</span><span class="sxs-lookup"><span data-stu-id="b9011-138">tooconfigure and run hello BLE sample application, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="b9011-139">Öppna `config-sensortag.json` i Visual Studio-koden genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="b9011-139">Open `config-sensortag.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![Skärmbild av config sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_sensortag.png)

2. <span data-ttu-id="b9011-141">Att Hej följande ersättningar i hello kod:</span><span class="sxs-lookup"><span data-stu-id="b9011-141">Make hello following replacements in hello code:</span></span>
   - <span data-ttu-id="b9011-142">Ersätt `[IoT hub name]` med hello IoT-hubbnamn som du använde.</span><span class="sxs-lookup"><span data-stu-id="b9011-142">Replace `[IoT hub name]` with hello IoT hub name that you used.</span></span>
   - <span data-ttu-id="b9011-143">Ersätt `[IoT device connection string]` med hello anslutningssträngen för SensorTag som du fick.</span><span class="sxs-lookup"><span data-stu-id="b9011-143">Replace `[IoT device connection string]` with hello connection string of SensorTag that you obtained.</span></span>
   - <span data-ttu-id="b9011-144">Ersätt `[device_mac_address]` med hello hello SensorTag som du fick MAC-adress.</span><span class="sxs-lookup"><span data-stu-id="b9011-144">Replace `[device_mac_address]` with hello MAC address of hello SensorTag that you obtained.</span></span>

3. <span data-ttu-id="b9011-145">Köra hello TIVERA exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="b9011-145">Run hello BLE sample application.</span></span>

   <span data-ttu-id="b9011-146">toorun hello TIVERA exempelprogrammet, Följ dessa steg på värddatorn för hello:</span><span class="sxs-lookup"><span data-stu-id="b9011-146">toorun hello BLE sample application, follow these steps on hello host computer:</span></span>

   1. <span data-ttu-id="b9011-147">Aktivera SensorTag.</span><span class="sxs-lookup"><span data-stu-id="b9011-147">Turn on SensorTag.</span></span>

   2. <span data-ttu-id="b9011-148">Distribuera och köra exempelprogram för hello TIVERA på Intel NUC genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="b9011-148">Deploy and run hello BLE sample application on Intel NUC by running hello following command:</span></span>
   
      ```bash
      gulp run
      ```

## <a name="verify-that-hello-ble-sample-application-works"></a><span data-ttu-id="b9011-149">Kontrollera att det fungerar hello TIVERA exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="b9011-149">Verify that hello BLE sample application works</span></span>

<span data-ttu-id="b9011-150">Du bör nu se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="b9011-150">You should now see an output like hello following:</span></span>

![TIVERA exempel på utdata från programmet](media/iot-hub-gateway-kit-lessons/lesson3/BLE_running.png)

<span data-ttu-id="b9011-152">hello exempelprogrammet håller temperatur datainsamling och skicka den tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b9011-152">hello sample application keeps collecting temperature data and sent it tooyour IoT hub.</span></span> <span data-ttu-id="b9011-153">hello exempelprogrammet avbryter automatiskt när du har skickat 40 sekunder.</span><span class="sxs-lookup"><span data-stu-id="b9011-153">hello sample application terminates automatically after sending 40 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="b9011-154">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="b9011-154">Summary</span></span>

<span data-ttu-id="b9011-155">Du har har konfigurera hello anslutningar mellan SensorTag och Intel NUC och kör ett tabell-exempelprogram som samlar in och skickar data från SensorTag tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b9011-155">You've successfully set up hello connectivity between SensorTag and Intel NUC, and run a BLE sample application which collects and sends data from SensorTag tooyour IoT hub.</span></span> <span data-ttu-id="b9011-156">Du är klar toolearn hur tooverify som har tagit emot din IoT-hubb hello data.</span><span class="sxs-lookup"><span data-stu-id="b9011-156">You're ready toolearn how tooverify that your IoT hub has received hello data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9011-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9011-157">Next steps</span></span>
[<span data-ttu-id="b9011-158">Läs meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="b9011-158">Read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)