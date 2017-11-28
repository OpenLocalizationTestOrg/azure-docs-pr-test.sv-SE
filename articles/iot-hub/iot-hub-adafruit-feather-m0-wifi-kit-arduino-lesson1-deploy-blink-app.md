---
title: 'Ansluta Arduino tooAzure IoT - lektionen 1: distribuera appen | Microsoft Docs'
description: "Klona hello Arduino exempelprogrammet från GitHub och kör det här programmet tooyour Adafruit ludd M0 WiFi för gulp toodeploy. Det här exempelprogrammet blinkar hello GPIO"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: arduino ledde projekt, arduino ledde blinka, arduino ledde blinka koden, arduino blinka program, arduino blinka exempel
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: b0a7d076-d580-4686-9f7d-c0712750b615
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5bf8e4ae88e070aeacf34bfc43b8d2daeeb1a2fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a><span data-ttu-id="f674d-105">Skapa och distribuera hello blinka program</span><span class="sxs-lookup"><span data-stu-id="f674d-105">Create and deploy hello blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="f674d-106">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="f674d-106">What you will do</span></span>
<span data-ttu-id="f674d-107">Klona hello Arduino exempelprogrammet från GitHub och använda hello gulp verktyget toodeploy hello exempel programmet tooyour Adafruit ludd M0 WiFi Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="f674d-107">Clone hello sample Arduino application from GitHub, and use hello gulp tool toodeploy hello sample application tooyour Adafruit Feather M0 WiFi Arduino board.</span></span> <span data-ttu-id="f674d-108">hello exempel programmet blinkningar hello GPIO #13 på barod VÄGLEDS varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="f674d-108">hello sample application blinks hello GPIO #13 on-barod LED every two seconds.</span></span>

<span data-ttu-id="f674d-109">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting-page].</span><span class="sxs-lookup"><span data-stu-id="f674d-109">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting-page].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f674d-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="f674d-110">What you will learn</span></span>
* <span data-ttu-id="f674d-111">Hur toodeploy och kör hello exempelprogram på Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="f674d-111">How toodeploy and run hello sample application on your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f674d-112">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="f674d-112">What you need</span></span>
<span data-ttu-id="f674d-113">Du måste ha slutfört hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="f674d-113">You must have successfully completed hello following operations:</span></span>

