---
title: 'Connect Intel EDISON (C) till Azure IoT - lektionen 1: distribuera programmet | Microsoft Docs'
description: "Klona exempelprogrammet C från GitHub och kör gulp om du vill distribuera programmet till Intel modern-kort. Det här exempelprogrammet blinkar Indikator anslutna till kortet varannan sekund."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: arduino ledde projekt, arduino ledde blinka, arduino ledde blinka koden, arduino blinka program, arduino blinka exempel
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: b02dfd3f-28fd-4b52-8775-eb0eaf74d707
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c45ff5f41bdbc78da8532ffdcaaeec15c695f531
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a><span data-ttu-id="0864c-105">Skapa och distribuera blinkningsprogrammet</span><span class="sxs-lookup"><span data-stu-id="0864c-105">Create and deploy the blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="0864c-106">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="0864c-106">What you will do</span></span>
<span data-ttu-id="0864c-107">Klona exempelprogrammet C från GitHub och verktyget gulp för att distribuera exempelprogrammet till Intel modern.</span><span class="sxs-lookup"><span data-stu-id="0864c-107">Clone the sample C application from GitHub, and use the gulp tool to deploy the sample application to Intel Edison.</span></span> <span data-ttu-id="0864c-108">Exempelprogrammet blinkar Indikator anslutna till kortet varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="0864c-108">The sample application blinks the LED connected to the board every two seconds.</span></span> <span data-ttu-id="0864c-109">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="0864c-109">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="0864c-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="0864c-110">What you will learn</span></span>
* <span data-ttu-id="0864c-111">Hur du distribuerar och kör exempelprogrammet på modern.</span><span class="sxs-lookup"><span data-stu-id="0864c-111">How to deploy and run the sample application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="0864c-112">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="0864c-112">What you need</span></span>
<span data-ttu-id="0864c-113">Du måste ha slutfört följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="0864c-113">You must have successfully completed the following operations:</span></span>

