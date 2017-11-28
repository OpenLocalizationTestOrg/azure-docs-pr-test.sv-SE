---
title: 'Ansluta Raspberry Pi (C) till Azure IoT - lektionen 4: moln till enhet | Microsoft Docs'
description: "Ett exempelprogram som körs på Pi och övervakar inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden till Pi från din IoT-hubb blinkar på Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "moln till enhet, meddelande från molnet"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: fcbc0dd0-cae3-47b0-8e58-240e4f406f75
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 86c7be931319d9995c2a7311267c7e7c03c3c1b8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a><span data-ttu-id="8d403-105">Kör ett exempelprogram som tar emot meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="8d403-105">Run a sample application to receive cloud-to-device messages</span></span>
<span data-ttu-id="8d403-106">I den här artikeln kan du distribuera ett exempelprogram på hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="8d403-106">In this article, you deploy a sample application on Raspberry Pi 3.</span></span> <span data-ttu-id="8d403-107">Exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="8d403-107">The sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="8d403-108">Du kan också köra en aktivitet med gulp på datorn för att skicka meddelanden till Pi från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="8d403-108">You also run a gulp task on your computer to send messages to Pi from your IoT hub.</span></span> <span data-ttu-id="8d403-109">När exempelprogrammet som tar emot meddelanden, blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="8d403-109">When the sample application receives the messages, it blinks the LED.</span></span> <span data-ttu-id="8d403-110">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="8d403-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="8d403-111">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="8d403-111">What you will do</span></span>
* <span data-ttu-id="8d403-112">Ansluta till din IoT-hubb exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="8d403-112">Connect the sample application to your IoT hub.</span></span>
* <span data-ttu-id="8d403-113">Distribuera och köra exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="8d403-113">Deploy and run the sample application.</span></span>
* <span data-ttu-id="8d403-114">Skicka meddelanden från din IoT-hubb till Pi blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="8d403-114">Send messages from your IoT hub to Pi to blink the LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="8d403-115">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="8d403-115">What you will learn</span></span>
<span data-ttu-id="8d403-116">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="8d403-116">In this article, you will learn:</span></span>
* <span data-ttu-id="8d403-117">Så här övervakar du inkommande meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="8d403-117">How to monitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="8d403-118">Hur du skickar meddelanden moln till enhet från din IoT-hubb till Pi.</span><span class="sxs-lookup"><span data-stu-id="8d403-118">How to send cloud-to-device messages from your IoT hub to Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="8d403-119">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="8d403-119">What you need</span></span>
* <span data-ttu-id="8d403-120">Raspberry Pi 3 ställts in för användning.</span><span class="sxs-lookup"><span data-stu-id="8d403-120">Raspberry Pi 3, set up for use.</span></span> <span data-ttu-id="8d403-121">Information om hur du ställer in Pi finns [konfigurera din enhet](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md).</span><span class="sxs-lookup"><span data-stu-id="8d403-121">To learn how to set up Pi, see [Configure your device](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md).</span></span>
* <span data-ttu-id="8d403-122">En IoT-hubb som skapas i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8d403-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="8d403-123">Information om hur du skapar din IoT-hubb finns [skapa din IoT-hubb och registrera hallon Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="8d403-123">To learn how to create your IoT hub, see [Create your IoT hub and register Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span></span>

## <a name="connect-the-sample-application-to-your-iot-hub"></a><span data-ttu-id="8d403-124">Ansluta exempelprogrammet till din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="8d403-124">Connect the sample application to your IoT hub</span></span>
1. <span data-ttu-id="8d403-125">Kontrollera att du är i mappen lagringsplatsen `iot-hub-c-raspberrypi-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="8d403-125">Make sure that you're in the repo folder `iot-hub-c-raspberrypi-getting-started`.</span></span> <span data-ttu-id="8d403-126">Öppna exempelprogrammet i Visual Studio Code genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="8d403-126">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="8d403-127">Observera den `app.c` filen i den `app` undermappen.</span><span class="sxs-lookup"><span data-stu-id="8d403-127">Notice the `app.c` file in the `app` subfolder.</span></span> <span data-ttu-id="8d403-128">Den `app.c` filen är viktiga källfilen som innehåller koden för övervakning av inkommande meddelanden från IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="8d403-128">The `app.c` file is the key source file that contains the code to monitor incoming messages from the IoT hub.</span></span> <span data-ttu-id="8d403-129">Den `blinkLED` funktionen blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="8d403-129">The `blinkLED` function blinks the LED.</span></span>

   ![Lagringsplatsen strukturen i exempelprogrammet](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure_c.png)
2. <span data-ttu-id="8d403-131">Initiera konfigurationsfilen genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="8d403-131">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

   <span data-ttu-id="8d403-132">Om du har slutfört stegen i [skapa ett Azure-funktion appen och storage-konto](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) på den här datorn alla konfigurationer ärvs, så du kan hoppa till steg för att distribuera och köra exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="8d403-132">If you completed the steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) on this computer, all the configurations are inherited, so you can skip to step to the task of deploying and running the sample application.</span></span> <span data-ttu-id="8d403-133">Om du har slutfört stegen i [skapa ett Azure-funktion appen och storage-konto](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) på en annan dator som du behöver ersätta platshållare i den `config-raspberrypi.json` filen.</span><span class="sxs-lookup"><span data-stu-id="8d403-133">If you completed the steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) on a different computer, you need to replace the placeholders in the `config-raspberrypi.json` file.</span></span> <span data-ttu-id="8d403-134">Den `config-raspberrypi.json` filen finns i undermappen arbetsmappen.</span><span class="sxs-lookup"><span data-stu-id="8d403-134">The `config-raspberrypi.json` file is in the subfolder of your home folder.</span></span>

   ![Innehållet i filen config raspberrypi.json](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* <span data-ttu-id="8d403-136">Ersätt **[enhet värdnamn eller IP-adress]** med Pis IP-adressen eller värdnamnet namn som du kan hämta genom att köra den `devdisco list --eth` kommando.</span><span class="sxs-lookup"><span data-stu-id="8d403-136">Replace **[device hostname or IP address]** with Pi’s IP address or host name that you get by running the `devdisco list --eth` command.</span></span>
* <span data-ttu-id="8d403-137">Ersätt **[anslutningssträngen för IoT-enhet]** med den anslutningssträng för enheten som du får genom att köra den `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` kommando.</span><span class="sxs-lookup"><span data-stu-id="8d403-137">Replace **[IoT device connection string]** with the device connection string that you get by running the `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` command.</span></span>
* <span data-ttu-id="8d403-138">Ersätt **[anslutningssträngen för IoT-hubb]** med IoT-hubb anslutningssträngen som du får genom att köra den `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` kommando.</span><span class="sxs-lookup"><span data-stu-id="8d403-138">Replace **[IoT hub connection string]** with the IoT hub connection string that you get by running the `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` command.</span></span>

> [!NOTE]
> <span data-ttu-id="8d403-139">Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.</span><span class="sxs-lookup"><span data-stu-id="8d403-139">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="8d403-140">Distribuera och köra exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="8d403-140">Deploy and run the sample application</span></span>
<span data-ttu-id="8d403-141">Distribuera och köra exempelprogrammet på Pi genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="8d403-141">Deploy and run the sample application on Pi by running the following commands:</span></span>

```
gulp deploy && gulp run
```

<span data-ttu-id="8d403-142">Kommandot gulp körs aktiviteten installera verktyg först.</span><span class="sxs-lookup"><span data-stu-id="8d403-142">The gulp command runs the install-tools task first.</span></span> <span data-ttu-id="8d403-143">Den distribuerar sedan exempelprogrammet till Pi.</span><span class="sxs-lookup"><span data-stu-id="8d403-143">Then it deploys the sample application to Pi.</span></span> <span data-ttu-id="8d403-144">Slutligen körs programmet på Pi och en separat åtgärd på värddatorn för att skicka 20 blinka meddelanden till Pi från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="8d403-144">Finally, it runs the application on Pi and a separate task on your host computer to send 20 blink messages to Pi from your IoT hub.</span></span>

<span data-ttu-id="8d403-145">När exempelprogrammet som körs, startas lyssnar på meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="8d403-145">After the sample application runs, it starts listening to messages from your IoT hub.</span></span> <span data-ttu-id="8d403-146">Under tiden skickar aktiviteten gulp flera ”blinkar” meddelanden från din IoT-hubb till Pi.</span><span class="sxs-lookup"><span data-stu-id="8d403-146">Meanwhile, the gulp task sends several "blink" messages from your IoT hub to Pi.</span></span> <span data-ttu-id="8d403-147">För varje meddelande blinka som tar emot Pi exempelprogrammet anropar den `blinkLED` funktionen blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="8d403-147">For each blink message that Pi receives, the sample application calls the `blinkLED` function to blink the LED.</span></span>

<span data-ttu-id="8d403-148">Du bör se Indikator blinka varannan sekund som aktiviteten gulp 20 meddelanden skickas från din IoT-hubb till Pi.</span><span class="sxs-lookup"><span data-stu-id="8d403-148">You should see the LED blink every two seconds as the gulp task sends 20 messages from your IoT hub to Pi.</span></span> <span data-ttu-id="8d403-149">Den sista som är en ”stoppa” meddelande som förhindrar att program körs.</span><span class="sxs-lookup"><span data-stu-id="8d403-149">The last one is a "stop" message that stops the application from running.</span></span>

![Exempelprogrammet med gulp kommandot och blinkar meddelanden](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink_c.png)

## <a name="summary"></a><span data-ttu-id="8d403-151">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="8d403-151">Summary</span></span>
<span data-ttu-id="8d403-152">Du har skickat meddelanden från din IoT-hubb till Pi blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="8d403-152">You’ve successfully sent messages from your IoT hub to Pi to blink the LED.</span></span> <span data-ttu-id="8d403-153">Nästa uppgift är valfritt: ändra på och av beteendet för Indikatorn.</span><span class="sxs-lookup"><span data-stu-id="8d403-153">The next task is optional: change the on and off behavior of the LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d403-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d403-154">Next steps</span></span>
[<span data-ttu-id="8d403-155">Ändra på och av beteendet för Indikatorn</span><span class="sxs-lookup"><span data-stu-id="8d403-155">Change the on and off behavior of the LED</span></span>](iot-hub-raspberry-pi-kit-c-lesson4-change-led-behavior.md)
