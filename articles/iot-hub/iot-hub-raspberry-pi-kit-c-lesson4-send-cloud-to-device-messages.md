---
title: 'Connect Raspberry PI (C) tooAzure IoT - lektionen 4: moln till enhet | Microsoft Docs'
description: "Ett exempelprogram som körs på Pi och övervakar inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden tooPi från din IoT-hubb tooblink hello Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "molnet toodevice, meddelande från molnet"
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
ms.openlocfilehash: 5596bf3a83c21f2bd54b2f83e2a8fdad7a608b94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a><span data-ttu-id="10b5b-105">Kör ett exempel programmet tooreceive meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="10b5b-105">Run a sample application tooreceive cloud-to-device messages</span></span>
<span data-ttu-id="10b5b-106">I den här artikeln kan du distribuera ett exempelprogram på hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="10b5b-106">In this article, you deploy a sample application on Raspberry Pi 3.</span></span> <span data-ttu-id="10b5b-107">hello exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="10b5b-107">hello sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="10b5b-108">Du också köra en aktivitet med gulp på din dator toosend meddelanden tooPi från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="10b5b-108">You also run a gulp task on your computer toosend messages tooPi from your IoT hub.</span></span> <span data-ttu-id="10b5b-109">När hello exempelprogrammet får hälsningsmeddelande, blinkar hello-Indikator.</span><span class="sxs-lookup"><span data-stu-id="10b5b-109">When hello sample application receives hello messages, it blinks hello LED.</span></span> <span data-ttu-id="10b5b-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="10b5b-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="10b5b-111">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="10b5b-111">What you will do</span></span>
* <span data-ttu-id="10b5b-112">Ansluta hello exempel programmet tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="10b5b-112">Connect hello sample application tooyour IoT hub.</span></span>
* <span data-ttu-id="10b5b-113">Distribuera och köra hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="10b5b-113">Deploy and run hello sample application.</span></span>
* <span data-ttu-id="10b5b-114">Skicka meddelanden från din IoT-hubb tooPi tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="10b5b-114">Send messages from your IoT hub tooPi tooblink hello LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="10b5b-115">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="10b5b-115">What you will learn</span></span>
<span data-ttu-id="10b5b-116">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="10b5b-116">In this article, you will learn:</span></span>
* <span data-ttu-id="10b5b-117">Hur toomonitor inkommande meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="10b5b-117">How toomonitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="10b5b-118">Hur toosend moln till enhet meddelanden från din IoT-hubb tooPi.</span><span class="sxs-lookup"><span data-stu-id="10b5b-118">How toosend cloud-to-device messages from your IoT hub tooPi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="10b5b-119">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="10b5b-119">What you need</span></span>
* <span data-ttu-id="10b5b-120">Raspberry Pi 3 ställts in för användning.</span><span class="sxs-lookup"><span data-stu-id="10b5b-120">Raspberry Pi 3, set up for use.</span></span> <span data-ttu-id="10b5b-121">hur tooset in Pi, se toolearn [konfigurera din enhet](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md).</span><span class="sxs-lookup"><span data-stu-id="10b5b-121">toolearn how tooset up Pi, see [Configure your device](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md).</span></span>
* <span data-ttu-id="10b5b-122">En IoT-hubb som skapas i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="10b5b-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="10b5b-123">toolearn hur toocreate din IoT-hubb finns [skapa din IoT-hubb och registrera hallon Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="10b5b-123">toolearn how toocreate your IoT hub, see [Create your IoT hub and register Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span></span>

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a><span data-ttu-id="10b5b-124">Ansluta hello exempel programmet tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="10b5b-124">Connect hello sample application tooyour IoT hub</span></span>
1. <span data-ttu-id="10b5b-125">Kontrollera att du arbetar i hello lagringsplatsen mappen `iot-hub-c-raspberrypi-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="10b5b-125">Make sure that you're in hello repo folder `iot-hub-c-raspberrypi-getting-started`.</span></span> <span data-ttu-id="10b5b-126">Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="10b5b-126">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="10b5b-127">Meddelande hello `app.c` filen i hello `app` undermappen.</span><span class="sxs-lookup"><span data-stu-id="10b5b-127">Notice hello `app.c` file in hello `app` subfolder.</span></span> <span data-ttu-id="10b5b-128">Hej `app.c` filen är hello källa som innehåller hello toomonitor inkommande meddelanden från hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="10b5b-128">hello `app.c` file is hello key source file that contains hello code toomonitor incoming messages from hello IoT hub.</span></span> <span data-ttu-id="10b5b-129">Hej `blinkLED` funktionen blinkar hello-Indikator.</span><span class="sxs-lookup"><span data-stu-id="10b5b-129">hello `blinkLED` function blinks hello LED.</span></span>

   ![Lagringsplatsen strukturen i hello exempelprogram](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure_c.png)
2. <span data-ttu-id="10b5b-131">Initiera hello konfigurationsfilen genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="10b5b-131">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

   <span data-ttu-id="10b5b-132">Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) på den här datorn alla konfigurationer av hello ärvs, så du kan hoppa över toostep toohello uppgiften att distribuera och köra hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="10b5b-132">If you completed hello steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) on this computer, all hello configurations are inherited, so you can skip toostep toohello task of deploying and running hello sample application.</span></span> <span data-ttu-id="10b5b-133">Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) på en annan dator måste tooreplace hello platshållare i hello `config-raspberrypi.json` fil.</span><span class="sxs-lookup"><span data-stu-id="10b5b-133">If you completed hello steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) on a different computer, you need tooreplace hello placeholders in hello `config-raspberrypi.json` file.</span></span> <span data-ttu-id="10b5b-134">Hej `config-raspberrypi.json` filen har hello undermapp i arbetsmappen.</span><span class="sxs-lookup"><span data-stu-id="10b5b-134">hello `config-raspberrypi.json` file is in hello subfolder of your home folder.</span></span>

   ![Innehållet i hello raspberrypi.json config-fil](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* <span data-ttu-id="10b5b-136">Ersätt **[enhet värdnamn eller IP-adress]** med Pis IP-adressen eller värdnamnet namn som du får genom att köra hello `devdisco list --eth` kommando.</span><span class="sxs-lookup"><span data-stu-id="10b5b-136">Replace **[device hostname or IP address]** with Pi’s IP address or host name that you get by running hello `devdisco list --eth` command.</span></span>
* <span data-ttu-id="10b5b-137">Ersätt **[anslutningssträngen för IoT-enhet]** med anslutningssträngen för hello enhet som du får genom att köra hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` kommando.</span><span class="sxs-lookup"><span data-stu-id="10b5b-137">Replace **[IoT device connection string]** with hello device connection string that you get by running hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` command.</span></span>
* <span data-ttu-id="10b5b-138">Ersätt **[anslutningssträngen för IoT-hubb]** med hello anslutningssträngen för IoT-hubb som är tillgängliga genom att köra hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` kommando.</span><span class="sxs-lookup"><span data-stu-id="10b5b-138">Replace **[IoT hub connection string]** with hello IoT hub connection string that you get by running hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` command.</span></span>

> [!NOTE]
> <span data-ttu-id="10b5b-139">Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.</span><span class="sxs-lookup"><span data-stu-id="10b5b-139">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="10b5b-140">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="10b5b-140">Deploy and run hello sample application</span></span>
<span data-ttu-id="10b5b-141">Distribuera och köra hello exempelprogrammet på Pi genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="10b5b-141">Deploy and run hello sample application on Pi by running hello following commands:</span></span>

```
gulp deploy && gulp run
```

<span data-ttu-id="10b5b-142">Hej gulp kommandot körs hello först installera verktyg aktivitet.</span><span class="sxs-lookup"><span data-stu-id="10b5b-142">hello gulp command runs hello install-tools task first.</span></span> <span data-ttu-id="10b5b-143">Den distribuerar sedan hello exempel programmet tooPi.</span><span class="sxs-lookup"><span data-stu-id="10b5b-143">Then it deploys hello sample application tooPi.</span></span> <span data-ttu-id="10b5b-144">Slutligen körs programmet hello på Pi och en separat åtgärd på din värd datorn toosend 20 blinka meddelanden tooPi från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="10b5b-144">Finally, it runs hello application on Pi and a separate task on your host computer toosend 20 blink messages tooPi from your IoT hub.</span></span>

<span data-ttu-id="10b5b-145">När hello exempelprogrammet körs, startas lyssnar toomessages från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="10b5b-145">After hello sample application runs, it starts listening toomessages from your IoT hub.</span></span> <span data-ttu-id="10b5b-146">Under tiden skickar hello gulp aktivitet flera ”blinkar” meddelanden från din IoT-hubb tooPi.</span><span class="sxs-lookup"><span data-stu-id="10b5b-146">Meanwhile, hello gulp task sends several "blink" messages from your IoT hub tooPi.</span></span> <span data-ttu-id="10b5b-147">För varje meddelande blinka som tar emot Pi hello exempelprogrammet anropar hello `blinkLED` funktionen tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="10b5b-147">For each blink message that Pi receives, hello sample application calls hello `blinkLED` function tooblink hello LED.</span></span>

<span data-ttu-id="10b5b-148">Du bör se hello Indikator blinka varannan sekund som hello gulp aktivitet skickar 20 meddelanden från din IoT-hubb tooPi.</span><span class="sxs-lookup"><span data-stu-id="10b5b-148">You should see hello LED blink every two seconds as hello gulp task sends 20 messages from your IoT hub tooPi.</span></span> <span data-ttu-id="10b5b-149">hello senast är en ett ”stop” visas som stoppar hello program från att köras.</span><span class="sxs-lookup"><span data-stu-id="10b5b-149">hello last one is a "stop" message that stops hello application from running.</span></span>

![Exempelprogrammet med gulp kommandot och blinkar meddelanden](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink_c.png)

## <a name="summary"></a><span data-ttu-id="10b5b-151">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="10b5b-151">Summary</span></span>
<span data-ttu-id="10b5b-152">Du har skickat meddelanden från din IoT-hubb tooPi tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="10b5b-152">You’ve successfully sent messages from your IoT hub tooPi tooblink hello LED.</span></span> <span data-ttu-id="10b5b-153">hello nästa uppgift är valfritt: ändra hello och inaktivera beteendet för hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="10b5b-153">hello next task is optional: change hello on and off behavior of hello LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="10b5b-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="10b5b-154">Next steps</span></span>
[<span data-ttu-id="10b5b-155">Ändra hello och inaktivera beteendet för hello Indikator</span><span class="sxs-lookup"><span data-stu-id="10b5b-155">Change hello on and off behavior of hello LED</span></span>](iot-hub-raspberry-pi-kit-c-lesson4-change-led-behavior.md)
