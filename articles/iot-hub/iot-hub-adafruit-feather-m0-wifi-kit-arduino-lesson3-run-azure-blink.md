---
title: "Connect Arduino (C) till Azure IoT - lektionen 3: köra exemplet | Microsoft Docs"
description: "Distribuera och köra ett exempelprogram till Adafruit ludd M0 WiFi som skickar meddelanden till din IoT-hubb och blinkar på Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-Molntjänsten, arduino skicka data till molnet"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 92cce319-2b17-4c9b-889d-deac959e3e7c
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0c17fe74dbd78abca955f7789a1674ed6333367f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a><span data-ttu-id="25883-104">Kör ett exempelprogram för att skicka meddelanden från enhet till moln</span><span class="sxs-lookup"><span data-stu-id="25883-104">Run a sample application to send device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="25883-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="25883-105">What you will do</span></span>
<span data-ttu-id="25883-106">Den här artikeln visar hur du distribuera och köra ett exempelprogram på Adafruit ludd M0 WiFi Arduino-kort som skickar meddelanden till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="25883-106">This article will show you how to deploy and run a sample application on your Adafruit Feather M0 WiFi Arduino board that sends messages to your IoT hub.</span></span>

<span data-ttu-id="25883-107">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="25883-107">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="25883-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="25883-108">What you will learn</span></span>
<span data-ttu-id="25883-109">Du lära dig hur du använder verktyget gulp att distribuera och köra Arduino exempelprogrammet på Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="25883-109">You will learn how to use the gulp tool to deploy and run the sample Arduino application on your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="25883-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="25883-110">What you need</span></span>
* <span data-ttu-id="25883-111">Innan du börjar den här uppgiften måste har slutförts [skapa en funktionsapp i Azure-och ett lagringskonto för att bearbeta och lagra IoT-hubb meddelanden][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="25883-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account to process and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="25883-112">Hämta din IoT-hubb och enheten anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="25883-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="25883-113">Anslutningssträngen enheten används för att ansluta Arduino-kort till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="25883-113">The device connection string is used to connect your Arduino board to your IoT hub.</span></span> <span data-ttu-id="25883-114">IoT-hubb anslutningssträngen används för att ansluta din IoT-hubb för enhetens identitet som representerar Arduino-kort i IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="25883-114">The IoT hub connection string is used to connect your IoT hub to the device identity that represents your Arduino board in the IoT hub.</span></span>

* <span data-ttu-id="25883-115">Lista alla IoT hubs i resursgruppen genom att köra följande kommando i Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="25883-115">List all your IoT hubs in your resource group by running the following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="25883-116">Använd `iot-sample` som värde för `{resource group name}` om du inte ändra värdet.</span><span class="sxs-lookup"><span data-stu-id="25883-116">Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.</span></span>

* <span data-ttu-id="25883-117">Hämta anslutningssträngen för IoT-hubb genom att köra följande kommando i Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="25883-117">Get the IoT hub connection string by running the following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name}
```

<span data-ttu-id="25883-118">`{my hub name}`är det namn som du angav när du skapade din IoT-hubb och registrerat Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="25883-118">`{my hub name}` is the name that you specified when you created your IoT hub and registered your Arduino board.</span></span>

* <span data-ttu-id="25883-119">Hämta anslutningssträngen för enheten genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="25883-119">Get the device connection string by running the following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id mym0wifi
```

<span data-ttu-id="25883-120">Använd `mym0wifi` som värde för `{device id}` om du inte ändra värdet.</span><span class="sxs-lookup"><span data-stu-id="25883-120">Use `mym0wifi` as the value of `{device id}` if you didn't change the value.</span></span>
## <a name="configure-the-device-connection"></a><span data-ttu-id="25883-121">Konfigurera enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="25883-121">Configure the device connection</span></span>
<span data-ttu-id="25883-122">Följ dessa steg för att konfigurera enhetsanslutningen:</span><span class="sxs-lookup"><span data-stu-id="25883-122">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="25883-123">Hämta den seriella porten på enheten med enheten identifiering cli:</span><span class="sxs-lookup"><span data-stu-id="25883-123">Obtain the serial port of the device with the device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="25883-124">Du bör se utdata som liknar följande och hitta usb COM-port för Arduino-skiva:</span><span class="sxs-lookup"><span data-stu-id="25883-124">You should see an output that is similar to the following and find the usb COM port for your Arduino board:</span></span>

   ![Identifiering av nätverksenheter][device-discovery]

2. <span data-ttu-id="25883-126">Öppna filen `config.json` i mappen lektionen och lägga till värdet för hittade COM-portnummer:</span><span class="sxs-lookup"><span data-stu-id="25883-126">Open the file `config.json` in the lesson folder and add the value of the found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > <span data-ttu-id="25883-128">COM-porten på Windows-plattformen, den har formatet för `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="25883-128">For the COM port, on Windows platform, it has the format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="25883-129">I macOS eller Ubuntu det börjar med `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="25883-129">On macOS or Ubuntu, it starts with `/dev/`.</span></span>

