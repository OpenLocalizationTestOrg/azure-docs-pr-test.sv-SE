---
title: 'Ansluta Arduino (C) till Azure IoT - lektionen 4: moln till enhet | Microsoft Docs'
description: "Ett exempelprogram som körs på Adafruit ludd M0 WiFi och Övervakare inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden till Adafruit ludd M0 WiFi från din IoT-hubb blinkar på Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "arduino kontroll ledde från webben, arduino kontroll ledde via webben"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: a0bf53fb-29fb-485f-ba4a-6c715057b1a2
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b4f4c1d86b10b64c104ac812d1f650d723322be8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a><span data-ttu-id="3296a-105">Kör ett exempelprogram som tar emot meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="3296a-105">Run a sample application to receive cloud-to-device messages</span></span>
<span data-ttu-id="3296a-106">I den här artikeln får distribuera du ett exempelprogram på Adafruit ludd M0 WiFi Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="3296a-106">In this article, you deploy a sample application on your Adafruit Feather M0 WiFi Arduino board.</span></span>

<span data-ttu-id="3296a-107">Exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3296a-107">The sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="3296a-108">Du kan också köra en aktivitet med gulp på datorn för att skicka meddelanden till Arduino-kort från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3296a-108">You also run a gulp task on your computer to send messages to your Arduino board from your IoT hub.</span></span> <span data-ttu-id="3296a-109">När exempelprogrammet som tar emot meddelanden, blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="3296a-109">When the sample application receives the messages, it blinks the LED.</span></span> <span data-ttu-id="3296a-110">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="3296a-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="3296a-111">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="3296a-111">What you will do</span></span>
* <span data-ttu-id="3296a-112">Ansluta till din IoT-hubb exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="3296a-112">Connect the sample application to your IoT hub.</span></span>
* <span data-ttu-id="3296a-113">Distribuera och köra exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="3296a-113">Deploy and run the sample application.</span></span>
* <span data-ttu-id="3296a-114">Skicka meddelanden från din IoT-hubb din Arduino planen blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="3296a-114">Send messages from your IoT hub to your Arduino board to blink the LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="3296a-115">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="3296a-115">What you will learn</span></span>
<span data-ttu-id="3296a-116">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="3296a-116">In this article, you will learn:</span></span>
* <span data-ttu-id="3296a-117">Så här övervakar du inkommande meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3296a-117">How to monitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="3296a-118">Hur du skickar meddelanden moln till enhet från din IoT-hubb Arduino-planen.</span><span class="sxs-lookup"><span data-stu-id="3296a-118">How to send cloud-to-device messages from your IoT hub to your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="3296a-119">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="3296a-119">What you need</span></span>
* <span data-ttu-id="3296a-120">Kort din Arduino som ställts in för användning.</span><span class="sxs-lookup"><span data-stu-id="3296a-120">Your Arduino board, set up for use.</span></span> <span data-ttu-id="3296a-121">Information om hur du ställer in Arduino-kort finns [konfigurera din enhet][configure-your-device].</span><span class="sxs-lookup"><span data-stu-id="3296a-121">To learn how to set up your Arduino board, see [Configure your device][configure-your-device].</span></span>
* <span data-ttu-id="3296a-122">En IoT-hubb som skapas i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3296a-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="3296a-123">Information om hur du skapar din IoT-hubb finns [skapa Azure IoT Hub][create-your-azure-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="3296a-123">To learn how to create your IoT hub, see [Create your Azure IoT Hub][create-your-azure-iot-hub].</span></span>

## <a name="connect-the-sample-application-to-your-iot-hub"></a><span data-ttu-id="3296a-124">Ansluta exempelprogrammet till din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="3296a-124">Connect the sample application to your IoT hub</span></span>

1. <span data-ttu-id="3296a-125">Kontrollera att du är i mappen lagringsplatsen `iot-hub-c-feather-m0-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="3296a-125">Make sure that you're in the repo folder `iot-hub-c-feather-m0-getting-started`.</span></span>

   <span data-ttu-id="3296a-126">Öppna exempelprogrammet i Visual Studio Code genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3296a-126">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="3296a-127">Observera den `app.ino` filen i den `app` undermappen.</span><span class="sxs-lookup"><span data-stu-id="3296a-127">Notice the `app.ino` file in the `app` subfolder.</span></span> <span data-ttu-id="3296a-128">Den `app.ino` filen är viktiga källfilen som innehåller koden för övervakning av inkommande meddelanden från IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="3296a-128">The `app.ino` file is the key source file that contains the code to monitor incoming messages from the IoT hub.</span></span> <span data-ttu-id="3296a-129">Den `blinkLED` funktionen blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="3296a-129">The `blinkLED` function blinks the LED.</span></span>

   ![Lagringsplatsen strukturen i exempelprogrammet][repo-structure]

2. <span data-ttu-id="3296a-131">Hämta den seriella porten på enheten med enheten identifiering cli:</span><span class="sxs-lookup"><span data-stu-id="3296a-131">Obtain the serial port of the device with the device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="3296a-132">Du bör se utdata som liknar följande och hitta usb COM-port för Arduino-skiva:</span><span class="sxs-lookup"><span data-stu-id="3296a-132">You should see an output that is similar to the following and find the usb COM port for your Arduino board:</span></span>

   ![Identifiering av nätverksenheter][device-discovery]

3. <span data-ttu-id="3296a-134">Öppna filen `config.json` i mappen lektionen och inmatade värdet för hittade COM-portnummer:</span><span class="sxs-lookup"><span data-stu-id="3296a-134">Open the file `config.json` in the lesson folder and input the value of the found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > <span data-ttu-id="3296a-136">COM-porten på Windows-plattformen, den har formatet för `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="3296a-136">For the COM port, on Windows platform, it has the format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="3296a-137">I macOS eller Ubuntu startas med `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="3296a-137">On macOS or Ubuntu, it will start with `/dev/`.</span></span>

4. <span data-ttu-id="3296a-138">Initiera konfigurationsfilen genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3296a-138">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   # For Windows command prompt
   npm install
   gulp init
   gulp install-tools
   ```

5. <span data-ttu-id="3296a-139">Se följande ersättningar i den `config-arduino.json` filen:</span><span class="sxs-lookup"><span data-stu-id="3296a-139">Make the following replacements in the `config-arduino.json` file:</span></span>

   <span data-ttu-id="3296a-140">Om du har slutfört stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på den här datorn alla konfigurationer ärvs, så du kan hoppa över steg för att distribuera och köra exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="3296a-140">If you completed the steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on this computer, all the configurations are inherited, so you can skip the step to the task of deploying and running the sample application.</span></span> <span data-ttu-id="3296a-141">Om du har slutfört stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på en annan dator som du behöver ersätta platshållare i den `config-arduino.json` filen.</span><span class="sxs-lookup"><span data-stu-id="3296a-141">If you completed the steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on a different computer, you need to replace the placeholders in the `config-arduino.json` file.</span></span> <span data-ttu-id="3296a-142">Den `config-arduino.json` filen finns i undermappen arbetsmappen.</span><span class="sxs-lookup"><span data-stu-id="3296a-142">The `config-arduino.json` file is in the subfolder of your home folder.</span></span>

   ![Innehållet i filen config arduino.json][config-arduino-json]

   * <span data-ttu-id="3296a-144">Ersätt **[Wi-Fi SSID]** med din Wi-Fi-SSID ansluten till Internet.</span><span class="sxs-lookup"><span data-stu-id="3296a-144">Replace **[Wi-Fi SSID]** with your Wi-Fi SSID that connected to the Internet.</span></span>
   * <span data-ttu-id="3296a-145">Ersätt **[Wi-Fi-lösenord]** med Wi-Fi-lösenord.</span><span class="sxs-lookup"><span data-stu-id="3296a-145">Replace **[Wi-Fi password]** with your Wi-Fi password.</span></span> <span data-ttu-id="3296a-146">Ta bort strängen om din Wi-Fi inte kräver lösenord.</span><span class="sxs-lookup"><span data-stu-id="3296a-146">Remove the string if your Wi-Fi doesn't require password.</span></span>
   * <span data-ttu-id="3296a-147">Ersätt **[anslutningssträngen för IoT-enhet]** med den anslutningssträng för enheten som du får genom att köra den `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` kommando.</span><span class="sxs-lookup"><span data-stu-id="3296a-147">Replace **[IoT device connection string]** with the device connection string that you get by running the `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` command.</span></span>
   * <span data-ttu-id="3296a-148">Ersätt **[anslutningssträngen för IoT-hubb]** med IoT-hubb anslutningssträngen som du får genom att köra den `az iot hub show-connection-string --name {my hub name}` kommando.</span><span class="sxs-lookup"><span data-stu-id="3296a-148">Replace **[IoT hub connection string]** with the IoT hub connection string that you get by running the `az iot hub show-connection-string --name {my hub name}` command.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="3296a-149">Distribuera och köra exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="3296a-149">Deploy and run the sample application</span></span>
<span data-ttu-id="3296a-150">Distribuera och köra exempelprogrammet på Arduino-kort genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3296a-150">Deploy and run the sample application on your Arduino board by running the following commands:</span></span>

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

<span data-ttu-id="3296a-151">Kommandot gulp distribuerar exempelprogrammet Arduino-planen.</span><span class="sxs-lookup"><span data-stu-id="3296a-151">The gulp command deploys the sample application to your Arduino board.</span></span> <span data-ttu-id="3296a-152">Därefter körs programmet på Arduino-kort och en separat åtgärd på värddatorn för att skicka 20 blinka meddelanden till Arduino-kort från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3296a-152">Then, it runs the application on your Arduino board and a separate task on your host computer to send 20 blink messages to your Arduino board from your IoT hub.</span></span>

<span data-ttu-id="3296a-153">När exempelprogrammet som körs, startas lyssnar på meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3296a-153">After the sample application runs, it starts listening to messages from your IoT hub.</span></span> <span data-ttu-id="3296a-154">Gulp-aktivitet skickar under tiden kan flera ”blinkar” meddelanden från din IoT-hubb Arduino-planen.</span><span class="sxs-lookup"><span data-stu-id="3296a-154">Meanwhile, the gulp task sends several "blink" messages from your IoT hub to your Arduino board.</span></span> <span data-ttu-id="3296a-155">För varje meddelande blinka som tar emot planen exempelprogrammet anropar den `blinkLED` funktionen blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="3296a-155">For each blink message that the board receives, the sample application calls the `blinkLED` function to blink the LED.</span></span>

<span data-ttu-id="3296a-156">Du bör se Indikator blinka varannan sekund som aktiviteten gulp 20 meddelanden skickas från din IoT-hubb till Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="3296a-156">You should see the LED blink every two seconds as the gulp task sends 20 messages from your IoT hub to your Arduino board.</span></span> <span data-ttu-id="3296a-157">Den sista som är en ”stoppa” meddelande som förhindrar att program körs.</span><span class="sxs-lookup"><span data-stu-id="3296a-157">The last one is a "stop" message that stops the application from running.</span></span>

![Exempelprogrammet med gulp kommandot och blinkar meddelanden][sample-application]

## <a name="summary"></a><span data-ttu-id="3296a-159">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="3296a-159">Summary</span></span>
<span data-ttu-id="3296a-160">Du har skickat meddelanden från din IoT-hubb till din Arduino board blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="3296a-160">You’ve successfully sent messages from your IoT hub to your Arduino board to blink the LED.</span></span> <span data-ttu-id="3296a-161">Nästa uppgift är valfritt: ändra på och av beteendet för Indikatorn.</span><span class="sxs-lookup"><span data-stu-id="3296a-161">The next task is optional: change the on and off behavior of the LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3296a-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3296a-162">Next steps</span></span>
<span data-ttu-id="3296a-163">[Ändra på och av beteendet för Indikatorn][change-the-on-and-off-led-behavior]</span><span class="sxs-lookup"><span data-stu-id="3296a-163">[Change the on and off behavior of the LED][change-the-on-and-off-led-behavior]</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/repo_structure_arduino.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[create-an-azure-function-app-and-storage-account]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/config-arduino.png
[sample-application]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_blink_arduino.png
[change-the-on-and-off-led-behavior]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-change-led-behavior.md