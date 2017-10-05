---
title: "Simulerade enhet & Azure IoT Gateway - lektionen 3: kör exempelappen | Microsoft Docs"
description: "Kör en simulerad enhet exempelapp för att skicka temperatur data till din IoT-hubb"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: data till molnet
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
ms.openlocfilehash: 7df2d730c38a9f715e0fd57b4d436724a5727760
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-run-a-simulated-device-sample-app"></a><span data-ttu-id="bb441-104">Konfigurera och köra en simulerad enhet exempelapp</span><span class="sxs-lookup"><span data-stu-id="bb441-104">Configure and run a simulated device sample app</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="bb441-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="bb441-105">What you will do</span></span>

- <span data-ttu-id="bb441-106">Klona lagringsplatsen exempel.</span><span class="sxs-lookup"><span data-stu-id="bb441-106">Clone the sample repository.</span></span>
- <span data-ttu-id="bb441-107">Använda Azure CLI för att få din IoT-hubb och en logisk enhetsinformation för simulerade enheten exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="bb441-107">Use the Azure CLI to get your IoT hub and logical device information for simulated device sample application.</span></span> <span data-ttu-id="bb441-108">Konfigurera och köra exempelprogrammet simulerade enheten.</span><span class="sxs-lookup"><span data-stu-id="bb441-108">Configure and run the simulated device sample application.</span></span>

<span data-ttu-id="bb441-109">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="bb441-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="bb441-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="bb441-110">What you will learn</span></span>

<span data-ttu-id="bb441-111">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="bb441-111">In this article, you will learn:</span></span>

- <span data-ttu-id="bb441-112">Hur du konfigurerar och kör exempelprogrammet simulerade enheten.</span><span class="sxs-lookup"><span data-stu-id="bb441-112">How to configure and run the simulated device sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="bb441-113">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="bb441-113">What you need</span></span>

<span data-ttu-id="bb441-114">Du måste ha slutfört</span><span class="sxs-lookup"><span data-stu-id="bb441-114">You must have successfully completed</span></span>

