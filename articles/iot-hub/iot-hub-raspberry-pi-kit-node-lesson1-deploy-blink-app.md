---
featureFlags: usabilla
title: 'Ansluta hallon Pi (nod) tooAzure IoT - lektionen 1: distribuera appen | Microsoft Docs'
description: "Klona hello Node.js exempelprogrammet från GitHub och gulp toodeploy det här programmet tooyour hallon Pi 3-kort. Det här exempelprogrammet blinkar hello Indikator anslutna toohello board varannan sekund."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Raspberry pi ledde blinka, blinka ledde med raspberry pi
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: a5a03a57-fe86-416f-90ff-6eca17775842
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 9732df3009b8342d4872fe2318a975a6251e772b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a><span data-ttu-id="62c7a-105">Skapa och distribuera hello blinka program</span><span class="sxs-lookup"><span data-stu-id="62c7a-105">Create and deploy hello blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="62c7a-106">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="62c7a-106">What you will do</span></span>
<span data-ttu-id="62c7a-107">Klona hello Node.js exempelprogrammet från GitHub och använda hello gulp verktyget toodeploy hello exempel programmet tooyour hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="62c7a-107">Clone hello sample Node.js application from GitHub and use hello gulp tool toodeploy hello sample application tooyour Raspberry Pi 3.</span></span> <span data-ttu-id="62c7a-108">hello exempelprogrammet blinkar hello Indikator anslutna toohello board varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="62c7a-108">hello sample application blinks hello LED connected toohello board every two seconds.</span></span> <span data-ttu-id="62c7a-109">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="62c7a-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="62c7a-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="62c7a-110">What you will learn</span></span>
<span data-ttu-id="62c7a-111">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="62c7a-111">In this article, you will learn:</span></span>

* <span data-ttu-id="62c7a-112">Hur toouse hello `device-discover-cli` verktyget tooretrieve nätverk information om Pi.</span><span class="sxs-lookup"><span data-stu-id="62c7a-112">How toouse hello `device-discover-cli` tool tooretrieve networking information about Pi.</span></span>
* <span data-ttu-id="62c7a-113">Hur toodeploy och kör hello exempelprogram med Pi.</span><span class="sxs-lookup"><span data-stu-id="62c7a-113">How toodeploy and run hello sample application on Pi.</span></span>
* <span data-ttu-id="62c7a-114">Hur toodeploy och felsöka program som körs på Pi.</span><span class="sxs-lookup"><span data-stu-id="62c7a-114">How toodeploy and debug applications running remotely on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="62c7a-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="62c7a-115">What you need</span></span>
<span data-ttu-id="62c7a-116">Du måste ha slutfört hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="62c7a-116">You must have successfully completed hello following operations:</span></span>