3. <span data-ttu-id="25883-130">Initiera konfigurationsfilen genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="25883-130">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   npm install
   gulp init
   gulp install-tools
   ```
4. <span data-ttu-id="25883-131">Öppna konfigurationsfilen enheten `config-arduino.json` i Visual Studio-koden genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="25883-131">Open the device configuration file `config-arduino.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```

   ![config-arduino.json][config-arduino-json]

5. <span data-ttu-id="25883-133">Se följande ersättningar i den `config-arduino.json` filen:</span><span class="sxs-lookup"><span data-stu-id="25883-133">Make the following replacements in the `config-arduino.json` file:</span></span>

   * <span data-ttu-id="25883-134">Ersätt **[Wi-Fi SSID]** med din Wi-Fi-SSID ansluten till Internet.</span><span class="sxs-lookup"><span data-stu-id="25883-134">Replace **[Wi-Fi SSID]** with your Wi-Fi SSID that connected to the Internet.</span></span>
   * <span data-ttu-id="25883-135">Ersätt **[Wi-Fi-lösenord]** med Wi-Fi-lösenord.</span><span class="sxs-lookup"><span data-stu-id="25883-135">Replace **[Wi-Fi password]** with your Wi-Fi password.</span></span> <span data-ttu-id="25883-136">Ta bort strängen om din Wi-Fi inte kräver lösenord.</span><span class="sxs-lookup"><span data-stu-id="25883-136">Remove the string if your Wi-Fi doesn't require password.</span></span>
   * <span data-ttu-id="25883-137">Ersätt **[anslutningssträngen för IoT-enhet]** med den `device connection string` du fick.</span><span class="sxs-lookup"><span data-stu-id="25883-137">Replace **[IoT device connection string]** with the `device connection string` you obtained.</span></span>
   * <span data-ttu-id="25883-138">Ersätt **[anslutningssträngen för IoT-hubb]** med den `iot hub connection string` du fick.</span><span class="sxs-lookup"><span data-stu-id="25883-138">Replace **[IoT hub connection string]** with the `iot hub connection string` you obtained.</span></span>

   > [!NOTE]
   > <span data-ttu-id="25883-139">Du behöver inte `azure_storage_connection_string` i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="25883-139">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="25883-140">Se till att den är.</span><span class="sxs-lookup"><span data-stu-id="25883-140">Keep it as is.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="25883-141">Distribuera och köra exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="25883-141">Deploy and run the sample application</span></span>
<span data-ttu-id="25883-142">Distribuera och köra exempelprogrammet på Arduino-kort genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="25883-142">Deploy and run the sample application on your Arduino board by running the following command:</span></span>

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

> [!NOTE]
> <span data-ttu-id="25883-143">Standard gulp aktiviteten körs `install-tools` och `run` aktiviteter sekventiellt.</span><span class="sxs-lookup"><span data-stu-id="25883-143">The default gulp task runs `install-tools` and `run` tasks sequentially.</span></span> <span data-ttu-id="25883-144">När du [har distribuerat appen blinka][deployed-the-blink-app], du körde dessa uppgifter separat.</span><span class="sxs-lookup"><span data-stu-id="25883-144">When you [deployed the blink app][deployed-the-blink-app], you ran these tasks separately.</span></span>

## <a name="verify-that-the-sample-application-works"></a><span data-ttu-id="25883-145">Kontrollera att det fungerar exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="25883-145">Verify that the sample application works</span></span>
<span data-ttu-id="25883-146">Du bör se den GPIO #0 inbyggd Indikator blinkande varannan sekunder.</span><span class="sxs-lookup"><span data-stu-id="25883-146">You should see the GPIO #0 on-board LED blinking every two seconds.</span></span> <span data-ttu-id="25883-147">Varje gång Indikatorn blinkar exempelprogrammet skickar ett meddelande till din IoT-hubb och verifierar att meddelandet har skickats till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="25883-147">Every time the LED blinks, the sample application sends a message to your IoT hub and verifies that the message has been successfully sent to your IoT hub.</span></span> <span data-ttu-id="25883-148">Dessutom kan ut varje meddelande tas emot av IoT-hubben i konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="25883-148">In addition, each message received by the IoT hub is printed in the console window.</span></span> <span data-ttu-id="25883-149">Exempelprogrammet avbryter automatiskt efter 20 meddelanden skickas.</span><span class="sxs-lookup"><span data-stu-id="25883-149">The sample application terminates automatically after sending 20 messages.</span></span>

![Exempelprogrammet med skickade och mottagna meddelanden][sample-application-with-sent-and-received-messages]

## <a name="summary"></a><span data-ttu-id="25883-151">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="25883-151">Summary</span></span>
<span data-ttu-id="25883-152">Du har distribuerat och köra den nya blinka exempelprogrammet på Arduino-kort för att skicka meddelanden från enhet till moln till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="25883-152">You've deployed and run the new blink sample application on your Arduino board to send device-to-cloud messages to your IoT hub.</span></span> <span data-ttu-id="25883-153">Du kan nu övervaka dina meddelanden när de skrivs till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="25883-153">You now monitor your messages as they are written to the storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25883-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="25883-154">Next steps</span></span>
<span data-ttu-id="25883-155">[Läs meddelandena kvar i Azure Storage][read-messages-persisted-in-azure-storage]</span><span class="sxs-lookup"><span data-stu-id="25883-155">[Read messages persisted in Azure Storage][read-messages-persisted-in-azure-storage]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/config-arduino.png
[deployed-the-blink-app]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_run_arduino.png
[read-messages-persisted-in-azure-storage]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-read-table-storage.md