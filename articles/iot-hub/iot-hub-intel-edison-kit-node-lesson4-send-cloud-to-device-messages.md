---
title: 'Ansluta Intel modern (nod) tooAzure IoT - lektionen 4: ta emot meddelanden | Microsoft Docs'
description: "Ett exempelprogram som körs på modern och övervakar inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden tooEdison från din IoT-hubb tooblink hello Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "arduino kontroll ledde från webben, arduino kontroll ledde via webben"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: bc738bf6-e38d-4024-82d7-39b6c2d4bacb
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: aab0ced4810dd3d4f5ba636940b06563f1db9241
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a><span data-ttu-id="bee80-105">Kör ett exempel programmet tooreceive meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="bee80-105">Run a sample application tooreceive cloud-to-device messages</span></span>
<span data-ttu-id="bee80-106">I den här artikeln kan du distribuera ett exempelprogram på Intel modern.</span><span class="sxs-lookup"><span data-stu-id="bee80-106">In this article, you deploy a sample application on Intel Edison.</span></span> <span data-ttu-id="bee80-107">hello exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bee80-107">hello sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="bee80-108">Du också köra en aktivitet med gulp på din dator toosend meddelanden tooEdison från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bee80-108">You also run a gulp task on your computer toosend messages tooEdison from your IoT hub.</span></span> <span data-ttu-id="bee80-109">När hello exempelprogrammet får hälsningsmeddelande, blinkar hello-Indikator.</span><span class="sxs-lookup"><span data-stu-id="bee80-109">When hello sample application receives hello messages, it blinks hello LED.</span></span> <span data-ttu-id="bee80-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="bee80-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="bee80-111">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="bee80-111">What you will do</span></span>
* <span data-ttu-id="bee80-112">Ansluta hello exempel programmet tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bee80-112">Connect hello sample application tooyour IoT hub.</span></span>
* <span data-ttu-id="bee80-113">Distribuera och köra hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="bee80-113">Deploy and run hello sample application.</span></span>
* <span data-ttu-id="bee80-114">Skicka meddelanden från din IoT-hubb tooEdison tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="bee80-114">Send messages from your IoT hub tooEdison tooblink hello LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="bee80-115">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="bee80-115">What you will learn</span></span>
<span data-ttu-id="bee80-116">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="bee80-116">In this article, you will learn:</span></span>
* <span data-ttu-id="bee80-117">Hur toomonitor inkommande meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bee80-117">How toomonitor incoming messages from your IoT hub.</span></span>
* <span data-ttu-id="bee80-118">Hur toosend moln till enhet meddelanden från din IoT-hubb tooEdison.</span><span class="sxs-lookup"><span data-stu-id="bee80-118">How toosend cloud-to-device messages from your IoT hub tooEdison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="bee80-119">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="bee80-119">What you need</span></span>
* <span data-ttu-id="bee80-120">Intel modern ställts in för användning.</span><span class="sxs-lookup"><span data-stu-id="bee80-120">Intel Edison, set up for use.</span></span> <span data-ttu-id="bee80-121">hur tooset in modern, se toolearn [konfigurera din enhet][configure-your-device].</span><span class="sxs-lookup"><span data-stu-id="bee80-121">toolearn how tooset up Edison, see [Configure your device][configure-your-device].</span></span>
* <span data-ttu-id="bee80-122">En IoT-hubb som skapas i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bee80-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="bee80-123">toolearn hur toocreate din IoT-hubb finns [skapa Azure IoT Hub][create-your-azure-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="bee80-123">toolearn how toocreate your IoT hub, see [Create your Azure IoT Hub][create-your-azure-iot-hub].</span></span>

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a><span data-ttu-id="bee80-124">Ansluta hello exempel programmet tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="bee80-124">Connect hello sample application tooyour IoT hub</span></span>
1. <span data-ttu-id="bee80-125">Kontrollera att du arbetar i hello lagringsplatsen mappen `iot-hub-node-edison-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="bee80-125">Make sure that you're in hello repo folder `iot-hub-node-edison-getting-started`.</span></span> <span data-ttu-id="bee80-126">Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="bee80-126">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd Lesson4
   code .
   ```

   <span data-ttu-id="bee80-127">hello-filen i hello `app` undermapp är hello källa fil som innehåller hello toomonitor inkommande meddelanden från hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bee80-127">hello file in hello `app` subfolder is hello key source file that contains hello code toomonitor incoming messages from hello IoT hub.</span></span> <span data-ttu-id="bee80-128">Hej `blinkLED` funktionen blinkar hello-Indikator.</span><span class="sxs-lookup"><span data-stu-id="bee80-128">hello `blinkLED` function blinks hello LED.</span></span>

   ![Lagringsplatsen strukturen i hello exempelprogram][repo-structure]
2. <span data-ttu-id="bee80-130">Initiera hello konfigurationsfilen genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="bee80-130">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```

   <span data-ttu-id="bee80-131">Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på den här datorn alla konfigurationer av hello ärvs, så du kan hoppa över hello steg toohello aktiviteten för att distribuera och Kör hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="bee80-131">If you completed hello steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on this computer, all hello configurations are inherited, so you can skip hello step toohello task of deploying and running hello sample application.</span></span> <span data-ttu-id="bee80-132">Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på en annan dator måste tooreplace hello platshållare i hello `config-edison.json` fil.</span><span class="sxs-lookup"><span data-stu-id="bee80-132">If you completed hello steps in [Create an Azure function app and storage account][create-an-azure-function-app-and-storage-account] on a different computer, you need tooreplace hello placeholders in hello `config-edison.json` file.</span></span> <span data-ttu-id="bee80-133">Hej `config-edison.json` filen har hello undermapp i arbetsmappen.</span><span class="sxs-lookup"><span data-stu-id="bee80-133">hello `config-edison.json` file is in hello subfolder of your home folder.</span></span>

   ![Innehållet i hello edison.json config-fil](media/iot-hub-intel-edison-lessons/lesson4/config-edison.png)

   * <span data-ttu-id="bee80-135">Ersätt **[enhet värdnamn eller IP-adress]** med hello enhetens IP-adress som du har markerat ned när du har konfigurerat din enhet.</span><span class="sxs-lookup"><span data-stu-id="bee80-135">Replace **[device hostname or IP address]** with hello device IP address you marked down when you configured your device.</span></span>
   * <span data-ttu-id="bee80-136">Ersätt **[anslutningssträngen för IoT-enhet]** med anslutningssträngen för hello enhet som du får genom att köra hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` kommando.</span><span class="sxs-lookup"><span data-stu-id="bee80-136">Replace **[IoT device connection string]** with hello device connection string that you get by running hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` command.</span></span>
   * <span data-ttu-id="bee80-137">Ersätt **[anslutningssträngen för IoT-hubb]** med hello anslutningssträngen för IoT-hubb som är tillgängliga genom att köra hello `az iot hub show-connection-string --name {my hub name}` kommando.</span><span class="sxs-lookup"><span data-stu-id="bee80-137">Replace **[IoT hub connection string]** with hello IoT hub connection string that you get by running hello `az iot hub show-connection-string --name {my hub name}` command.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="bee80-138">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="bee80-138">Deploy and run hello sample application</span></span>
<span data-ttu-id="bee80-139">Distribuera och köra hello exempelprogrammet på modern genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="bee80-139">Deploy and run hello sample application on Edison by running hello following commands:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="bee80-140">Hej gulp kommandot distribuerar hello exempel programmet tooEdison.</span><span class="sxs-lookup"><span data-stu-id="bee80-140">hello gulp command deploys hello sample application tooEdison.</span></span> <span data-ttu-id="bee80-141">Sedan körs programmet hello på modern och en separat åtgärd på din värd datorn toosend 20 blinka meddelanden tooEdison från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bee80-141">Then, it runs hello application on Edison and a separate task on your host computer toosend 20 blink messages tooEdison from your IoT hub.</span></span>

<span data-ttu-id="bee80-142">När hello exempelprogrammet körs, startas lyssnar toomessages från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bee80-142">After hello sample application runs, it starts listening toomessages from your IoT hub.</span></span> <span data-ttu-id="bee80-143">Under tiden skickar hello gulp aktivitet flera ”blinkar” meddelanden från din IoT-hubb tooEdison.</span><span class="sxs-lookup"><span data-stu-id="bee80-143">Meanwhile, hello gulp task sends several "blink" messages from your IoT hub tooEdison.</span></span> <span data-ttu-id="bee80-144">För varje meddelande blinka som tar emot modern hello exempelprogrammet anropar hello `blinkLED` funktionen tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="bee80-144">For each blink message that Edison receives, hello sample application calls hello `blinkLED` function tooblink hello LED.</span></span>

<span data-ttu-id="bee80-145">Du bör se hello Indikator blinka varannan sekund som hello gulp aktivitet skickar 20 meddelanden från din IoT-hubb tooEdison.</span><span class="sxs-lookup"><span data-stu-id="bee80-145">You should see hello LED blink every two seconds as hello gulp task sends 20 messages from your IoT hub tooEdison.</span></span> <span data-ttu-id="bee80-146">hello senast är en ett ”stop” visas som stoppar hello program från att köras.</span><span class="sxs-lookup"><span data-stu-id="bee80-146">hello last one is a "stop" message that stops hello application from running.</span></span>

![Exempelprogrammet med gulp kommandot och blinkar meddelanden][gulp-command-and-blink-messages]

## <a name="summary"></a><span data-ttu-id="bee80-148">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="bee80-148">Summary</span></span>
<span data-ttu-id="bee80-149">Du har skickat meddelanden från din IoT-hubb tooEdison tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="bee80-149">You’ve successfully sent messages from your IoT hub tooEdison tooblink hello LED.</span></span> <span data-ttu-id="bee80-150">hello nästa uppgift är valfritt: ändra hello och inaktivera beteendet för hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="bee80-150">hello next task is optional: change hello on and off behavior of hello LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bee80-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bee80-151">Next steps</span></span>
<span data-ttu-id="bee80-152">[Ändra hello och inaktivera beteendet för hello Indikator][change-the-on-and-off-behavior-of-the-led]</span><span class="sxs-lookup"><span data-stu-id="bee80-152">[Change hello on and off behavior of hello LED][change-the-on-and-off-behavior-of-the-led]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[configure-your-device]: iot-hub-intel-edison-kit-node-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson4/repo_structure.png
[create-an-azure-function-app-and-storage-account]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md
[gulp-command-and-blink-messages]: media/iot-hub-intel-edison-lessons/lesson4/gulp_blink.png
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-node-lesson4-change-led-behavior.md