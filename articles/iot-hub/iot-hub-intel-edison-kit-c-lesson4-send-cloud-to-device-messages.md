---
title: 'Connect Intel EDISON (C) till Azure IoT - lektionen 4: ta emot meddelanden | Microsoft Docs'
description: "Ett exempelprogram som körs på modern och övervakar inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden till modern från din IoT-hubb blinkar på Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "arduino kontroll ledde från webben, arduino kontroll ledde via webben"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 820d38f3-d3b8-4249-9e2b-f1b9b771e62f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b7de7a8b53cdb1d7c2560225fce9166e555e5123
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a><span data-ttu-id="4f990-105">Kör ett exempelprogram som tar emot meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="4f990-105">Run a sample application to receive cloud-to-device messages</span></span>
<span data-ttu-id="4f990-106">I den här artikeln kan du distribuera ett exempelprogram på Intel modern.</span><span class="sxs-lookup"><span data-stu-id="4f990-106">In this article, you deploy a sample application on Intel Edison.</span></span> <span data-ttu-id="4f990-107">Exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4f990-107">The sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="4f990-108">Du kan också köra en aktivitet med gulp på datorn för att skicka meddelanden till modern från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4f990-108">You also run a gulp task on your computer to send messages to Edison from your IoT hub.</span></span> <span data-ttu-id="4f990-109">När exempelprogrammet som tar emot meddelanden, blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="4f990-109">When the sample application receives the messages, it blinks the LED.</span></span> <span data-ttu-id="4f990-110">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="4f990-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="4f990-111">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="4f990-111">What you will do</span></span>
* <span data-ttu-id="4f990-112">Ansluta till din IoT-hubb exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="4f990-112">Connect the sample application to your IoT hub.</span></span>
* <span data-ttu-id="4f990-113">Distribuera och köra exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="4f990-113">Deploy and run the sample application.</span></span>
* <span data-ttu-id="4f990-114">Skicka meddelanden från din IoT-hubb för modern blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="4f990-114">Send messages from your IoT hub to Edison to blink the LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="4f990-115">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="4f990-115">What you will learn</span></span>
<span data-ttu-id="4f990-116">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="4f990-116">In this article, you will learn:</span></span>
* <span data-ttu-id="4f990-117">Så här övervakar du inkommande meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4f990-117">How to monitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="4f990-118">Hur du skickar meddelanden moln till enhet från din IoT-hubb till modern.</span><span class="sxs-lookup"><span data-stu-id="4f990-118">How to send cloud-to-device messages from your IoT hub to Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="4f990-119">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="4f990-119">What you need</span></span>
* <span data-ttu-id="4f990-120">Intel modern ställts in för användning.</span><span class="sxs-lookup"><span data-stu-id="4f990-120">Intel Edison, set up for use.</span></span> <span data-ttu-id="4f990-121">Information om hur du ställer in modern finns [konfigurera din enhet][configure-your-device].</span><span class="sxs-lookup"><span data-stu-id="4f990-121">To learn how to set up Edison, see [Configure your device][configure-your-device].</span></span>
* <span data-ttu-id="4f990-122">En IoT-hubb som skapas i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4f990-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="4f990-123">Information om hur du skapar din IoT-hubb finns [skapa Azure IoT Hub][create-your-azure-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="4f990-123">To learn how to create your IoT hub, see [Create your Azure IoT Hub][create-your-azure-iot-hub].</span></span>

## <a name="connect-the-sample-application-to-your-iot-hub"></a><span data-ttu-id="4f990-124">Ansluta exempelprogrammet till din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="4f990-124">Connect the sample application to your IoT hub</span></span>
1. <span data-ttu-id="4f990-125">Kontrollera att du är i mappen lagringsplatsen `iot-hub-c-edison-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="4f990-125">Make sure that you're in the repo folder `iot-hub-c-edison-getting-started`.</span></span> <span data-ttu-id="4f990-126">Öppna exempelprogrammet i Visual Studio Code genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="4f990-126">Open the sample application in Visual Studio Code by running the following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="4f990-127">Filen i den `app` undermapp är viktiga källfilen som innehåller koden för övervakning av inkommande meddelanden från IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="4f990-127">The file in the `app` subfolder is the key source file that contains the code to monitor incoming messages from the IoT hub.</span></span> <span data-ttu-id="4f990-128">Den `blinkLED` funktionen blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="4f990-128">The `blinkLED` function blinks the LED.</span></span>

   ![Lagringsplatsen strukturen i exempelprogrammet][repo-structure]
2. <span data-ttu-id="4f990-130">Initiera konfigurationsfilen genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="4f990-130">Initialize the configuration file by running the following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

   <span data-ttu-id="4f990-131">Om du har slutfört stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på den här datorn alla konfigurationer ärvs, så du kan hoppa över steg för att distribuera och köra exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="4f990-131">If you completed the steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on this computer, all the configurations are inherited, so you can skip the step to the task of deploying and running the sample application.</span></span> <span data-ttu-id="4f990-132">Om du har slutfört stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på en annan dator som du behöver ersätta platshållare i den `config-edison.json` filen.</span><span class="sxs-lookup"><span data-stu-id="4f990-132">If you completed the steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on a different computer, you need to replace the placeholders in the `config-edison.json` file.</span></span> <span data-ttu-id="4f990-133">Den `config-edison.json` filen finns i undermappen arbetsmappen.</span><span class="sxs-lookup"><span data-stu-id="4f990-133">The `config-edison.json` file is in the subfolder of your home folder.</span></span>

   ![Innehållet i filen config edison.json](media/iot-hub-intel-edison-lessons/lesson4/config-edison.png)

   * <span data-ttu-id="4f990-135">Ersätt **[enhet värdnamn eller IP-adress]** med enhetens IP-adress som du har markerat ned när du har konfigurerat din enhet.</span><span class="sxs-lookup"><span data-stu-id="4f990-135">Replace **[device hostname or IP address]** with the device IP address you marked down when you configured your device.</span></span>
   * <span data-ttu-id="4f990-136">Ersätt **[anslutningssträngen för IoT-enhet]** med den anslutningssträng för enheten som du får genom att köra den `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` kommando.</span><span class="sxs-lookup"><span data-stu-id="4f990-136">Replace **[IoT device connection string]** with the device connection string that you get by running the `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` command.</span></span>
   * <span data-ttu-id="4f990-137">Ersätt **[anslutningssträngen för IoT-hubb]** med IoT-hubb anslutningssträngen som du får genom att köra den `az iot hub show-connection-string --name {my hub name}` kommando.</span><span class="sxs-lookup"><span data-stu-id="4f990-137">Replace **[IoT hub connection string]** with the IoT hub connection string that you get by running the `az iot hub show-connection-string --name {my hub name}` command.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4f990-138">Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.</span><span class="sxs-lookup"><span data-stu-id="4f990-138">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="4f990-139">Distribuera och köra exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="4f990-139">Deploy and run the sample application</span></span>
<span data-ttu-id="4f990-140">Distribuera och köra exempelprogrammet på modern genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="4f990-140">Deploy and run the sample application on Edison by running the following commands:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="4f990-141">Kommandot gulp distribuerar exempelprogrammet till modern.</span><span class="sxs-lookup"><span data-stu-id="4f990-141">The gulp command deploys the sample application to Edison.</span></span> <span data-ttu-id="4f990-142">Därefter körs programmet för modern och en separat åtgärd på värddatorn för att skicka 20 blinka meddelanden till modern från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4f990-142">Then, it runs the application on Edison and a separate task on your host computer to send 20 blink messages to Edison from your IoT hub.</span></span>

<span data-ttu-id="4f990-143">När exempelprogrammet som körs, startas lyssnar på meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4f990-143">After the sample application runs, it starts listening to messages from your IoT hub.</span></span> <span data-ttu-id="4f990-144">Gulp-aktivitet skickar under tiden kan flera ”blinkar” meddelanden från din IoT-hubb till modern.</span><span class="sxs-lookup"><span data-stu-id="4f990-144">Meanwhile, the gulp task sends several "blink" messages from your IoT hub to Edison.</span></span> <span data-ttu-id="4f990-145">För varje meddelande blinka som tar emot modern exempelprogrammet anropar den `blinkLED` funktionen blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="4f990-145">For each blink message that Edison receives, the sample application calls the `blinkLED` function to blink the LED.</span></span>

<span data-ttu-id="4f990-146">Du bör se Indikator blinka varannan sekund som aktiviteten gulp 20 meddelanden skickas från din IoT-hubb till modern.</span><span class="sxs-lookup"><span data-stu-id="4f990-146">You should see the LED blink every two seconds as the gulp task sends 20 messages from your IoT hub to Edison.</span></span> <span data-ttu-id="4f990-147">Den sista som är en ”stoppa” meddelande som förhindrar att program körs.</span><span class="sxs-lookup"><span data-stu-id="4f990-147">The last one is a "stop" message that stops the application from running.</span></span>

![Exempelprogrammet med gulp kommandot och blinkar meddelanden][gulp-command-and-blink-messages]

## <a name="summary"></a><span data-ttu-id="4f990-149">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="4f990-149">Summary</span></span>
<span data-ttu-id="4f990-150">Du har skickat meddelanden från din IoT-hubb till modern blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="4f990-150">You’ve successfully sent messages from your IoT hub to Edison to blink the LED.</span></span> <span data-ttu-id="4f990-151">Nästa uppgift är valfritt: ändra på och av beteendet för Indikatorn.</span><span class="sxs-lookup"><span data-stu-id="4f990-151">The next task is optional: change the on and off behavior of the LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f990-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4f990-152">Next steps</span></span>
<span data-ttu-id="4f990-153">[Ändra på och av beteendet för Indikatorn][change-the-on-and-off-behavior-of-the-led]</span><span class="sxs-lookup"><span data-stu-id="4f990-153">[Change the on and off behavior of the LED][change-the-on-and-off-behavior-of-the-led]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson4/repo_structure_c.png
[create-an-azure-function-app-and-storage-account]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[gulp-command-and-blink-messages]: media/iot-hub-intel-edison-lessons/lesson4/gulp_blink_c.png
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-c-lesson4-change-led-behavior.md