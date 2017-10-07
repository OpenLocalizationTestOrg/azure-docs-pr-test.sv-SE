---
title: "Simulerade enhet & Azure IoT Gateway - lektionen 3: kör exempelappen | Microsoft Docs"
description: "Kör en simulerad enhet exempel app toosend temperatur data tooyour IoT-hubb"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: data toocloud
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 5d051d99-9749-4150-b3c8-573b0bda9c52
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: bc2c97919e95e4e3977a8b6ac75162bf2b5017be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-run-a-simulated-device-sample-app"></a><span data-ttu-id="7e17a-104">Konfigurera och köra en simulerad enhet exempelapp</span><span class="sxs-lookup"><span data-stu-id="7e17a-104">Configure and run a simulated device sample app</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="7e17a-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="7e17a-105">What you will do</span></span>

- <span data-ttu-id="7e17a-106">Klona hello exempel databas.</span><span class="sxs-lookup"><span data-stu-id="7e17a-106">Clone hello sample repository.</span></span>
- <span data-ttu-id="7e17a-107">Använd hello Azure CLI tooget din IoT-hubb och information om logisk enhet för simulerad enhet exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="7e17a-107">Use hello Azure CLI tooget your IoT hub and logical device information for simulated device sample application.</span></span> <span data-ttu-id="7e17a-108">Konfigurera och köra hello simulerade enheten exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="7e17a-108">Configure and run hello simulated device sample application.</span></span>

<span data-ttu-id="7e17a-109">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="7e17a-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="7e17a-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="7e17a-110">What you will learn</span></span>

<span data-ttu-id="7e17a-111">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="7e17a-111">In this article, you will learn:</span></span>

- <span data-ttu-id="7e17a-112">Hur tooconfigure och kör hello simulerade enheten exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="7e17a-112">How tooconfigure and run hello simulated device sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="7e17a-113">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="7e17a-113">What you need</span></span>

<span data-ttu-id="7e17a-114">Du måste ha slutfört</span><span class="sxs-lookup"><span data-stu-id="7e17a-114">You must have successfully completed</span></span>

