---
title: 'Ansluta Intel modern (nod) tooAzure IoT - lektionen 1: distribuera appen | Microsoft Docs'
description: "Klona hello-exempelprogram C från GitHub och kör gulp toodeploy det här programmet tooyour Intel modern kort. Det här exempelprogrammet blinkar hello Indikator anslutna toohello board varannan sekund."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: arduino ledde projekt, arduino ledde blinka, arduino ledde blinka koden, arduino blinka program, arduino blinka exempel
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: ed2c21d0-c72c-4ac2-9e70-347e9a0711c0
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: bc03c7e45bd1ba9e9b2c8f2fec70a1be647e96b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a><span data-ttu-id="1dd76-105">Skapa och distribuera hello blinka program</span><span class="sxs-lookup"><span data-stu-id="1dd76-105">Create and deploy hello blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="1dd76-106">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="1dd76-106">What you will do</span></span>
<span data-ttu-id="1dd76-107">Klona hello-exempelprogram C från GitHub och använda hello gulp verktyget toodeploy hello exempel programmet tooIntel modern.</span><span class="sxs-lookup"><span data-stu-id="1dd76-107">Clone hello sample C application from GitHub, and use hello gulp tool toodeploy hello sample application tooIntel Edison.</span></span> <span data-ttu-id="1dd76-108">hello exempelprogrammet blinkar hello Indikator anslutna toohello board varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="1dd76-108">hello sample application blinks hello LED connected toohello board every two seconds.</span></span> <span data-ttu-id="1dd76-109">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="1dd76-109">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="1dd76-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="1dd76-110">What you will learn</span></span>
* <span data-ttu-id="1dd76-111">Hur toodeploy och kör hello exempelprogram på modern.</span><span class="sxs-lookup"><span data-stu-id="1dd76-111">How toodeploy and run hello sample application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="1dd76-112">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="1dd76-112">What you need</span></span>
<span data-ttu-id="1dd76-113">Du måste ha slutfört hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="1dd76-113">You must have successfully completed hello following operations:</span></span>

* <span data-ttu-id="1dd76-114">[Konfigurera din enhet][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="1dd76-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="1dd76-115">[Hämta hello-verktyg][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="1dd76-115">[Get hello tools][get-the-tools]</span></span>

## <a name="open-hello-sample-application"></a><span data-ttu-id="1dd76-116">Öppna hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="1dd76-116">Open hello sample application</span></span>
<span data-ttu-id="1dd76-117">tooopen hello exempelprogram, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="1dd76-117">tooopen hello sample application, follow these steps:</span></span>

1. <span data-ttu-id="1dd76-118">Klona hello exempel databasen från GitHub genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="1dd76-118">Clone hello sample repository from GitHub by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-edison-getting-started.git
   ```
2. <span data-ttu-id="1dd76-119">Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="1dd76-119">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd iot-hub-node-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Lagringsplatsen struktur][repo-structure]

<span data-ttu-id="1dd76-121">hello-filen i hello `app` undermapp är hello källa fil som innehåller hello kod toocontrol hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="1dd76-121">hello file in hello `app` subfolder is hello key source file that contains hello code toocontrol hello LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="1dd76-122">Installera beroenden</span><span class="sxs-lookup"><span data-stu-id="1dd76-122">Install application dependencies</span></span>
<span data-ttu-id="1dd76-123">Installera hello bibliotek och andra moduler som du behöver för hello exempelprogrammet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="1dd76-123">Install hello libraries and other modules you need for hello sample application by running hello following command:</span></span>

```bash
npm install
```

## <a name="configure-hello-device-connection"></a><span data-ttu-id="1dd76-124">Konfigurera hello enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="1dd76-124">Configure hello device connection</span></span>
<span data-ttu-id="1dd76-125">tooconfigure Hej enhetsanslutning, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="1dd76-125">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="1dd76-126">Generera hello konfigurationsfilen för enheten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="1dd76-126">Generate hello device configuration file by running hello following command:</span></span>

   ```bash
   gulp init
   ```

   <span data-ttu-id="1dd76-127">hello konfigurationsfilen `config-edison.json` innehåller hello-autentiseringsuppgifter som du använder toolog i tooEdison.</span><span class="sxs-lookup"><span data-stu-id="1dd76-127">hello configuration file `config-edison.json` contains hello user credentials you use toolog in tooEdison.</span></span> <span data-ttu-id="1dd76-128">tooavoid hello läckage av användarautentiseringsuppgifter, hello konfigurationsfilen genereras i hello undermapp `.iot-hub-getting-started` av hello arbetsmapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="1dd76-128">tooavoid hello leak of user credentials, hello configuration file is generated in hello subfolder `.iot-hub-getting-started` of hello home folder on your computer.</span></span>

2. <span data-ttu-id="1dd76-129">Öppna konfigurationsfilen för hello enheten i Visual Studio Code genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="1dd76-129">Open hello device configuration file in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. <span data-ttu-id="1dd76-130">Ersätt platshållaren hello `[device hostname or IP address]` och `[device password]` med hello IP-adress och lösenord som du har markerat i föregående lektionen.</span><span class="sxs-lookup"><span data-stu-id="1dd76-130">Replace hello placeholder `[device hostname or IP address]` and `[device password]` with hello IP address and password that you marked down in previous lesson.</span></span>

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

<span data-ttu-id="1dd76-132">Grattis!</span><span class="sxs-lookup"><span data-stu-id="1dd76-132">Congratulations!</span></span> <span data-ttu-id="1dd76-133">Du har skapat hello första exempelprogram för modern.</span><span class="sxs-lookup"><span data-stu-id="1dd76-133">You've successfully created hello first sample application for Edison.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="1dd76-134">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="1dd76-134">Deploy and run hello sample application</span></span>

### <a name="deploy-and-run-hello-sample-app"></a><span data-ttu-id="1dd76-135">Distribuera och köra hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="1dd76-135">Deploy and run hello sample app</span></span>
<span data-ttu-id="1dd76-136">Distribuera och köra hello exempelprogrammet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="1dd76-136">Deploy and run hello sample application by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a><span data-ttu-id="1dd76-137">Verifiera hello appen fungerar</span><span class="sxs-lookup"><span data-stu-id="1dd76-137">Verify hello app works</span></span>
<span data-ttu-id="1dd76-138">hello exempelprogrammet avbryter automatiskt när hello Indikator blinkar för 20 gånger.</span><span class="sxs-lookup"><span data-stu-id="1dd76-138">hello sample application terminates automatically after hello LED blinks for 20 times.</span></span> <span data-ttu-id="1dd76-139">Om du inte ser hello Indikator blinkar, se hello [felsökningsguide för] [ troubleshooting] för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="1dd76-139">If you don’t see hello LED blinking, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

![Blinka Indikator][led-blinking]

## <a name="summary"></a><span data-ttu-id="1dd76-141">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="1dd76-141">Summary</span></span>
<span data-ttu-id="1dd76-142">Du har installerat hello krävs verktyg toowork med modern och distribuerat en exempel programmet tooEdison tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="1dd76-142">You've installed hello required tools toowork with Edison and deployed a sample application tooEdison tooblink hello LED.</span></span> <span data-ttu-id="1dd76-143">Du kan nu skapa, distribuera, och kör en annan exempelprogram som ansluter modern tooAzure IoT-hubb toosend och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="1dd76-143">You can now create, deploy, and run another sample application that connects Edison tooAzure IoT Hub toosend and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1dd76-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1dd76-144">Next steps</span></span>
<span data-ttu-id="1dd76-145">[Hämta hello Azure-verktyg][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="1dd76-145">[Get hello Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-node-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md
