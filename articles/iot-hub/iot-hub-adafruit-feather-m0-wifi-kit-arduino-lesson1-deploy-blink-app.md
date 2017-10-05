---
title: 'Ansluta Arduino till Azure IoT - lektionen 1: distribuera appen | Microsoft Docs'
description: "Klona Arduino exempelprogrammet från GitHub och kör gulp om du vill distribuera programmet till din Adafruit ludd M0 WiFi. Det här exempelprogrammet blinkar på GPIO"
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
ms.openlocfilehash: 4431808ac6182d194e841c087c8f89f1a12b1911
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a><span data-ttu-id="e78f8-105">Skapa och distribuera blinkningsprogrammet</span><span class="sxs-lookup"><span data-stu-id="e78f8-105">Create and deploy the blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="e78f8-106">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="e78f8-106">What you will do</span></span>
<span data-ttu-id="e78f8-107">Klona Arduino exempelprogrammet från GitHub och verktyget gulp för att distribuera exempelprogrammet Adafruit ludd M0 WiFi Arduino-planen.</span><span class="sxs-lookup"><span data-stu-id="e78f8-107">Clone the sample Arduino application from GitHub, and use the gulp tool to deploy the sample application to your Adafruit Feather M0 WiFi Arduino board.</span></span> <span data-ttu-id="e78f8-108">Exempel programmet blinkningar den GPIO #13 på-barod VÄGLEDS varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="e78f8-108">The sample application blinks the GPIO #13 on-barod LED every two seconds.</span></span>

<span data-ttu-id="e78f8-109">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting-page].</span><span class="sxs-lookup"><span data-stu-id="e78f8-109">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting-page].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e78f8-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="e78f8-110">What you will learn</span></span>
* <span data-ttu-id="e78f8-111">Hur du distribuerar och kör exempelprogrammet på Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="e78f8-111">How to deploy and run the sample application on your Arduino board.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e78f8-112">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="e78f8-112">What you need</span></span>
<span data-ttu-id="e78f8-113">Du måste ha slutfört följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="e78f8-113">You must have successfully completed the following operations:</span></span>