* [<span data-ttu-id="62c7a-117">Konfigurera din enhet</span><span class="sxs-lookup"><span data-stu-id="62c7a-117">Configure your device</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
* [<span data-ttu-id="62c7a-118">Hämta hello-verktyg</span><span class="sxs-lookup"><span data-stu-id="62c7a-118">Get hello tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="obtain-hello-ip-address-and-host-name-of-pi"></a><span data-ttu-id="62c7a-119">Hämta hello IP-adress och värddatornamn namn PI</span><span class="sxs-lookup"><span data-stu-id="62c7a-119">Obtain hello IP address and host name of Pi</span></span>
<span data-ttu-id="62c7a-120">Öppna en kommandotolk i Windows eller en terminal i macOS eller Ubuntu och kör sedan följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="62c7a-120">Open a command prompt in Windows or a terminal in macOS or Ubuntu, and then run hello following command:</span></span>

```bash
devdisco list --eth
```

<span data-ttu-id="62c7a-121">Du bör se utdata som är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="62c7a-121">You should see an output that is similar toohello following:</span></span>

![Identifiering av nätverksenheter](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

<span data-ttu-id="62c7a-123">Anteckna hello `IP address` och `hostname` pi.</span><span class="sxs-lookup"><span data-stu-id="62c7a-123">Take note of hello `IP address` and `hostname` of Pi.</span></span> <span data-ttu-id="62c7a-124">Du behöver den här informationen senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="62c7a-124">You need this information later in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="62c7a-125">Se till att Pi är anslutna toohello samma nätverk som datorn.</span><span class="sxs-lookup"><span data-stu-id="62c7a-125">Make sure that Pi is connected toohello same network as your computer.</span></span> <span data-ttu-id="62c7a-126">Till exempel om datorn är ansluten tooa trådlösa nätverk medan Pi är anslutna tooa kabelanslutet nätverk kan du kanske inte se hello IP-adress i hello devdisco utdata.</span><span class="sxs-lookup"><span data-stu-id="62c7a-126">For example, if your computer is connected tooa wireless network while Pi is connected tooa wired network, you might not see hello IP address in hello devdisco output.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="62c7a-127">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="62c7a-127">Clone hello sample application</span></span>
<span data-ttu-id="62c7a-128">tooopen hello exempelkoden, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="62c7a-128">tooopen hello sample code, follow these steps:</span></span>

1. <span data-ttu-id="62c7a-129">Klona hello exempel databasen från GitHub genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="62c7a-129">Clone hello sample repository from GitHub by running hello following command:</span></span>
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
   ```
2. <span data-ttu-id="62c7a-130">Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="62c7a-130">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>
   
   ```bash
   cd iot-hub-node-raspberrypi-getting-started
   cd Lesson1
   code .
   ```

![Lagringsplatsen struktur](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

<span data-ttu-id="62c7a-132">Hej `app.js` filen i hello `app` undermapp är hello källa fil som innehåller hello kod toocontrol hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="62c7a-132">hello `app.js` file in hello `app` subfolder is hello key source file that contains hello code toocontrol hello LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="62c7a-133">Installera beroenden</span><span class="sxs-lookup"><span data-stu-id="62c7a-133">Install application dependencies</span></span>
<span data-ttu-id="62c7a-134">Installera hello bibliotek och andra moduler som du behöver för hello exempelprogrammet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="62c7a-134">Install hello libraries and other modules you need for hello sample application by running hello following command:</span></span>

```bash
npm install
```

## <a name="configure-hello-device-connection"></a><span data-ttu-id="62c7a-135">Konfigurera hello enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="62c7a-135">Configure hello device connection</span></span>
<span data-ttu-id="62c7a-136">tooconfigure Hej enhetsanslutning, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="62c7a-136">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="62c7a-137">Generera hello konfigurationsfilen för enheten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="62c7a-137">Generate hello device configuration file by running hello following command:</span></span>
   
   ```bash
   gulp init
   ```
   
   <span data-ttu-id="62c7a-138">hello konfigurationsfilen `config-raspberrypi.json` innehåller hello-autentiseringsuppgifter som du använder toolog i tooPi.</span><span class="sxs-lookup"><span data-stu-id="62c7a-138">hello configuration file `config-raspberrypi.json` contains hello user credentials you use toolog in tooPi.</span></span> <span data-ttu-id="62c7a-139">tooavoid hello läckage av användarautentiseringsuppgifter, hello konfigurationsfilen genereras i hello undermapp `.iot-hub-getting-started` av hello arbetsmapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="62c7a-139">tooavoid hello leak of user credentials, hello configuration file is generated in hello subfolder `.iot-hub-getting-started` of hello home folder on your computer.</span></span>

2. <span data-ttu-id="62c7a-140">Öppna konfigurationsfilen för hello enheten i Visual Studio Code genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="62c7a-140">Open hello device configuration file in Visual Studio Code by running hello following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
3. <span data-ttu-id="62c7a-141">Ersätt platshållaren hello `[device hostname or IP address]` med hello IP-adress eller värdnamn för hello som du har fått tidigare i ”hämta hello IP-adress och värddatornamn name pi”.</span><span class="sxs-lookup"><span data-stu-id="62c7a-141">Replace hello placeholder `[device hostname or IP address]` with hello IP address or hello host name that you got previously in "Obtain hello IP address and host name of Pi."</span></span>
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> <span data-ttu-id="62c7a-143">Du kan använda SSH-nyckel i stället för användarnamn och lösenord när du ansluter tooRaspberry Pi.</span><span class="sxs-lookup"><span data-stu-id="62c7a-143">You can use SSH key instead of user name and password when connecting tooRaspberry Pi.</span></span> <span data-ttu-id="62c7a-144">I ordning toodo detta behöver toogenerate hello nyckel med **ssh-keygen** och **ssh-kopia-id pi @\<enhetsadress\>**.</span><span class="sxs-lookup"><span data-stu-id="62c7a-144">In order toodo this you will have toogenerate hello key using **ssh-keygen** and **ssh-copy-id pi@\<device address\>**.</span></span>
>
> <span data-ttu-id="62c7a-145">Dessa kommandon är tillgängliga i Windows **Git Bash**.</span><span class="sxs-lookup"><span data-stu-id="62c7a-145">On Windows these commands are available in **Git Bash**.</span></span>
>
> <span data-ttu-id="62c7a-146">I MacOS måste toorun **brew installera ssh-kopia-id**.</span><span class="sxs-lookup"><span data-stu-id="62c7a-146">On MacOS you need toorun **brew install ssh-copy-id**.</span></span>
>
> <span data-ttu-id="62c7a-147">Efter överföring har hello viktiga toohello hallon Pi, Ersätt **device_password** med **device_key_path** egenskap i **config raspberrypi.json**.</span><span class="sxs-lookup"><span data-stu-id="62c7a-147">After successfully uploading hello key toohello Raspberry Pi, replace **device_password** with **device_key_path** property in **config-raspberrypi.json**.</span></span>
>
> <span data-ttu-id="62c7a-148">Uppdaterade rader ska se ut som nedan:</span><span class="sxs-lookup"><span data-stu-id="62c7a-148">Updated lines should look as below:</span></span>
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

<span data-ttu-id="62c7a-149">Grattis!</span><span class="sxs-lookup"><span data-stu-id="62c7a-149">Congratulations!</span></span> <span data-ttu-id="62c7a-150">Du har skapat hello första exempelprogram för Pi.</span><span class="sxs-lookup"><span data-stu-id="62c7a-150">You've successfully created hello first sample application for Pi.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="62c7a-151">Distribuera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="62c7a-151">Deploy and run hello sample application</span></span>
### <a name="install-nodejs-and-npm-on-pi"></a><span data-ttu-id="62c7a-152">Installera Node.js och NPM på Pi</span><span class="sxs-lookup"><span data-stu-id="62c7a-152">Install Node.js and NPM on Pi</span></span>
<span data-ttu-id="62c7a-153">Installera Node.js och NPM på Pi genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="62c7a-153">Install Node.js and NPM on Pi by running hello following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="62c7a-154">Den här uppgiften kan ta 10 minuter toocomplete hello första gången du kör den.</span><span class="sxs-lookup"><span data-stu-id="62c7a-154">This task might take 10 minutes toocomplete hello first time you run it.</span></span>

### <a name="deploy-and-run-hello-sample-app"></a><span data-ttu-id="62c7a-155">Distribuera och köra hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="62c7a-155">Deploy and run hello sample app</span></span>
<span data-ttu-id="62c7a-156">Distribuera och köra hello exempelprogrammet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="62c7a-156">Deploy and run hello sample application by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a><span data-ttu-id="62c7a-157">Verifiera hello appen fungerar</span><span class="sxs-lookup"><span data-stu-id="62c7a-157">Verify hello app works</span></span>
<span data-ttu-id="62c7a-158">Du bör nu se hello Indikator på Pi blinkande varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="62c7a-158">You should now see hello LED on Pi blinking every two seconds.</span></span>  <span data-ttu-id="62c7a-159">Om du inte ser hello Indikator blinkar, se hello [felsökningsguide för](iot-hub-raspberry-pi-kit-node-troubleshooting.md) för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="62c7a-159">If you don’t see hello LED blinking, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions toocommon problems.</span></span>
<span data-ttu-id="62c7a-160">![Blinka Indikator](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span><span class="sxs-lookup"><span data-stu-id="62c7a-160">![LED blinking](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span></span>

## <a name="summary"></a><span data-ttu-id="62c7a-161">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="62c7a-161">Summary</span></span>
<span data-ttu-id="62c7a-162">Du har installerat hello krävs verktyg toowork med Pi och distribuerat en exempel programmet tooPi tooblink hello Indikator.</span><span class="sxs-lookup"><span data-stu-id="62c7a-162">You've installed hello required tools toowork with Pi and deployed a sample application tooPi tooblink hello LED.</span></span> <span data-ttu-id="62c7a-163">Du kan nu skapa, distribuera, och kör en annan exempelprogram som ansluter Pi tooAzure IoT-hubb toosend och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="62c7a-163">You can now create, deploy, and run another sample application that connects Pi tooAzure IoT Hub toosend and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="62c7a-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="62c7a-164">Next steps</span></span>
[<span data-ttu-id="62c7a-165">Hämta hello Azure-verktyg</span><span class="sxs-lookup"><span data-stu-id="62c7a-165">Get hello Azure tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