* <span data-ttu-id="f674d-114">[Konfigurera din enhet][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="f674d-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="f674d-115">[Hämta hello-verktyg][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="f674d-115">[Get hello tools][get-the-tools]</span></span>

## <a name="open-hello-sample-application"></a><span data-ttu-id="f674d-116">Öppna hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="f674d-116">Open hello sample application</span></span>
<span data-ttu-id="f674d-117">tooopen hello exempelprogram, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="f674d-117">tooopen hello sample application, follow these steps:</span></span>

1. <span data-ttu-id="f674d-118">Klona hello exempel databasen från GitHub genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f674d-118">Clone hello sample repository from GitHub by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-feather-m0-getting-started.git
   ```
2. <span data-ttu-id="f674d-119">Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="f674d-119">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd iot-hub-c-feather-m0-getting-started
   cd Lesson1
   code .
   ```

   ![Lagringsplatsen struktur][repo-structure]

<span data-ttu-id="f674d-121">Hej `app.ino` filen i hello `app` undermapp är hello källa fil som innehåller hello kod toocontrol hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="f674d-121">hello `app.ino` file in hello `app` subfolder is hello key source file that contains hello code toocontrol hello LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="f674d-122">Installera beroenden</span><span class="sxs-lookup"><span data-stu-id="f674d-122">Install application dependencies</span></span>
<span data-ttu-id="f674d-123">Installera hello bibliotek och andra moduler som du behöver för hello exempelprogrammet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f674d-123">Install hello libraries and other modules you need for hello sample application by running hello following command:</span></span>

```bash
npm install
```

## <a name="configure-hello-device-connection"></a><span data-ttu-id="f674d-124">Konfigurera hello enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="f674d-124">Configure hello device connection</span></span>
<span data-ttu-id="f674d-125">tooconfigure Hej enhetsanslutning, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="f674d-125">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="f674d-126">Hämta hello serieport av hello-enhet med hello enheten identifiering cli:</span><span class="sxs-lookup"><span data-stu-id="f674d-126">Obtain hello serial port of hello device with hello device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="f674d-127">Du bör se utdata som är liknande toohello följande och hitta hello usb COM-port för din Arduino board: ![identifiering av nätverksenheter][device-discovery]</span><span class="sxs-lookup"><span data-stu-id="f674d-127">You should see an output that is similar toohello following and find hello usb COM port for your Arduino board: ![Device discovery][device-discovery]</span></span>

2. <span data-ttu-id="f674d-128">Öppna hello filen `config.json` i hello lektionen mappen och Lägg till hello värdet hello hitta COM-portnummer:</span><span class="sxs-lookup"><span data-stu-id="f674d-128">Open hello file `config.json` in hello lesson folder and add hello value of hello found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```
   ![Config.JSON][config-json]
   > [!NOTE]
   > <span data-ttu-id="f674d-130">För hello COM-port på Windows-plattformen, den har hello format för `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="f674d-130">For hello COM port, on Windows platform, it has hello format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="f674d-131">I macOS eller Ubuntu det börjar med `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="f674d-131">On macOS or Ubuntu, it starts with `/dev/`.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="f674d-132">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="f674d-132">Deploy and run hello sample application</span></span>
### <a name="install-hello-required-tools-for-your-arduino-board"></a><span data-ttu-id="f674d-133">Installera hello som krävs för Arduino-kort</span><span class="sxs-lookup"><span data-stu-id="f674d-133">Install hello required tools for your Arduino board</span></span>

<span data-ttu-id="f674d-134">Installera hello Azure IoT-hubb SDK för din Arduino board genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f674d-134">Install hello Azure IoT Hub SDK for your Arduino board by running hello following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="f674d-135">Den här uppgiften kan ta en lång tid toocomplete, beroende på nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="f674d-135">This task might take a long time toocomplete, depending on your network connection.</span></span>

> [!NOTE]
> <span data-ttu-id="f674d-136">Avsluta hello körs Arduino IDE instans gulp uppgifter: `install-tools`, `run`.</span><span class="sxs-lookup"><span data-stu-id="f674d-136">Please exit hello running Arduino IDE instance when running gulp tasks: `install-tools`, `run`.</span></span>

### <a name="deploy-and-run-hello-sample-app"></a><span data-ttu-id="f674d-137">Distribuera och köra hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="f674d-137">Deploy and run hello sample app</span></span>
<span data-ttu-id="f674d-138">Distribuera och köra hello exempelprogrammet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f674d-138">Deploy and run hello sample application by running hello following command:</span></span>

```bash
gulp run

# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

### <a name="verify-hello-app-works"></a><span data-ttu-id="f674d-139">Verifiera hello appen fungerar</span><span class="sxs-lookup"><span data-stu-id="f674d-139">Verify hello app works</span></span>
<span data-ttu-id="f674d-140">Om du inte ser hello Indikator blinkar, se hello [felsökningsguide för] [ troubleshooting-page] för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="f674d-140">If you don’t see hello LED blinking, see hello [troubleshooting guide][troubleshooting-page] for solutions toocommon problems.</span></span>

![Blinka Indikator][led-blinking]

## <a name="summary"></a><span data-ttu-id="f674d-142">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f674d-142">Summary</span></span>
<span data-ttu-id="f674d-143">Du har installerat hello krävs verktyg toowork med Arduino-kort och distribuerat en exempel programmet tooyour Arduino board tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="f674d-143">You've installed hello required tools toowork with your Arduino board and deployed a sample application tooyour Arduino board tooblink hello LED.</span></span> <span data-ttu-id="f674d-144">Du kan nu skapa, distribuera, och kör en annan exempelprogram som ansluter din Arduino board tooAzure IoT-hubb toosend och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="f674d-144">You can now create, deploy, and run another sample application that connects your Arduino board tooAzure IoT Hub toosend and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f674d-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f674d-145">Next steps</span></span>
<span data-ttu-id="f674d-146">[Hämta hello Azure-verktyg][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="f674d-146">[Get hello Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting-page]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-blink-arduino-mac.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[led-blinking]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md