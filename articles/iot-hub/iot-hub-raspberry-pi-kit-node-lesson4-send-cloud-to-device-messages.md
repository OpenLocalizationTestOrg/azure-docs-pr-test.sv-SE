---
featureFlags: usabilla
title: 'Ansluta hallon Pi (nod) tooAzure IoT - lektionen 4: moln till enhet | Microsoft Docs'
description: "hello exempelprogrammet körs på Pi och övervakar inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden tooPi från din IoT-hubb tooblink hello Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "molnet toodevice, meddelande från molnet"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 6ae6539e-1163-4490-8d72-fdf7803e3054
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d69ded4e30c27378481ab2a4fb9c5b73be7bd44e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-hello-sample-application-tooreceive-cloud-to-device-messages"></a><span data-ttu-id="c525c-105">Kör exempel programmet tooreceive moln till enhet hälsningsmeddelande</span><span class="sxs-lookup"><span data-stu-id="c525c-105">Run hello sample application tooreceive cloud-to-device messages</span></span>
<span data-ttu-id="c525c-106">I den här artikeln kan du distribuera ett exempelprogram på hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="c525c-106">In this article, you deploy a sample application on Raspberry Pi 3.</span></span> <span data-ttu-id="c525c-107">hello exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c525c-107">hello sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="c525c-108">Du också köra en aktivitet med gulp på din dator toosend meddelanden tooPi från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c525c-108">You also run a gulp task on your computer toosend messages tooPi from your IoT hub.</span></span> <span data-ttu-id="c525c-109">När hello exempelprogrammet får hälsningsmeddelande, blinkar hello-Indikator.</span><span class="sxs-lookup"><span data-stu-id="c525c-109">When hello sample application receives hello messages, it blinks hello LED.</span></span> <span data-ttu-id="c525c-110">Om du har några problem med sökning lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="c525c-110">If you have any problems, seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="c525c-111">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="c525c-111">What you will do</span></span>
* <span data-ttu-id="c525c-112">Ansluta hello exempel programmet tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c525c-112">Connect hello sample application tooyour IoT hub.</span></span>
* <span data-ttu-id="c525c-113">Distribuera och köra hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="c525c-113">Deploy and run hello sample application.</span></span>
* <span data-ttu-id="c525c-114">Skicka meddelanden från din IoT-hubb tooPi tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="c525c-114">Send messages from your IoT hub tooPi tooblink hello LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="c525c-115">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="c525c-115">What you will learn</span></span>
<span data-ttu-id="c525c-116">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="c525c-116">In this article, you will learn:</span></span>
* <span data-ttu-id="c525c-117">Hur toomonitor inkommande meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="c525c-117">How toomonitor incoming messages from your IoT hub</span></span>
* <span data-ttu-id="c525c-118">Hur toosend moln till enhet meddelanden från din IoT-hubb tooPi.</span><span class="sxs-lookup"><span data-stu-id="c525c-118">How toosend cloud-to-device messages from your IoT hub tooPi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="c525c-119">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="c525c-119">What you need</span></span>
* <span data-ttu-id="c525c-120">Raspberry Pi 3 ställts in för användning.</span><span class="sxs-lookup"><span data-stu-id="c525c-120">Raspberry Pi 3, set up for use.</span></span> <span data-ttu-id="c525c-121">hur tooset in Pi, se toolearn [konfigurera din enhet](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md).</span><span class="sxs-lookup"><span data-stu-id="c525c-121">toolearn how tooset up Pi, see [Configure your device](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md).</span></span>
* <span data-ttu-id="c525c-122">En IoT-hubb som skapas i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c525c-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="c525c-123">toolearn hur toocreate din IoT-hubb finns [skapa din IoT-hubb och registrera hallon Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="c525c-123">toolearn how toocreate your IoT hub, see [Create your IoT hub and register Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span></span>

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a><span data-ttu-id="c525c-124">Ansluta hello exempel programmet tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="c525c-124">Connect hello sample application tooyour IoT hub</span></span>
1. <span data-ttu-id="c525c-125">Kontrollera att du arbetar i hello lagringsplatsen mappen `iot-hub-node-raspberrypi-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="c525c-125">Make sure that you're in hello repo folder `iot-hub-node-raspberrypi-getting-started`.</span></span> <span data-ttu-id="c525c-126">Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="c525c-126">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>
   
   ```bash
   cd Lesson4
   code .
   ```
   
   <span data-ttu-id="c525c-127">Meddelande hello `app.js` filen i hello `app` undermappen.</span><span class="sxs-lookup"><span data-stu-id="c525c-127">Notice hello `app.js` file in hello `app` subfolder.</span></span> <span data-ttu-id="c525c-128">Hej `app.js` filen är hello källa som innehåller hello toomonitor inkommande meddelanden från hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c525c-128">hello `app.js` file is hello key source file that contains hello code toomonitor incoming messages from hello IoT hub.</span></span> <span data-ttu-id="c525c-129">Hej `blinkLED` funktionen blinkar hello-Indikator.</span><span class="sxs-lookup"><span data-stu-id="c525c-129">hello `blinkLED` function blinks hello LED.</span></span>
   
   ![Lagringsplatsen strukturen i hello exempelprogram](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)
2. <span data-ttu-id="c525c-131">Initiera hello-konfigurationsfilen med hjälp av hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="c525c-131">Initialize hello configuration file by using hello following commands:</span></span>
   
   ```bash
   npm install
   gulp init
   ```
   
   <span data-ttu-id="c525c-132">Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) på den här datorn alla konfigurationer av hello ärvs, så du kan hoppa över toohello uppgiften att distribuera och köra hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="c525c-132">If you completed hello steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) on this computer, all hello configurations are inherited, so you can skip toohello task of deploying and running hello sample application.</span></span> <span data-ttu-id="c525c-133">Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) på en annan dator måste tooreplace hello platshållare i hello `config-raspberrypi.json` fil.</span><span class="sxs-lookup"><span data-stu-id="c525c-133">If you completed hello steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) on a different computer, you need tooreplace hello placeholders in hello `config-raspberrypi.json` file.</span></span> <span data-ttu-id="c525c-134">Hej `config-raspberrypi.json` filen har hello undermapp i arbetsmappen.</span><span class="sxs-lookup"><span data-stu-id="c525c-134">hello `config-raspberrypi.json` file is in hello subfolder of your home folder.</span></span>
   
   ![Innehållet i hello raspberrypi.json config-fil](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* <span data-ttu-id="c525c-136">Ersätt **[enhet värdnamn eller IP-adress]** med hello IP-adress för Pi eller hello värdnamn som du får genom att köra hello `devdisco list --eth` kommando.</span><span class="sxs-lookup"><span data-stu-id="c525c-136">Replace **[device hostname or IP address]** with hello IP address of Pi or hello host name that you get by running hello `devdisco list --eth` command.</span></span>
* <span data-ttu-id="c525c-137">Ersätt **[anslutningssträngen för IoT-enhet]** med anslutningssträngen för hello enhet som du får genom att köra hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` kommando.</span><span class="sxs-lookup"><span data-stu-id="c525c-137">Replace **[IoT device connection string]** with hello device connection string that you get by running hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` command.</span></span>
* <span data-ttu-id="c525c-138">Ersätt **[anslutningssträngen för IoT-hubb]** med hello anslutningssträngen för IoT-hubb som är tillgängliga genom att köra hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` kommando.</span><span class="sxs-lookup"><span data-stu-id="c525c-138">Replace **[IoT hub connection string]** with hello IoT hub connection string that you get by running hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` command.</span></span>

> [!NOTE]
> <span data-ttu-id="c525c-139">Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.</span><span class="sxs-lookup"><span data-stu-id="c525c-139">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="c525c-140">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="c525c-140">Deploy and run hello sample application</span></span>
<span data-ttu-id="c525c-141">Distribuera och köra hello exempelprogrammet på Pi genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="c525c-141">Deploy and run hello sample application on Pi by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="c525c-142">hello kommandot distribuerar hello exempel programmet tooPi.</span><span class="sxs-lookup"><span data-stu-id="c525c-142">hello command deploys hello sample application tooPi.</span></span> <span data-ttu-id="c525c-143">Sedan körs programmet hello på Pi och en separat åtgärd på din värd datorn toosend 20 blinka meddelanden tooPi från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c525c-143">Then, it runs hello application on Pi and a separate task on your host computer toosend 20 blink messages tooPi from your IoT hub.</span></span>

<span data-ttu-id="c525c-144">När hello exempelprogrammet körs, startas lyssnar toomessages från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c525c-144">After hello sample application runs, it starts listening toomessages from your IoT hub.</span></span> <span data-ttu-id="c525c-145">Under tiden skickar hello gulp aktivitet flera ”blinkar” meddelanden från din IoT-hubb tooPi.</span><span class="sxs-lookup"><span data-stu-id="c525c-145">Meanwhile, hello gulp task sends several "blink" messages from your IoT hub tooPi.</span></span> <span data-ttu-id="c525c-146">För varje meddelande blinka som tar emot Pi hello exempelprogrammet anropar hello `blinkLED` funktionen tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="c525c-146">For each blink message that Pi receives, hello sample application calls hello `blinkLED` function tooblink hello LED.</span></span>

<span data-ttu-id="c525c-147">Du bör se hello Indikator blinka varannan sekund som hello gulp aktivitet skickar 20 meddelanden från din IoT-hubb tooPi.</span><span class="sxs-lookup"><span data-stu-id="c525c-147">You should see hello LED blink every two seconds as hello gulp task sends 20 messages from your IoT hub tooPi.</span></span> <span data-ttu-id="c525c-148">hello senast är en ett ”stop” visas som talar om hello programmet toostop körs.</span><span class="sxs-lookup"><span data-stu-id="c525c-148">hello last one is a "stop" message that tells hello application toostop running.</span></span>

![Exempelprogrammet med gulp kommandot och blinkar meddelanden](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="summary"></a><span data-ttu-id="c525c-150">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c525c-150">Summary</span></span>
<span data-ttu-id="c525c-151">Du har skickat meddelanden från din IoT-hubb tooPi tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="c525c-151">You’ve successfully sent messages from your IoT hub tooPi tooblink hello LED.</span></span> <span data-ttu-id="c525c-152">hello nästa uppgift är valfritt: ändra hello och inaktivera beteendet för hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="c525c-152">hello next task is optional: change hello on and off behavior of hello LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c525c-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c525c-153">Next steps</span></span>
[<span data-ttu-id="c525c-154">Ändra hello och inaktivera beteendet för hello Indikator</span><span class="sxs-lookup"><span data-stu-id="c525c-154">Change hello on and off behavior of hello LED</span></span>](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)

