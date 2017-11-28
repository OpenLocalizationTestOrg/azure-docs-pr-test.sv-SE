---
title: 'Connect Intel EDISON (C) tooAzure IoT - lektionen 1: distribuera programmet | Microsoft Docs'
description: "Klona hello-exempelprogram C från GitHub och kör gulp toodeploy det här programmet tooyour Intel modern kort. Det här exempelprogrammet blinkar hello Indikator anslutna toohello board varannan sekund."
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
ms.openlocfilehash: fa84fae812dd742a2ad4997a5e213c8e40e6fcf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a><span data-ttu-id="bd094-105">Skapa och distribuera hello blinka program</span><span class="sxs-lookup"><span data-stu-id="bd094-105">Create and deploy hello blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="bd094-106">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="bd094-106">What you will do</span></span>
<span data-ttu-id="bd094-107">Klona hello-exempelprogram C från GitHub och använda hello gulp verktyget toodeploy hello exempel programmet tooIntel modern.</span><span class="sxs-lookup"><span data-stu-id="bd094-107">Clone hello sample C application from GitHub, and use hello gulp tool toodeploy hello sample application tooIntel Edison.</span></span> <span data-ttu-id="bd094-108">hello exempelprogrammet blinkar hello Indikator anslutna toohello board varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="bd094-108">hello sample application blinks hello LED connected toohello board every two seconds.</span></span> <span data-ttu-id="bd094-109">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="bd094-109">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="bd094-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="bd094-110">What you will learn</span></span>
* <span data-ttu-id="bd094-111">Hur toodeploy och kör hello exempelprogram på modern.</span><span class="sxs-lookup"><span data-stu-id="bd094-111">How toodeploy and run hello sample application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="bd094-112">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="bd094-112">What you need</span></span>
<span data-ttu-id="bd094-113">Du måste ha slutfört hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="bd094-113">You must have successfully completed hello following operations:</span></span>

* <span data-ttu-id="bd094-114">[Konfigurera din enhet][configure-your-device]</span><span class="sxs-lookup"><span data-stu-id="bd094-114">[Configure your device][configure-your-device]</span></span>
* <span data-ttu-id="bd094-115">[Hämta hello-verktyg][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="bd094-115">[Get hello tools][get-the-tools]</span></span>

## <a name="open-hello-sample-application"></a><span data-ttu-id="bd094-116">Öppna hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="bd094-116">Open hello sample application</span></span>
<span data-ttu-id="bd094-117">tooopen hello exempelprogram, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="bd094-117">tooopen hello sample application, follow these steps:</span></span>

1. <span data-ttu-id="bd094-118">Klona hello exempel databasen från GitHub genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="bd094-118">Clone hello sample repository from GitHub by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-edison-getting-started.git
   ```
2. <span data-ttu-id="bd094-119">Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="bd094-119">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>

   ```bash
   cd iot-hub-c-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Lagringsplatsen struktur][repo-structure]

<span data-ttu-id="bd094-121">hello-filen i hello `app` undermapp är hello källa fil som innehåller hello kod toocontrol hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="bd094-121">hello file in hello `app` subfolder is hello key source file that contains hello code toocontrol hello LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="bd094-122">Installera beroenden</span><span class="sxs-lookup"><span data-stu-id="bd094-122">Install application dependencies</span></span>
<span data-ttu-id="bd094-123">Installera hello bibliotek och andra moduler som du behöver för hello exempelprogrammet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="bd094-123">Install hello libraries and other modules you need for hello sample application by running hello following command:</span></span>

```bash
npm install
```

## <a name="configure-hello-device-connection"></a><span data-ttu-id="bd094-124">Konfigurera hello enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="bd094-124">Configure hello device connection</span></span>
<span data-ttu-id="bd094-125">tooconfigure Hej enhetsanslutning, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="bd094-125">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="bd094-126">Generera hello konfigurationsfilen för enheten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="bd094-126">Generate hello device configuration file by running hello following command:</span></span>

   ```bash
   gulp init
   ```

   <span data-ttu-id="bd094-127">hello konfigurationsfilen `config-edison.json` innehåller hello-autentiseringsuppgifter som du använder toolog i tooEdison.</span><span class="sxs-lookup"><span data-stu-id="bd094-127">hello configuration file `config-edison.json` contains hello user credentials you use toolog in tooEdison.</span></span> <span data-ttu-id="bd094-128">tooavoid hello läckage av användarautentiseringsuppgifter, hello konfigurationsfilen genereras i hello undermapp `.iot-hub-getting-started` av hello arbetsmapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="bd094-128">tooavoid hello leak of user credentials, hello configuration file is generated in hello subfolder `.iot-hub-getting-started` of hello home folder on your computer.</span></span>