- [<span data-ttu-id="bb441-115">Skapa en IoT-hubb och registrera din enhet</span><span class="sxs-lookup"><span data-stu-id="bb441-115">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="clone-the-sample-repository-to-the-host-computer"></a><span data-ttu-id="bb441-116">Klona lagringsplatsen för exempel på värddatorn</span><span class="sxs-lookup"><span data-stu-id="bb441-116">Clone the sample repository to the host computer</span></span>

<span data-ttu-id="bb441-117">Följ dessa steg om du vill klona databasen prov på värddatorn:</span><span class="sxs-lookup"><span data-stu-id="bb441-117">To clone the sample repository, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="bb441-118">Öppna en kommandotolk i Windows eller en terminal i macOS eller Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="bb441-118">Open a Command Prompt in Windows or open a terminal in macOS or Ubuntu.</span></span>
2. <span data-ttu-id="bb441-119">Kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="bb441-119">Run the following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="configure-the-simulated-device-and-your-nuc"></a><span data-ttu-id="bb441-120">Konfigurera den simulerade enheten och din NUC</span><span class="sxs-lookup"><span data-stu-id="bb441-120">Configure the simulated device and your NUC</span></span>

1. <span data-ttu-id="bb441-121">Öppna konfigurationsfilen `config.json` i Visual Studio-koden genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bb441-121">Open the configuration file `config.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   code config.json
   ```

2. <span data-ttu-id="bb441-122">Ersätt `"has_sensortag": true` med`"has_sensortag": false`</span><span class="sxs-lookup"><span data-stu-id="bb441-122">Replace `"has_sensortag": true` with `"has_sensortag": false`</span></span>

   ![Konfigurationen som du inte har en TI SensorTag-enhet](media/iot-hub-gateway-kit-lessons/lesson3/config_no_sensortag.png)

3. <span data-ttu-id="bb441-124">Initiera konfigurationsfilen genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="bb441-124">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

4. <span data-ttu-id="bb441-125">Öppna `config-gateway.json` i Visual Studio-koden genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bb441-125">Open `config-gateway.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

5. <span data-ttu-id="bb441-126">Leta upp följande rad med kod och ersätter `[device hostname or IP address]` med IP-adressen eller värdnamnet namnet Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="bb441-126">Locate the following line of code and replace `[device hostname or IP address]` with IP address or host name of the Intel NUC.</span></span>
   <span data-ttu-id="bb441-127">![Skärmbild av config-gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span><span class="sxs-lookup"><span data-stu-id="bb441-127">![screenshot of config gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span></span>

## <a name="get-the-connection-string-of-your-iot-hub-logical-device"></a><span data-ttu-id="bb441-128">Hämta anslutningssträngen för din IoT-hubb logisk enhet</span><span class="sxs-lookup"><span data-stu-id="bb441-128">Get the connection string of your IoT hub logical device</span></span>

<span data-ttu-id="bb441-129">Kör följande kommando för att hämta anslutningssträngen för Azure IoT-hubb för logiska enheten på värddatorn:</span><span class="sxs-lookup"><span data-stu-id="bb441-129">To get the Azure IoT hub connection string of your logical device, run the following command on the host computer:</span></span>

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

<span data-ttu-id="bb441-130">`{IoT hub name}`är IoT hub-namn som du använde.</span><span class="sxs-lookup"><span data-stu-id="bb441-130">`{IoT hub name}` is the IoT hub name that you used.</span></span> <span data-ttu-id="bb441-131">Använd iot-gateway som värde för `{resource group name}` och använda mydevice som värde för `{device id}` om du inte ändra värdet i lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="bb441-131">Use iot-gateway as the value of `{resource group name}` and use mydevice as the value of `{device id}` if you didn't change the value in Lesson 2.</span></span>

## <a name="configure-the-simulated-device-cloud-upload-sample-application"></a><span data-ttu-id="bb441-132">Konfigurera simulerade enheten molnet överför exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="bb441-132">Configure the simulated device cloud upload sample application</span></span>

<span data-ttu-id="bb441-133">Följ dessa steg för att konfigurera och kör exempelprogrammet simulerade enheten molnet överföringen på värddatorn:</span><span class="sxs-lookup"><span data-stu-id="bb441-133">To configure and run the simulated device cloud upload sample application, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="bb441-134">Öppna `config-sensortag.json` i Visual Studio-koden genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bb441-134">Open `config-sensortag.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![Skärmbild av config sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_simulated_device.png)

2. <span data-ttu-id="bb441-136">Gör följande ersättningar i koden:</span><span class="sxs-lookup"><span data-stu-id="bb441-136">Make the following replacements in the code:</span></span>
   - <span data-ttu-id="bb441-137">Ersätt `[IoT hub name]` med IoT-hubbnamnet.</span><span class="sxs-lookup"><span data-stu-id="bb441-137">Replace `[IoT hub name]` with the IoT hub name.</span></span>
   - <span data-ttu-id="bb441-138">Ersätt `[IoT device connection string]` med anslutningssträngen för din IoT-hubb logisk enhet.</span><span class="sxs-lookup"><span data-stu-id="bb441-138">Replace `[IoT device connection string]` with the connection string of your IoT hub logical device.</span></span>

3. <span data-ttu-id="bb441-139">Kör appen.</span><span class="sxs-lookup"><span data-stu-id="bb441-139">Run the application.</span></span>

   <span data-ttu-id="bb441-140">Distribuera och köra programmet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bb441-140">Deploy and run the application by running the following command:</span></span>

   ```bash
   gulp run
   ```

## <a name="verify-the-sample-application-works"></a><span data-ttu-id="bb441-141">Verifiera exempel programmet fungerar</span><span class="sxs-lookup"><span data-stu-id="bb441-141">Verify the sample application works</span></span>

<span data-ttu-id="bb441-142">Du bör nu se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="bb441-142">You should now see output like the following:</span></span>

![simulerade enheten exempel på utdata från programmet](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_simudev.png)

<span data-ttu-id="bb441-144">Programmet skickar temperatur data till din IoT-hubb varar 40 sekunder.</span><span class="sxs-lookup"><span data-stu-id="bb441-144">The application sends temperature data to your IoT hub, which lasts for 40 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="bb441-145">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="bb441-145">Summary</span></span>

<span data-ttu-id="bb441-146">Du har har konfigurerats och kör simulerade enheten molnet överför exempelprogrammet som skickar data till din IoT-hubb med simulerade enhet.</span><span class="sxs-lookup"><span data-stu-id="bb441-146">You've successfully configured and run the simulated device cloud upload sample application which sends data to your IoT hub with simulated device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb441-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bb441-147">Next steps</span></span>
[<span data-ttu-id="bb441-148">Läs meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="bb441-148">Read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)