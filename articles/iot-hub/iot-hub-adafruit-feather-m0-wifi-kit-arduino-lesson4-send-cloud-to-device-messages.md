---
title: 'Connect Arduino (C) tooAzure IoT - lektionen 4: moln till enhet | Microsoft Docs'
description: "Ett exempelprogram som körs på Adafruit ludd M0 WiFi och Övervakare inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden tooAdafruit ludd M0 WiFi från din IoT-hubb tooblink hello Indikator."
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
ms.openlocfilehash: dcddd61ff684f49436103675938d719cb227c409
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a><span data-ttu-id="9d425-105">Kör ett exempel programmet tooreceive meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="9d425-105">Run a sample application tooreceive cloud-to-device messages</span></span>
<span data-ttu-id="9d425-106">I den här artikeln får distribuera du ett exempelprogram på Adafruit ludd M0 WiFi Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="9d425-106">In this article, you deploy a sample application on your Adafruit Feather M0 WiFi Arduino board.</span></span>

<span data-ttu-id="9d425-107">hello exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9d425-107">hello sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="9d425-108">Du också köra en aktivitet med gulp på din dator toosend meddelanden tooyour Arduino board från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9d425-108">You also run a gulp task on your computer toosend messages tooyour Arduino board from your IoT hub.</span></span> <span data-ttu-id="9d425-109">När hello exempelprogrammet får hälsningsmeddelande, blinkar hello-Indikator.</span><span class="sxs-lookup"><span data-stu-id="9d425-109">When hello sample application receives hello messages, it blinks hello LED.</span></span> <span data-ttu-id="9d425-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="9d425-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="9d425-111">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="9d425-111">What you will do</span></span>
* <span data-ttu-id="9d425-112">Ansluta hello exempel programmet tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9d425-112">Connect hello sample application tooyour IoT hub.</span></span>
* <span data-ttu-id="9d425-113">Distribuera och köra hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="9d425-113">Deploy and run hello sample application.</span></span>
* <span data-ttu-id="9d425-114">Skicka meddelanden från din IoT-hubb tooyour Arduino board tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="9d425-114">Send messages from your IoT hub tooyour Arduino board tooblink hello LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="9d425-115">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="9d425-115">What you will learn</span></span>
<span data-ttu-id="9d425-116">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="9d425-116">In this article, you will learn:</span></span>
* <span data-ttu-id="9d425-117">Hur toomonitor inkommande meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9d425-117">How toomonitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="9d425-118">Hur toosend moln till enhet meddelanden från din IoT-hubb tooyour Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="9d425-118">How toosend cloud-to-device messages from your IoT hub tooyour Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="9d425-119">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="9d425-119">What you need</span></span>
* <span data-ttu-id="9d425-120">Kort din Arduino som ställts in för användning.</span><span class="sxs-lookup"><span data-stu-id="9d425-120">Your Arduino board, set up for use.</span></span> <span data-ttu-id="9d425-121">hur tooset in Arduino-kort Se toolearn [konfigurera din enhet][configure-your-device].</span><span class="sxs-lookup"><span data-stu-id="9d425-121">toolearn how tooset up your Arduino board, see [Configure your device][configure-your-device].</span></span>
* <span data-ttu-id="9d425-122">En IoT-hubb som skapas i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9d425-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="9d425-123">toolearn hur toocreate din IoT-hubb finns [skapa Azure IoT Hub][create-your-azure-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="9d425-123">toolearn how toocreate your IoT hub, see [Create your Azure IoT Hub][create-your-azure-iot-hub].</span></span>

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a><span data-ttu-id="9d425-124">Ansluta hello exempel programmet tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="9d425-124">Connect hello sample application tooyour IoT hub</span></span>

1. <span data-ttu-id="9d425-125">Kontrollera att du arbetar i hello lagringsplatsen mappen `iot-hub-c-feather-m0-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="9d425-125">Make sure that you're in hello repo folder `iot-hub-c-feather-m0-getting-started`.</span></span>

   <span data-ttu-id="9d425-126">Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="9d425-126">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="9d425-127">Meddelande hello `app.ino` filen i hello `app` undermappen.</span><span class="sxs-lookup"><span data-stu-id="9d425-127">Notice hello `app.ino` file in hello `app` subfolder.</span></span> <span data-ttu-id="9d425-128">Hej `app.ino` filen är hello källa som innehåller hello toomonitor inkommande meddelanden från hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9d425-128">hello `app.ino` file is hello key source file that contains hello code toomonitor incoming messages from hello IoT hub.</span></span> <span data-ttu-id="9d425-129">Hej `blinkLED` funktionen blinkar hello-Indikator.</span><span class="sxs-lookup"><span data-stu-id="9d425-129">hello `blinkLED` function blinks hello LED.</span></span>

   ![Lagringsplatsen strukturen i hello exempelprogram][repo-structure]

2. <span data-ttu-id="9d425-131">Hämta hello serieport av hello-enhet med hello enheten identifiering cli:</span><span class="sxs-lookup"><span data-stu-id="9d425-131">Obtain hello serial port of hello device with hello device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="9d425-132">Du bör se utdata som är liknande toohello följande och hitta hello usb COM-port för Arduino-skiva:</span><span class="sxs-lookup"><span data-stu-id="9d425-132">You should see an output that is similar toohello following and find hello usb COM port for your Arduino board:</span></span>

   ![Identifiering av nätverksenheter][device-discovery]

3. <span data-ttu-id="9d425-134">Öppna hello filen `config.json` i hello lektionen mapp och inkommande hello värdet för hello hitta COM-portnummer:</span><span class="sxs-lookup"><span data-stu-id="9d425-134">Open hello file `config.json` in hello lesson folder and input hello value of hello found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > <span data-ttu-id="9d425-136">För hello COM-port på Windows-plattformen, den har hello format för `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="9d425-136">For hello COM port, on Windows platform, it has hello format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="9d425-137">I macOS eller Ubuntu startas med `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="9d425-137">On macOS or Ubuntu, it will start with `/dev/`.</span></span>

4. <span data-ttu-id="9d425-138">Initiera hello konfigurationsfilen genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="9d425-138">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   # For Windows command prompt
   npm install
   gulp init
   gulp install-tools
   ```

5. <span data-ttu-id="9d425-139">Se följande ersättningar i hello hello `config-arduino.json` fil:</span><span class="sxs-lookup"><span data-stu-id="9d425-139">Make hello following replacements in hello `config-arduino.json` file:</span></span>

   <span data-ttu-id="9d425-140">Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på den här datorn alla konfigurationer av hello ärvs, så du kan hoppa över hello steg toohello aktiviteten för att distribuera och Kör hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="9d425-140">If you completed hello steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on this computer, all hello configurations are inherited, so you can skip hello step toohello task of deploying and running hello sample application.</span></span> <span data-ttu-id="9d425-141">Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på en annan dator måste tooreplace hello platshållare i hello `config-arduino.json` fil.</span><span class="sxs-lookup"><span data-stu-id="9d425-141">If you completed hello steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on a different computer, you need tooreplace hello placeholders in hello `config-arduino.json` file.</span></span> <span data-ttu-id="9d425-142">Hej `config-arduino.json` filen har hello undermapp i arbetsmappen.</span><span class="sxs-lookup"><span data-stu-id="9d425-142">hello `config-arduino.json` file is in hello subfolder of your home folder.</span></span>

   ![Innehållet i hello arduino.json config-fil][config-arduino-json]

   * <span data-ttu-id="9d425-144">Ersätt **[Wi-Fi SSID]** med din Wi-Fi-SSID som anslutna toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="9d425-144">Replace **[Wi-Fi SSID]** with your Wi-Fi SSID that connected toohello Internet.</span></span>
   * <span data-ttu-id="9d425-145">Ersätt **[Wi-Fi-lösenord]** med Wi-Fi-lösenord.</span><span class="sxs-lookup"><span data-stu-id="9d425-145">Replace **[Wi-Fi password]** with your Wi-Fi password.</span></span> <span data-ttu-id="9d425-146">Ta bort hello sträng om din Wi-Fi inte kräver lösenord.</span><span class="sxs-lookup"><span data-stu-id="9d425-146">Remove hello string if your Wi-Fi doesn't require password.</span></span>
   * <span data-ttu-id="9d425-147">Ersätt **[anslutningssträngen för IoT-enhet]** med anslutningssträngen för hello enhet som du får genom att köra hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` kommando.</span><span class="sxs-lookup"><span data-stu-id="9d425-147">Replace **[IoT device connection string]** with hello device connection string that you get by running hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` command.</span></span>
   * <span data-ttu-id="9d425-148">Ersätt **[anslutningssträngen för IoT-hubb]** med hello anslutningssträngen för IoT-hubb som är tillgängliga genom att köra hello `az iot hub show-connection-string --name {my hub name}` kommando.</span><span class="sxs-lookup"><span data-stu-id="9d425-148">Replace **[IoT hub connection string]** with hello IoT hub connection string that you get by running hello `az iot hub show-connection-string --name {my hub name}` command.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="9d425-149">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="9d425-149">Deploy and run hello sample application</span></span>
<span data-ttu-id="9d425-150">Distribuera och köra hello exempelprogrammet på Arduino-kort genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="9d425-150">Deploy and run hello sample application on your Arduino board by running hello following commands:</span></span>

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

<span data-ttu-id="9d425-151">Hej gulp kommandot distribuerar hello exempel programmet tooyour Arduino kort.</span><span class="sxs-lookup"><span data-stu-id="9d425-151">hello gulp command deploys hello sample application tooyour Arduino board.</span></span> <span data-ttu-id="9d425-152">Sedan körs programmet hello på Arduino-kort och en separat åtgärd på din värd datorn toosend 20 blinka meddelanden tooyour Arduino board från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9d425-152">Then, it runs hello application on your Arduino board and a separate task on your host computer toosend 20 blink messages tooyour Arduino board from your IoT hub.</span></span>

<span data-ttu-id="9d425-153">När hello exempelprogrammet körs, startas lyssnar toomessages från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9d425-153">After hello sample application runs, it starts listening toomessages from your IoT hub.</span></span> <span data-ttu-id="9d425-154">Under tiden skickar hello gulp aktivitet flera ”blinkar” meddelanden från din IoT-hubb tooyour Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="9d425-154">Meanwhile, hello gulp task sends several "blink" messages from your IoT hub tooyour Arduino board.</span></span> <span data-ttu-id="9d425-155">För varje blinka meddelande som hello board får hello exempelprogrammet anropar hello `blinkLED` funktionen tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="9d425-155">For each blink message that hello board receives, hello sample application calls hello `blinkLED` function tooblink hello LED.</span></span>

<span data-ttu-id="9d425-156">Du bör se hello Indikator blinkar varannan sekund som hello gulp aktivitet skickar 20 meddelanden från din IoT-hubb tooyour Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="9d425-156">You should see hello LED blink every two seconds as hello gulp task sends 20 messages from your IoT hub tooyour Arduino board.</span></span> <span data-ttu-id="9d425-157">hello senast är en ett ”stop” visas som stoppar hello program från att köras.</span><span class="sxs-lookup"><span data-stu-id="9d425-157">hello last one is a "stop" message that stops hello application from running.</span></span>

![Exempelprogrammet med gulp kommandot och blinkar meddelanden][sample-application]

## <a name="summary"></a><span data-ttu-id="9d425-159">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="9d425-159">Summary</span></span>
<span data-ttu-id="9d425-160">Du har skickat meddelanden från din IoT-hubb tooyour Arduino board tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="9d425-160">You’ve successfully sent messages from your IoT hub tooyour Arduino board tooblink hello LED.</span></span> <span data-ttu-id="9d425-161">hello nästa uppgift är valfritt: ändra hello och inaktivera beteendet för hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="9d425-161">hello next task is optional: change hello on and off behavior of hello LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d425-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9d425-162">Next steps</span></span>
<span data-ttu-id="9d425-163">[Ändra hello och inaktivera beteendet för hello Indikator][change-the-on-and-off-led-behavior]</span><span class="sxs-lookup"><span data-stu-id="9d425-163">[Change hello on and off behavior of hello LED][change-the-on-and-off-led-behavior]</span></span>


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