* <span data-ttu-id="0864c-114">[Konfigurera din enhet][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="0864c-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="0864c-115">[Skaffa dig verktyg][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="0864c-115">[Get the tools][get-the-tools]</span></span>

## <a name="open-the-sample-application"></a><span data-ttu-id="0864c-116">Öppna exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="0864c-116">Open the sample application</span></span>
<span data-ttu-id="0864c-117">Följ dessa steg om du vill öppna exempelprogrammet:</span><span class="sxs-lookup"><span data-stu-id="0864c-117">To open the sample application, follow these steps:</span></span>

1. <span data-ttu-id="0864c-118">Klona lagringsplatsen exempel från GitHub genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0864c-118">Clone the sample repository from GitHub by running the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-edison-getting-started.git
   ```
2. <span data-ttu-id="0864c-119">Öppna exempelprogrammet i Visual Studio Code genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0864c-119">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd iot-hub-c-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Lagringsplatsen struktur][repo-structure]

<span data-ttu-id="0864c-121">Filen i den `app` undermapp är viktiga källfilen som innehåller koden för att styra Indikatorn.</span><span class="sxs-lookup"><span data-stu-id="0864c-121">The file in the `app` subfolder is the key source file that contains the code to control the LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="0864c-122">Installera beroenden</span><span class="sxs-lookup"><span data-stu-id="0864c-122">Install application dependencies</span></span>
<span data-ttu-id="0864c-123">Installera bibliotek och andra moduler som du behöver för exempelprogrammet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0864c-123">Install the libraries and other modules you need for the sample application by running the following command:</span></span>

```bash
npm install
```

## <a name="configure-the-device-connection"></a><span data-ttu-id="0864c-124">Konfigurera enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="0864c-124">Configure the device connection</span></span>
<span data-ttu-id="0864c-125">Följ dessa steg för att konfigurera enhetsanslutningen:</span><span class="sxs-lookup"><span data-stu-id="0864c-125">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="0864c-126">Generera konfigurationsfilen enheten genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0864c-126">Generate the device configuration file by running the following command:</span></span>

   ```bash
   gulp init
   ```

   <span data-ttu-id="0864c-127">Konfigurationsfilen `config-edison.json` innehåller autentiseringsuppgifter för användare som du använder för att logga in på modern.</span><span class="sxs-lookup"><span data-stu-id="0864c-127">The configuration file `config-edison.json` contains the user credentials you use to log in to Edison.</span></span> <span data-ttu-id="0864c-128">För att undvika läckage av användarautentiseringsuppgifter konfigurationsfilen genereras i undermappen `.iot-hub-getting-started` för arbetsmapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="0864c-128">To avoid the leak of user credentials, the configuration file is generated in the subfolder `.iot-hub-getting-started` of the home folder on your computer.</span></span>

2. <span data-ttu-id="0864c-129">Öppna konfigurationsfilen enheten i Visual Studio Code genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0864c-129">Open the device configuration file in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. <span data-ttu-id="0864c-130">Ersätt platshållaren `[device hostname or IP address]` och `[device password]` med IP-adress och lösenord som du har markerat i föregående lektionen.</span><span class="sxs-lookup"><span data-stu-id="0864c-130">Replace the placeholder `[device hostname or IP address]` and `[device password]` with the IP address and password that you marked down in previous lesson.</span></span>

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

<span data-ttu-id="0864c-132">Grattis!</span><span class="sxs-lookup"><span data-stu-id="0864c-132">Congratulations!</span></span> <span data-ttu-id="0864c-133">Du har skapat det första exempelprogrammet för modern.</span><span class="sxs-lookup"><span data-stu-id="0864c-133">You've successfully created the first sample application for Edison.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="0864c-134">Distribuera och köra exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="0864c-134">Deploy and run the sample application</span></span>
### <a name="install-the-azure-iot-hub-sdk-on-edison"></a><span data-ttu-id="0864c-135">Installera Azure IoT-hubb SDK på modern</span><span class="sxs-lookup"><span data-stu-id="0864c-135">Install the Azure IoT Hub SDK on Edison</span></span>
<span data-ttu-id="0864c-136">Installera Azure IoT-hubb SDK på modern genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0864c-136">Install the Azure IoT Hub SDK on Edison by running the following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="0864c-137">Den här uppgiften kan ta lång tid att slutföra beroende på nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="0864c-137">This task might take a long time to complete, depending on your network connection.</span></span> <span data-ttu-id="0864c-138">Den måste köras en gång för en modern.</span><span class="sxs-lookup"><span data-stu-id="0864c-138">It needs to be run only once for one Edison.</span></span>

### <a name="deploy-and-run-the-sample-app"></a><span data-ttu-id="0864c-139">Distribuera och köra sample-appen</span><span class="sxs-lookup"><span data-stu-id="0864c-139">Deploy and run the sample app</span></span>
<span data-ttu-id="0864c-140">Distribuera och köra exempelprogrammet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0864c-140">Deploy and run the sample application by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a><span data-ttu-id="0864c-141">Kontrollera appen fungerar</span><span class="sxs-lookup"><span data-stu-id="0864c-141">Verify the app works</span></span>
<span data-ttu-id="0864c-142">Exempelprogrammet avbryter automatiskt när Indikatorn blinkar för 20 gånger.</span><span class="sxs-lookup"><span data-stu-id="0864c-142">The sample application terminates automatically after the LED blinks for 20 times.</span></span> <span data-ttu-id="0864c-143">Om du inte ser Indikator blinkar, finns det [felsökningsguide för] [ troubleshooting] efter lösningar på vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="0864c-143">If you don’t see the LED blinking, see the [troubleshooting guide][troubleshooting] for solutions to common problems.</span></span>

![Blinka Indikator][led-blinking]

## <a name="summary"></a><span data-ttu-id="0864c-145">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0864c-145">Summary</span></span>
<span data-ttu-id="0864c-146">Du har installerat de verktyg som krävs för att arbeta med modern och distribuerat ett exempelprogram för modern blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="0864c-146">You've installed the required tools to work with Edison and deployed a sample application to Edison to blink the LED.</span></span> <span data-ttu-id="0864c-147">Du kan nu skapa, distribuera och köra en annan exempelprogrammet som ansluter modern till Azure IoT Hub skicka och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0864c-147">You can now create, deploy, and run another sample application that connects Edison to Azure IoT Hub to send and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0864c-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0864c-148">Next steps</span></span>
<span data-ttu-id="0864c-149">[Hämta Azure-verktyg][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="0864c-149">[Get the Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure_c.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking_c.jpg
[get-the-azure-tools]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
