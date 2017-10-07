---
title: "Connect Arduino (C) tooAzure IoT - lektionen 3: köra exemplet | Microsoft Docs"
description: "Distribuera och köra en exempel programmet tooAdafruit ludd M0 WiFi som skickar meddelanden tooyour IoT-hubb och blinkar hello-Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-Molntjänsten, arduino skicka data toocloud"
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
ms.openlocfilehash: ddca015a3655f8a1a9de2a00e718ec0d28a5affb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a><span data-ttu-id="33747-104">Kör ett exempel programmet toosend meddelanden från enhet till moln</span><span class="sxs-lookup"><span data-stu-id="33747-104">Run a sample application toosend device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="33747-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="33747-105">What you will do</span></span>
<span data-ttu-id="33747-106">Den här artikeln visar hur toodeploy och kör ett exempelprogram på din Adafruit ludd M0 WiFi Arduino board som skickar meddelanden tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="33747-106">This article will show you how toodeploy and run a sample application on your Adafruit Feather M0 WiFi Arduino board that sends messages tooyour IoT hub.</span></span>

<span data-ttu-id="33747-107">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="33747-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="33747-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="33747-108">What you will learn</span></span>
<span data-ttu-id="33747-109">Du kommer lära dig hur toouse hello gulp verktyget toodeploy och köra hello exempelprogrammet Arduino på Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="33747-109">You will learn how toouse hello gulp tool toodeploy and run hello sample Arduino application on your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="33747-110">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="33747-110">What you need</span></span>
* <span data-ttu-id="33747-111">Innan du börjar den här uppgiften måste har slutförts [skapa en funktionsapp i Azure-och en storage-konto tooprocess och lagra IoT-hubb meddelanden][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="33747-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account tooprocess and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="33747-112">Hämta din IoT-hubb och enheten anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="33747-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="33747-113">Hej enheten anslutningssträngen är används tooconnect din Arduino board tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="33747-113">hello device connection string is used tooconnect your Arduino board tooyour IoT hub.</span></span> <span data-ttu-id="33747-114">anslutningssträngen för hello IoT hub är används tooconnect din IoT-hubb toohello enhetsidentitet som representerar din Arduino board i hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="33747-114">hello IoT hub connection string is used tooconnect your IoT hub toohello device identity that represents your Arduino board in hello IoT hub.</span></span>

* <span data-ttu-id="33747-115">Lista alla IoT hubs i resursgruppen genom att köra hello följande Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="33747-115">List all your IoT hubs in your resource group by running hello following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="33747-116">Använd `iot-sample` som hello värde för `{resource group name}` om du inte ändrar hello-värdet.</span><span class="sxs-lookup"><span data-stu-id="33747-116">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>

* <span data-ttu-id="33747-117">Hämta hello IoT hub-anslutningssträng genom att köra hello följande Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="33747-117">Get hello IoT hub connection string by running hello following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name}
```

<span data-ttu-id="33747-118">`{my hub name}`är hello-namn som du angav när du skapade din IoT-hubb och registrerat Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="33747-118">`{my hub name}` is hello name that you specified when you created your IoT hub and registered your Arduino board.</span></span>

* <span data-ttu-id="33747-119">Hämta anslutningssträngen för hello enheten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="33747-119">Get hello device connection string by running hello following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id mym0wifi
```

<span data-ttu-id="33747-120">Använd `mym0wifi` som hello värde för `{device id}` om du inte ändrar hello-värdet.</span><span class="sxs-lookup"><span data-stu-id="33747-120">Use `mym0wifi` as hello value of `{device id}` if you didn't change hello value.</span></span>
## <a name="configure-hello-device-connection"></a><span data-ttu-id="33747-121">Konfigurera hello enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="33747-121">Configure hello device connection</span></span>
<span data-ttu-id="33747-122">tooconfigure Hej enhetsanslutning, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="33747-122">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="33747-123">Hämta hello serieport av hello-enhet med hello enheten identifiering cli:</span><span class="sxs-lookup"><span data-stu-id="33747-123">Obtain hello serial port of hello device with hello device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="33747-124">Du bör se utdata som är liknande toohello följande och hitta hello usb COM-port för Arduino-skiva:</span><span class="sxs-lookup"><span data-stu-id="33747-124">You should see an output that is similar toohello following and find hello usb COM port for your Arduino board:</span></span>

   ![Identifiering av nätverksenheter][device-discovery]

2. <span data-ttu-id="33747-126">Öppna hello filen `config.json` i hello lektionen mappen och Lägg till hello värdet hello hitta COM-portnummer:</span><span class="sxs-lookup"><span data-stu-id="33747-126">Open hello file `config.json` in hello lesson folder and add hello value of hello found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > <span data-ttu-id="33747-128">För hello COM-port på Windows-plattformen, den har hello format för `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="33747-128">For hello COM port, on Windows platform, it has hello format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="33747-129">I macOS eller Ubuntu det börjar med `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="33747-129">On macOS or Ubuntu, it starts with `/dev/`.</span></span>

