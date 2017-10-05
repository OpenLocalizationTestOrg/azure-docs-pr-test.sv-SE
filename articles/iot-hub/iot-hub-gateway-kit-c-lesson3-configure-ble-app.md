---
title: "SensorTag enhet & Azure IoT-Gateway - lektion 3: kör exempelappen | Microsoft Docs"
description: "Kör ett TIVERA exempelprogram som tar emot data från TIVERA SensorTag och från din IoT-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Aktivera appen, sensor övervaka app, insamling av sensor, data från sensorer, sensordata till molnet"
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
ms.openlocfilehash: f6fa158dbe1d48be7d493efa6217e1e0a759d2f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-run-a-ble-sample-application"></a><span data-ttu-id="eb765-104">Konfigurera och köra ett TIVERA exempelprogram</span><span class="sxs-lookup"><span data-stu-id="eb765-104">Configure and run a BLE sample application</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="eb765-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="eb765-105">What you will do</span></span>

- <span data-ttu-id="eb765-106">Klona lagringsplatsen exempel.</span><span class="sxs-lookup"><span data-stu-id="eb765-106">Clone the sample repository.</span></span> 
- <span data-ttu-id="eb765-107">Konfigurera anslutningen mellan SensorTag och Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="eb765-107">Set up the connectivity between SensorTag and Intel NUC.</span></span> 
- <span data-ttu-id="eb765-108">Använda Azure CLI för att få din IoT-hubb och SensorTag information för ett exempelprogram TIVERA (Bluetooth låg energi).</span><span class="sxs-lookup"><span data-stu-id="eb765-108">Use the Azure CLI to get your IoT hub and SensorTag information for a BLE(Bluetooth Low Energy) sample application.</span></span> <span data-ttu-id="eb765-109">Konfigurera och köra exempelprogrammet tabell.</span><span class="sxs-lookup"><span data-stu-id="eb765-109">And configure and run the BLE sample application.</span></span> 

<span data-ttu-id="eb765-110">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="eb765-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="eb765-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="eb765-111">What you will learn</span></span>

<span data-ttu-id="eb765-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="eb765-112">In this article, you will learn:</span></span>

- <span data-ttu-id="eb765-113">Hur du konfigurerar och kör exempelprogrammet tabell.</span><span class="sxs-lookup"><span data-stu-id="eb765-113">How to configure and run the BLE sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="eb765-114">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="eb765-114">What you need</span></span>

<span data-ttu-id="eb765-115">Du måste ha slutfört</span><span class="sxs-lookup"><span data-stu-id="eb765-115">You must have successfully completed</span></span>