* <span data-ttu-id="e78f8-114">[Konfigurera din enhet][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="e78f8-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="e78f8-115">[Skaffa dig verktyg][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="e78f8-115">[Get the tools][get-the-tools]</span></span>

## <a name="open-the-sample-application"></a><span data-ttu-id="e78f8-116">Öppna exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="e78f8-116">Open the sample application</span></span>
<span data-ttu-id="e78f8-117">Följ dessa steg om du vill öppna exempelprogrammet:</span><span class="sxs-lookup"><span data-stu-id="e78f8-117">To open the sample application, follow these steps:</span></span>

1. <span data-ttu-id="e78f8-118">Klona lagringsplatsen exempel från GitHub genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e78f8-118">Clone the sample repository from GitHub by running the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-feather-m0-getting-started.git
   ```
2. <span data-ttu-id="e78f8-119">Öppna exempelprogrammet i Visual Studio Code genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="e78f8-119">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd iot-hub-c-feather-m0-getting-started
   cd Lesson1
   code .
   ```

   ![Lagringsplatsen struktur][repo-structure]

<span data-ttu-id="e78f8-121">Den `app.ino` filen i den `app` undermapp är viktiga källfilen som innehåller koden för att styra Indikatorn.</span><span class="sxs-lookup"><span data-stu-id="e78f8-121">The `app.ino` file in the `app` subfolder is the key source file that contains the code to control the LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="e78f8-122">Installera beroenden</span><span class="sxs-lookup"><span data-stu-id="e78f8-122">Install application dependencies</span></span>
<span data-ttu-id="e78f8-123">Installera bibliotek och andra moduler som du behöver för exempelprogrammet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e78f8-123">Install the libraries and other modules you need for the sample application by running the following command:</span></span>

```bash
npm install
```

## <a name="configure-the-device-connection"></a><span data-ttu-id="e78f8-124">Konfigurera enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="e78f8-124">Configure the device connection</span></span>
<span data-ttu-id="e78f8-125">Följ dessa steg för att konfigurera enhetsanslutningen:</span><span class="sxs-lookup"><span data-stu-id="e78f8-125">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="e78f8-126">Hämta den seriella porten på enheten med enheten identifiering cli:</span><span class="sxs-lookup"><span data-stu-id="e78f8-126">Obtain the serial port of the device with the device discovery cli:</span></span>

   ```bash
   devdisco list --usb
   ```

   <span data-ttu-id="e78f8-127">Du bör se utdata som liknar följande och hitta usb COM-port för din Arduino board: ![identifiering av nätverksenheter][device-discovery]</span><span class="sxs-lookup"><span data-stu-id="e78f8-127">You should see an output that is similar to the following and find the usb COM port for your Arduino board: ![Device discovery][device-discovery]</span></span>

2. <span data-ttu-id="e78f8-128">Öppna filen `config.json` i mappen lektionen och lägga till värdet för hittade COM-portnummer:</span><span class="sxs-lookup"><span data-stu-id="e78f8-128">Open the file `config.json` in the lesson folder and add the value of the found COM port number:</span></span>

   ```json
   {
       "device_port" : "COM1"
   }
   ```
   ![Config.JSON][config-json]
   > [!NOTE]
   > <span data-ttu-id="e78f8-130">COM-porten på Windows-plattformen, den har formatet för `COM1, COM2, ...`.</span><span class="sxs-lookup"><span data-stu-id="e78f8-130">For the COM port, on Windows platform, it has the format of `COM1, COM2, ...`.</span></span> <span data-ttu-id="e78f8-131">I macOS eller Ubuntu det börjar med `/dev/`.</span><span class="sxs-lookup"><span data-stu-id="e78f8-131">On macOS or Ubuntu, it starts with `/dev/`.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="e78f8-132">Distribuera och köra exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="e78f8-132">Deploy and run the sample application</span></span>
### <a name="install-the-required-tools-for-your-arduino-board"></a><span data-ttu-id="e78f8-133">Installera nödvändiga för Arduino-kort</span><span class="sxs-lookup"><span data-stu-id="e78f8-133">Install the required tools for your Arduino board</span></span>

<span data-ttu-id="e78f8-134">Installera Azure IoT-hubb SDK för din Arduino board genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e78f8-134">Install the Azure IoT Hub SDK for your Arduino board by running the following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="e78f8-135">Den här uppgiften kan ta lång tid att slutföra beroende på nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="e78f8-135">This task might take a long time to complete, depending on your network connection.</span></span>

> [!NOTE]
> <span data-ttu-id="e78f8-136">Avsluta Arduino IDE-instans som körs när du kör gulp uppgifter: `install-tools`, `run`.</span><span class="sxs-lookup"><span data-stu-id="e78f8-136">Please exit the running Arduino IDE instance when running gulp tasks: `install-tools`, `run`.</span></span>

### <a name="deploy-and-run-the-sample-app"></a><span data-ttu-id="e78f8-137">Distribuera och köra sample-appen</span><span class="sxs-lookup"><span data-stu-id="e78f8-137">Deploy and run the sample app</span></span>
<span data-ttu-id="e78f8-138">Distribuera och köra exempelprogrammet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e78f8-138">Deploy and run the sample application by running the following command:</span></span>

```bash
gulp run

# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

### <a name="verify-the-app-works"></a><span data-ttu-id="e78f8-139">Kontrollera appen fungerar</span><span class="sxs-lookup"><span data-stu-id="e78f8-139">Verify the app works</span></span>
<span data-ttu-id="e78f8-140">Om du inte ser Indikator blinkar, finns det [felsökningsguide för] [ troubleshooting-page] efter lösningar på vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="e78f8-140">If you don’t see the LED blinking, see the [troubleshooting guide][troubleshooting-page] for solutions to common problems.</span></span>

![Blinka Indikator][led-blinking]

## <a name="summary"></a><span data-ttu-id="e78f8-142">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e78f8-142">Summary</span></span>
<span data-ttu-id="e78f8-143">Du har installerat de verktyg som krävs för att arbeta med Arduino-kort och distribuerat ett exempelprogram Arduino-planen blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="e78f8-143">You've installed the required tools to work with your Arduino board and deployed a sample application to your Arduino board to blink the LED.</span></span> <span data-ttu-id="e78f8-144">Du kan nu skapa, distribuera och köra en annan exempelprogrammet som ansluter Arduino-kort till Azure IoT Hub skicka och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e78f8-144">You can now create, deploy, and run another sample application that connects your Arduino board to Azure IoT Hub to send and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e78f8-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e78f8-145">Next steps</span></span>
<span data-ttu-id="e78f8-146">[Hämta Azure-verktyg][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="e78f8-146">[Get the Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting-page]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-blink-arduino-mac.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[led-blinking]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md