- [<span data-ttu-id="7e17a-115">Skapa en IoT-hubb och registrera din enhet</span><span class="sxs-lookup"><span data-stu-id="7e17a-115">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="clone-hello-sample-repository-toohello-host-computer"></a><span data-ttu-id="7e17a-116">Klona hello exempel databasen toohello värddatorn</span><span class="sxs-lookup"><span data-stu-id="7e17a-116">Clone hello sample repository toohello host computer</span></span>

<span data-ttu-id="7e17a-117">tooclone hello exempel lagringsplatsen, Följ dessa steg på värddatorn för hello:</span><span class="sxs-lookup"><span data-stu-id="7e17a-117">tooclone hello sample repository, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="7e17a-118">Öppna en kommandotolk i Windows eller en terminal i macOS eller Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="7e17a-118">Open a Command Prompt in Windows or open a terminal in macOS or Ubuntu.</span></span>
2. <span data-ttu-id="7e17a-119">Kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="7e17a-119">Run hello following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="configure-hello-simulated-device-and-your-nuc"></a><span data-ttu-id="7e17a-120">Konfigurera hello simulerade enheten och din NUC</span><span class="sxs-lookup"><span data-stu-id="7e17a-120">Configure hello simulated device and your NUC</span></span>

1. <span data-ttu-id="7e17a-121">Öppna hello konfigurationsfilen `config.json` i Visual Studio-koden genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7e17a-121">Open hello configuration file `config.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   code config.json
   ```

2. <span data-ttu-id="7e17a-122">Ersätt `"has_sensortag": true` med`"has_sensortag": false`</span><span class="sxs-lookup"><span data-stu-id="7e17a-122">Replace `"has_sensortag": true` with `"has_sensortag": false`</span></span>

   ![Konfigurationen som du inte har en TI SensorTag-enhet](media/iot-hub-gateway-kit-lessons/lesson3/config_no_sensortag.png)

3. <span data-ttu-id="7e17a-124">Initiera hello konfigurationsfilen genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="7e17a-124">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

4. <span data-ttu-id="7e17a-125">Öppna `config-gateway.json` i Visual Studio-koden genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7e17a-125">Open `config-gateway.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

5. <span data-ttu-id="7e17a-126">Leta upp följande kodrad hello och ersätta `[device hostname or IP address]` med IP-adressen eller värdnamnet namnet hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="7e17a-126">Locate hello following line of code and replace `[device hostname or IP address]` with IP address or host name of hello Intel NUC.</span></span>
   <span data-ttu-id="7e17a-127">![Skärmbild av config-gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span><span class="sxs-lookup"><span data-stu-id="7e17a-127">![screenshot of config gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span></span>

## <a name="get-hello-connection-string-of-your-iot-hub-logical-device"></a><span data-ttu-id="7e17a-128">Hämta hello anslutningssträngen för din IoT-hubb logisk enhet</span><span class="sxs-lookup"><span data-stu-id="7e17a-128">Get hello connection string of your IoT hub logical device</span></span>

<span data-ttu-id="7e17a-129">tooget hello Azure IoT hub-anslutningssträngen för din logiska enheter som kör följande kommando på värddatorn för hello hello:</span><span class="sxs-lookup"><span data-stu-id="7e17a-129">tooget hello Azure IoT hub connection string of your logical device, run hello following command on hello host computer:</span></span>

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

<span data-ttu-id="7e17a-130">`{IoT hub name}`är hello IoT-hubbnamn som du använde.</span><span class="sxs-lookup"><span data-stu-id="7e17a-130">`{IoT hub name}` is hello IoT hub name that you used.</span></span> <span data-ttu-id="7e17a-131">Använd iot-gateway som hello värde för `{resource group name}` och använda mydevice som hello värdet för `{device id}` om du inte ändrar hello värdet i lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="7e17a-131">Use iot-gateway as hello value of `{resource group name}` and use mydevice as hello value of `{device id}` if you didn't change hello value in Lesson 2.</span></span>

## <a name="configure-hello-simulated-device-cloud-upload-sample-application"></a><span data-ttu-id="7e17a-132">Konfigurera hello simulerade enheten molnet överför exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="7e17a-132">Configure hello simulated device cloud upload sample application</span></span>

<span data-ttu-id="7e17a-133">tooconfigure och kör hello simulerade enhet moln överför exempelprogrammet, Följ dessa steg på värddatorn för hello:</span><span class="sxs-lookup"><span data-stu-id="7e17a-133">tooconfigure and run hello simulated device cloud upload sample application, follow these steps on hello host computer:</span></span>

1. <span data-ttu-id="7e17a-134">Öppna `config-sensortag.json` i Visual Studio-koden genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7e17a-134">Open `config-sensortag.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![Skärmbild av config sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_simulated_device.png)

2. <span data-ttu-id="7e17a-136">Att Hej följande ersättningar i hello kod:</span><span class="sxs-lookup"><span data-stu-id="7e17a-136">Make hello following replacements in hello code:</span></span>
   - <span data-ttu-id="7e17a-137">Ersätt `[IoT hub name]` med hello IoT-hubbnamnet.</span><span class="sxs-lookup"><span data-stu-id="7e17a-137">Replace `[IoT hub name]` with hello IoT hub name.</span></span>
   - <span data-ttu-id="7e17a-138">Ersätt `[IoT device connection string]` med hello anslutningssträngen för din IoT-hubb logisk enhet.</span><span class="sxs-lookup"><span data-stu-id="7e17a-138">Replace `[IoT device connection string]` with hello connection string of your IoT hub logical device.</span></span>

3. <span data-ttu-id="7e17a-139">Kör hello program.</span><span class="sxs-lookup"><span data-stu-id="7e17a-139">Run hello application.</span></span>

   <span data-ttu-id="7e17a-140">Distribuera och köra programmet hello genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7e17a-140">Deploy and run hello application by running hello following command:</span></span>

   ```bash
   gulp run
   ```

## <a name="verify-hello-sample-application-works"></a><span data-ttu-id="7e17a-141">Verifiera hello exempel programmet fungerar</span><span class="sxs-lookup"><span data-stu-id="7e17a-141">Verify hello sample application works</span></span>

<span data-ttu-id="7e17a-142">Du bör nu se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="7e17a-142">You should now see output like hello following:</span></span>

![simulerade enheten exempel på utdata från programmet](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_simudev.png)

<span data-ttu-id="7e17a-144">hello skickar temperatur data tooyour IoT-hubben, som gäller i 40 sekunder.</span><span class="sxs-lookup"><span data-stu-id="7e17a-144">hello application sends temperature data tooyour IoT hub, which lasts for 40 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="7e17a-145">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7e17a-145">Summary</span></span>

<span data-ttu-id="7e17a-146">Du har konfigurerat och kör hello simulerade enheten molnet Överför med exempelprogrammet som skickar data tooyour IoT-hubb med simulerade enhet.</span><span class="sxs-lookup"><span data-stu-id="7e17a-146">You've successfully configured and run hello simulated device cloud upload sample application which sends data tooyour IoT hub with simulated device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e17a-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e17a-147">Next steps</span></span>
[<span data-ttu-id="7e17a-148">Läs meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="7e17a-148">Read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)