- [<span data-ttu-id="eb765-116">Skapa en IoT-hubb och registrera SensorTag</span><span class="sxs-lookup"><span data-stu-id="eb765-116">Create an IoT hub and register SensorTag</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="clone-the-sample-repository-to-the-host-computer"></a><span data-ttu-id="eb765-117">Klona lagringsplatsen för exempel på värddatorn</span><span class="sxs-lookup"><span data-stu-id="eb765-117">Clone the sample repository to the host computer</span></span>

<span data-ttu-id="eb765-118">Följ dessa steg om du vill klona databasen prov på värddatorn:</span><span class="sxs-lookup"><span data-stu-id="eb765-118">To clone the sample repository, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="eb765-119">Öppna en kommandotolk i Windows eller en terminal i macOS eller Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="eb765-119">Open a Command Prompt window in Windows or open a terminal in macOS or Ubuntu.</span></span>
2. <span data-ttu-id="eb765-120">Kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="eb765-120">Run the following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="set-up-the-connectivity-between-sensortag-and-intel-nuc"></a><span data-ttu-id="eb765-121">Konfigurera anslutningen mellan SensorTag och Intel NUC</span><span class="sxs-lookup"><span data-stu-id="eb765-121">Set up the connectivity between SensorTag and Intel NUC</span></span>

<span data-ttu-id="eb765-122">Följ dessa steg om du vill konfigurera anslutningen på värddatorn:</span><span class="sxs-lookup"><span data-stu-id="eb765-122">To set up the connectivity, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="eb765-123">Initiera konfigurationsfilen genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="eb765-123">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

2. <span data-ttu-id="eb765-124">Öppna `config-gateway.json` i Visual Studio-koden genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="eb765-124">Open `config-gateway.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

3. <span data-ttu-id="eb765-125">Leta upp följande rad med kod och ersätter `[device hostname or IP address]` med IP-adressen eller värdnamnet namnet på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="eb765-125">Locate the following line of code and replace `[device hostname or IP address]` with the IP address or host name of Intel NUC.</span></span>
   <span data-ttu-id="eb765-126">![Skärmbild av config-gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span><span class="sxs-lookup"><span data-stu-id="eb765-126">![screenshot of config gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)</span></span>

4. <span data-ttu-id="eb765-127">Installera helper verktyg på Intel NUC genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="eb765-127">Install helper tools on Intel NUC by running the following command:</span></span>

   ```bash
   gulp install-tools
   ```

5. <span data-ttu-id="eb765-128">Aktivera SensorTag genom att trycka på strömknappen som på bilden nedan och grön Indikator blinka.</span><span class="sxs-lookup"><span data-stu-id="eb765-128">Turn on SensorTag by pressing the power button as the following picture, and the green LED should blink.</span></span>

   ![Aktivera Sensor Tag](media/iot-hub-gateway-kit-lessons/lesson3/turn on_off sensortag.jpg)

6. <span data-ttu-id="eb765-130">Sök igenom SensorTag-enheter genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="eb765-130">Scan SensorTag devices by running the following commands:</span></span>

   ```bash
   gulp discover-sensortag
   ```

7. <span data-ttu-id="eb765-131">Testa anslutningen mellan SensorTag och Intel NUC genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="eb765-131">Test the connectivity between the SensorTag and Intel NUC by running the following command:</span></span>

   ```bash
   gulp test-connectivity --mac {mac address}
   ```

   <span data-ttu-id="eb765-132">Ersätt `{mac address}` med MAC-adress som du hämtade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="eb765-132">Replace `{mac address}` with the MAC address that you obtained in the previous step.</span></span>

## <a name="get-the-connection-string-of-sensortag"></a><span data-ttu-id="eb765-133">Hämta anslutningssträngen för SensorTag</span><span class="sxs-lookup"><span data-stu-id="eb765-133">Get the connection string of SensorTag</span></span>

<span data-ttu-id="eb765-134">Kör följande kommando för att hämta anslutningssträngen för Azure IoT-hubb för SensorTag på värddatorn:</span><span class="sxs-lookup"><span data-stu-id="eb765-134">To get the Azure IoT hub connection string of SensorTag, run the following command on the host computer:</span></span>

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

<span data-ttu-id="eb765-135">`{IoT hub name}`är IoT hub-namn som du använde.</span><span class="sxs-lookup"><span data-stu-id="eb765-135">`{IoT hub name}` is the IoT hub name that you used.</span></span> <span data-ttu-id="eb765-136">Använd iot-gateway som värde för `{resource group name}` och använda mydevice som värde för `{device id}` om du inte ändra värdet i lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="eb765-136">Use iot-gateway as the value of `{resource group name}` and use mydevice as the value of `{device id}` if you didn't change the value in Lesson 2.</span></span>

## <a name="configure-the-ble-sample-application"></a><span data-ttu-id="eb765-137">Konfigurera TIVERA exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="eb765-137">Configure the BLE sample application</span></span>

<span data-ttu-id="eb765-138">Följ dessa steg för att konfigurera och köra exempelprogrammet TIVERA på värddatorn:</span><span class="sxs-lookup"><span data-stu-id="eb765-138">To configure and run the BLE sample application, follow these steps on the host computer:</span></span>

1. <span data-ttu-id="eb765-139">Öppna `config-sensortag.json` i Visual Studio-koden genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="eb765-139">Open `config-sensortag.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![Skärmbild av config sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_sensortag.png)

2. <span data-ttu-id="eb765-141">Gör följande ersättningar i koden:</span><span class="sxs-lookup"><span data-stu-id="eb765-141">Make the following replacements in the code:</span></span>
   - <span data-ttu-id="eb765-142">Ersätt `[IoT hub name]` med IoT-hubbnamn som du använde.</span><span class="sxs-lookup"><span data-stu-id="eb765-142">Replace `[IoT hub name]` with the IoT hub name that you used.</span></span>
   - <span data-ttu-id="eb765-143">Ersätt `[IoT device connection string]` med anslutningssträngen för SensorTag som du fick.</span><span class="sxs-lookup"><span data-stu-id="eb765-143">Replace `[IoT device connection string]` with the connection string of SensorTag that you obtained.</span></span>
   - <span data-ttu-id="eb765-144">Ersätt `[device_mac_address]` med MAC-adressen för SensorTag som du fick.</span><span class="sxs-lookup"><span data-stu-id="eb765-144">Replace `[device_mac_address]` with the MAC address of the SensorTag that you obtained.</span></span>

3. <span data-ttu-id="eb765-145">Kör exempelprogrammet tabell.</span><span class="sxs-lookup"><span data-stu-id="eb765-145">Run the BLE sample application.</span></span>

   <span data-ttu-id="eb765-146">Följ dessa steg om du vill köra exempelprogrammet TIVERA på värddatorn:</span><span class="sxs-lookup"><span data-stu-id="eb765-146">To run the BLE sample application, follow these steps on the host computer:</span></span>

   1. <span data-ttu-id="eb765-147">Aktivera SensorTag.</span><span class="sxs-lookup"><span data-stu-id="eb765-147">Turn on SensorTag.</span></span>

   2. <span data-ttu-id="eb765-148">Distribuera och köra exempelprogrammet TIVERA på Intel NUC genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="eb765-148">Deploy and run the BLE sample application on Intel NUC by running the following command:</span></span>
   
      ```bash
      gulp run
      ```

## <a name="verify-that-the-ble-sample-application-works"></a><span data-ttu-id="eb765-149">Kontrollera att exempelprogrammet TIVERA fungerar</span><span class="sxs-lookup"><span data-stu-id="eb765-149">Verify that the BLE sample application works</span></span>

<span data-ttu-id="eb765-150">Du bör nu se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="eb765-150">You should now see an output like the following:</span></span>

![TIVERA exempel på utdata från programmet](media/iot-hub-gateway-kit-lessons/lesson3/BLE_running.png)

<span data-ttu-id="eb765-152">Exempelprogrammet fortsätter att samla in data för temperatur och skickas till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="eb765-152">The sample application keeps collecting temperature data and sent it to your IoT hub.</span></span> <span data-ttu-id="eb765-153">Exempelprogrammet avbryter automatiskt när du har skickat 40 sekunder.</span><span class="sxs-lookup"><span data-stu-id="eb765-153">The sample application terminates automatically after sending 40 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="eb765-154">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="eb765-154">Summary</span></span>

<span data-ttu-id="eb765-155">Du har har konfigurera anslutningar mellan SensorTag och Intel NUC och kör ett tabell-exempelprogram som samlar in och skickar data från SensorTag till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="eb765-155">You've successfully set up the connectivity between SensorTag and Intel NUC, and run a BLE sample application which collects and sends data from SensorTag to your IoT hub.</span></span> <span data-ttu-id="eb765-156">Du är redo att lära dig hur du kontrollerar att din IoT-hubb har tagit emot data.</span><span class="sxs-lookup"><span data-stu-id="eb765-156">You're ready to learn how to verify that your IoT hub has received the data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eb765-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eb765-157">Next steps</span></span>
[<span data-ttu-id="eb765-158">Läs meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="eb765-158">Read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)