2. <span data-ttu-id="bd094-129">Öppna konfigurationsfilen för hello enheten i Visual Studio Code genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="bd094-129">Open hello device configuration file in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. <span data-ttu-id="bd094-130">Ersätt platshållaren hello `[device hostname or IP address]` och `[device password]` med hello IP-adress och lösenord som du har markerat i föregående lektionen.</span><span class="sxs-lookup"><span data-stu-id="bd094-130">Replace hello placeholder `[device hostname or IP address]` and `[device password]` with hello IP address and password that you marked down in previous lesson.</span></span>

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

<span data-ttu-id="bd094-132">Grattis!</span><span class="sxs-lookup"><span data-stu-id="bd094-132">Congratulations!</span></span> <span data-ttu-id="bd094-133">Du har skapat hello första exempelprogram för modern.</span><span class="sxs-lookup"><span data-stu-id="bd094-133">You've successfully created hello first sample application for Edison.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="bd094-134">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="bd094-134">Deploy and run hello sample application</span></span>
### <a name="install-hello-azure-iot-hub-sdk-on-edison"></a><span data-ttu-id="bd094-135">Installera hello Azure IoT-hubb SDK på modern</span><span class="sxs-lookup"><span data-stu-id="bd094-135">Install hello Azure IoT Hub SDK on Edison</span></span>
<span data-ttu-id="bd094-136">Installera hello Azure IoT-hubb SDK på modern genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="bd094-136">Install hello Azure IoT Hub SDK on Edison by running hello following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="bd094-137">Den här uppgiften kan ta en lång tid toocomplete, beroende på nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="bd094-137">This task might take a long time toocomplete, depending on your network connection.</span></span> <span data-ttu-id="bd094-138">Det krävs toobe köras en gång för en modern.</span><span class="sxs-lookup"><span data-stu-id="bd094-138">It needs toobe run only once for one Edison.</span></span>

### <a name="deploy-and-run-hello-sample-app"></a><span data-ttu-id="bd094-139">Distribuera och köra hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="bd094-139">Deploy and run hello sample app</span></span>
<span data-ttu-id="bd094-140">Distribuera och köra hello exempelprogrammet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="bd094-140">Deploy and run hello sample application by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a><span data-ttu-id="bd094-141">Verifiera hello appen fungerar</span><span class="sxs-lookup"><span data-stu-id="bd094-141">Verify hello app works</span></span>
<span data-ttu-id="bd094-142">hello exempelprogrammet avbryter automatiskt när hello Indikator blinkar för 20 gånger.</span><span class="sxs-lookup"><span data-stu-id="bd094-142">hello sample application terminates automatically after hello LED blinks for 20 times.</span></span> <span data-ttu-id="bd094-143">Om du inte ser hello Indikator blinkar, se hello [felsökningsguide för] [ troubleshooting] för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="bd094-143">If you don’t see hello LED blinking, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

![Blinka Indikator][led-blinking]

## <a name="summary"></a><span data-ttu-id="bd094-145">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="bd094-145">Summary</span></span>
<span data-ttu-id="bd094-146">Du har installerat hello krävs verktyg toowork med modern och distribuerat en exempel programmet tooEdison tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="bd094-146">You've installed hello required tools toowork with Edison and deployed a sample application tooEdison tooblink hello LED.</span></span> <span data-ttu-id="bd094-147">Du kan nu skapa, distribuera, och kör en annan exempelprogram som ansluter modern tooAzure IoT-hubb toosend och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bd094-147">You can now create, deploy, and run another sample application that connects Edison tooAzure IoT Hub toosend and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd094-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bd094-148">Next steps</span></span>
<span data-ttu-id="bd094-149">[Hämta hello Azure-verktyg][get-the-azure-tools]</span><span class="sxs-lookup"><span data-stu-id="bd094-149">[Get hello Azure tools][get-the-azure-tools]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure_c.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking_c.jpg
[get-the-azure-tools]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