3. <span data-ttu-id="33747-130">Initiera hello konfigurationsfilen genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="33747-130">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   npm install
   gulp init
   gulp install-tools
   ```
4. <span data-ttu-id="33747-131">Öppna hello enheten konfigurationsfilen `config-arduino.json` i Visual Studio-koden genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="33747-131">Open hello device configuration file `config-arduino.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```

   ![config-arduino.json][config-arduino-json]

5. <span data-ttu-id="33747-133">Se följande ersättningar i hello hello `config-arduino.json` fil:</span><span class="sxs-lookup"><span data-stu-id="33747-133">Make hello following replacements in hello `config-arduino.json` file:</span></span>

   * <span data-ttu-id="33747-134">Ersätt **[Wi-Fi SSID]** med din Wi-Fi-SSID som anslutna toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="33747-134">Replace **[Wi-Fi SSID]** with your Wi-Fi SSID that connected toohello Internet.</span></span>
   * <span data-ttu-id="33747-135">Ersätt **[Wi-Fi-lösenord]** med Wi-Fi-lösenord.</span><span class="sxs-lookup"><span data-stu-id="33747-135">Replace **[Wi-Fi password]** with your Wi-Fi password.</span></span> <span data-ttu-id="33747-136">Ta bort hello sträng om din Wi-Fi inte kräver lösenord.</span><span class="sxs-lookup"><span data-stu-id="33747-136">Remove hello string if your Wi-Fi doesn't require password.</span></span>
   * <span data-ttu-id="33747-137">Ersätt **[anslutningssträngen för IoT-enhet]** med hello `device connection string` du fick.</span><span class="sxs-lookup"><span data-stu-id="33747-137">Replace **[IoT device connection string]** with hello `device connection string` you obtained.</span></span>
   * <span data-ttu-id="33747-138">Ersätt **[anslutningssträngen för IoT-hubb]** med hello `iot hub connection string` du fick.</span><span class="sxs-lookup"><span data-stu-id="33747-138">Replace **[IoT hub connection string]** with hello `iot hub connection string` you obtained.</span></span>

   > [!NOTE]
   > <span data-ttu-id="33747-139">Du behöver inte `azure_storage_connection_string` i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="33747-139">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="33747-140">Se till att den är.</span><span class="sxs-lookup"><span data-stu-id="33747-140">Keep it as is.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="33747-141">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="33747-141">Deploy and run hello sample application</span></span>
<span data-ttu-id="33747-142">Distribuera och köra hello exempelprogrammet på Arduino-kort genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="33747-142">Deploy and run hello sample application on your Arduino board by running hello following command:</span></span>

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

> [!NOTE]
> <span data-ttu-id="33747-143">hello standard gulp aktiviteten körs `install-tools` och `run` aktiviteter sekventiellt.</span><span class="sxs-lookup"><span data-stu-id="33747-143">hello default gulp task runs `install-tools` and `run` tasks sequentially.</span></span> <span data-ttu-id="33747-144">När du [distribuerat hello blinka app][deployed-the-blink-app], du körde dessa uppgifter separat.</span><span class="sxs-lookup"><span data-stu-id="33747-144">When you [deployed hello blink app][deployed-the-blink-app], you ran these tasks separately.</span></span>

## <a name="verify-that-hello-sample-application-works"></a><span data-ttu-id="33747-145">Kontrollera att det fungerar hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="33747-145">Verify that hello sample application works</span></span>
<span data-ttu-id="33747-146">Du bör se hello GPIO #0 inbyggd Indikator blinkande varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="33747-146">You should see hello GPIO #0 on-board LED blinking every two seconds.</span></span> <span data-ttu-id="33747-147">Varje gång hello Indikator blinkar hello exempelprogrammet skickar ett meddelande tooyour IoT-hubb och verifierar att hello-meddelande har skickats tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="33747-147">Every time hello LED blinks, hello sample application sends a message tooyour IoT hub and verifies that hello message has been successfully sent tooyour IoT hub.</span></span> <span data-ttu-id="33747-148">Dessutom kan ut varje meddelande tas emot av hello IoT-hubb i hello konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="33747-148">In addition, each message received by hello IoT hub is printed in hello console window.</span></span> <span data-ttu-id="33747-149">hello exempelprogrammet avbryter automatiskt efter 20 meddelanden skickas.</span><span class="sxs-lookup"><span data-stu-id="33747-149">hello sample application terminates automatically after sending 20 messages.</span></span>

![Exempelprogrammet med skickade och mottagna meddelanden][sample-application-with-sent-and-received-messages]

## <a name="summary"></a><span data-ttu-id="33747-151">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="33747-151">Summary</span></span>
<span data-ttu-id="33747-152">Du har distribuerat och köra hello nya blinka exempelprogrammet på din Arduino board toosend meddelanden från enhet till moln tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="33747-152">You've deployed and run hello new blink sample application on your Arduino board toosend device-to-cloud messages tooyour IoT hub.</span></span> <span data-ttu-id="33747-153">Du kan nu övervaka dina meddelanden som de är skrivna toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="33747-153">You now monitor your messages as they are written toohello storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="33747-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="33747-154">Next steps</span></span>
<span data-ttu-id="33747-155">[Läs meddelandena kvar i Azure Storage][read-messages-persisted-in-azure-storage]</span><span class="sxs-lookup"><span data-stu-id="33747-155">[Read messages persisted in Azure Storage][read-messages-persisted-in-azure-storage]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/config-arduino.png
[deployed-the-blink-app]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_run_arduino.png
[read-messages-persisted-in-azure-storage]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-read-table-storage.md