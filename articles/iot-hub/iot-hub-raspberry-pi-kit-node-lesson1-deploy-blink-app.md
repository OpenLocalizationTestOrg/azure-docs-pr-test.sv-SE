---
featureFlags: usabilla
title: 'Ansluta Raspberry Pi (nod) till Azure IoT - lektionen 1: distribuera appen | Microsoft Docs'
description: "Klona Node.js exempelprogrammet från GitHub och gulp om du vill distribuera programmet till hallon Pi 3-kort. Det här exempelprogrammet blinkar Indikator anslutna till kortet varannan sekund."
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
ms.openlocfilehash: 8b73000c166950172c07b8e188025dc9da5bc011
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a><span data-ttu-id="871ac-105">Skapa och distribuera blinkningsprogrammet</span><span class="sxs-lookup"><span data-stu-id="871ac-105">Create and deploy the blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="871ac-106">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="871ac-106">What you will do</span></span>
<span data-ttu-id="871ac-107">Klona Node.js exempelprogrammet från GitHub och verktyget gulp för att distribuera exempelprogrammet till din hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="871ac-107">Clone the sample Node.js application from GitHub and use the gulp tool to deploy the sample application to your Raspberry Pi 3.</span></span> <span data-ttu-id="871ac-108">Exempelprogrammet blinkar Indikator anslutna till kortet varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="871ac-108">The sample application blinks the LED connected to the board every two seconds.</span></span> <span data-ttu-id="871ac-109">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="871ac-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="871ac-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="871ac-110">What you will learn</span></span>
<span data-ttu-id="871ac-111">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="871ac-111">In this article, you will learn:</span></span>

* <span data-ttu-id="871ac-112">Hur du använder den `device-discover-cli` verktyg för att hämta nätverksinformation om Pi.</span><span class="sxs-lookup"><span data-stu-id="871ac-112">How to use the `device-discover-cli` tool to retrieve networking information about Pi.</span></span>
* <span data-ttu-id="871ac-113">Hur du distribuerar och kör exempelprogrammet på Pi.</span><span class="sxs-lookup"><span data-stu-id="871ac-113">How to deploy and run the sample application on Pi.</span></span>
* <span data-ttu-id="871ac-114">Hur du distribuerar och felsöka program som körs på Pi.</span><span class="sxs-lookup"><span data-stu-id="871ac-114">How to deploy and debug applications running remotely on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="871ac-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="871ac-115">What you need</span></span>
<span data-ttu-id="871ac-116">Du måste ha slutfört följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="871ac-116">You must have successfully completed the following operations:</span></span>

* [<span data-ttu-id="871ac-117">Konfigurera din enhet</span><span class="sxs-lookup"><span data-stu-id="871ac-117">Configure your device</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
* [<span data-ttu-id="871ac-118">Skaffa dig verktyg</span><span class="sxs-lookup"><span data-stu-id="871ac-118">Get the tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="obtain-the-ip-address-and-host-name-of-pi"></a><span data-ttu-id="871ac-119">Hämta IP-adress och värddatornamn namnet PI</span><span class="sxs-lookup"><span data-stu-id="871ac-119">Obtain the IP address and host name of Pi</span></span>
<span data-ttu-id="871ac-120">Öppna en kommandotolk i Windows eller en terminal i macOS eller Ubuntu och kör sedan följande kommando:</span><span class="sxs-lookup"><span data-stu-id="871ac-120">Open a command prompt in Windows or a terminal in macOS or Ubuntu, and then run the following command:</span></span>

```bash
devdisco list --eth
```

<span data-ttu-id="871ac-121">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="871ac-121">You should see an output that is similar to the following:</span></span>

![Identifiering av nätverksenheter](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

<span data-ttu-id="871ac-123">Anteckna den `IP address` och `hostname` pi.</span><span class="sxs-lookup"><span data-stu-id="871ac-123">Take note of the `IP address` and `hostname` of Pi.</span></span> <span data-ttu-id="871ac-124">Du behöver den här informationen senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="871ac-124">You need this information later in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="871ac-125">Kontrollera att Pi är ansluten till samma nätverk som datorn.</span><span class="sxs-lookup"><span data-stu-id="871ac-125">Make sure that Pi is connected to the same network as your computer.</span></span> <span data-ttu-id="871ac-126">Om datorn är ansluten till ett trådlöst nätverk när Pi är ansluten till ett kabelanslutet nätverk, kan du inte se IP-adressen i devdisco utdata.</span><span class="sxs-lookup"><span data-stu-id="871ac-126">For example, if your computer is connected to a wireless network while Pi is connected to a wired network, you might not see the IP address in the devdisco output.</span></span>

## <a name="clone-the-sample-application"></a><span data-ttu-id="871ac-127">Klona exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="871ac-127">Clone the sample application</span></span>
<span data-ttu-id="871ac-128">Följ dessa steg om du vill öppna exempelkoden:</span><span class="sxs-lookup"><span data-stu-id="871ac-128">To open the sample code, follow these steps:</span></span>

1. <span data-ttu-id="871ac-129">Klona lagringsplatsen exempel från GitHub genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="871ac-129">Clone the sample repository from GitHub by running the following command:</span></span>
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
   ```
2. <span data-ttu-id="871ac-130">Öppna exempelprogrammet i Visual Studio Code genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="871ac-130">Open the sample application in Visual Studio Code by running the following commands:</span></span>
   
   ```bash
   cd iot-hub-node-raspberrypi-getting-started
   cd Lesson1
   code .
   ```

![Lagringsplatsen struktur](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

<span data-ttu-id="871ac-132">Den `app.js` filen i den `app` undermapp är viktiga källfilen som innehåller koden för att styra Indikatorn.</span><span class="sxs-lookup"><span data-stu-id="871ac-132">The `app.js` file in the `app` subfolder is the key source file that contains the code to control the LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="871ac-133">Installera beroenden</span><span class="sxs-lookup"><span data-stu-id="871ac-133">Install application dependencies</span></span>
<span data-ttu-id="871ac-134">Installera bibliotek och andra moduler som du behöver för exempelprogrammet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="871ac-134">Install the libraries and other modules you need for the sample application by running the following command:</span></span>

```bash
npm install
```

## <a name="configure-the-device-connection"></a><span data-ttu-id="871ac-135">Konfigurera enhetsanslutning</span><span class="sxs-lookup"><span data-stu-id="871ac-135">Configure the device connection</span></span>
<span data-ttu-id="871ac-136">Följ dessa steg för att konfigurera enhetsanslutningen:</span><span class="sxs-lookup"><span data-stu-id="871ac-136">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="871ac-137">Generera konfigurationsfilen enheten genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="871ac-137">Generate the device configuration file by running the following command:</span></span>
   
   ```bash
   gulp init
   ```
   
   <span data-ttu-id="871ac-138">Konfigurationsfilen `config-raspberrypi.json` innehåller autentiseringsuppgifter för användare som du använder för att logga in till Pi.</span><span class="sxs-lookup"><span data-stu-id="871ac-138">The configuration file `config-raspberrypi.json` contains the user credentials you use to log in to Pi.</span></span> <span data-ttu-id="871ac-139">För att undvika läckage av användarautentiseringsuppgifter konfigurationsfilen genereras i undermappen `.iot-hub-getting-started` för arbetsmapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="871ac-139">To avoid the leak of user credentials, the configuration file is generated in the subfolder `.iot-hub-getting-started` of the home folder on your computer.</span></span>

2. <span data-ttu-id="871ac-140">Öppna konfigurationsfilen enheten i Visual Studio Code genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="871ac-140">Open the device configuration file in Visual Studio Code by running the following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
3. <span data-ttu-id="871ac-141">Ersätt platshållaren `[device hostname or IP address]` med IP-adressen eller värdnamnet som du har fått tidigare i ”hämta i IP-adress och värddatornamn namnet pi”.</span><span class="sxs-lookup"><span data-stu-id="871ac-141">Replace the placeholder `[device hostname or IP address]` with the IP address or the host name that you got previously in "Obtain the IP address and host name of Pi."</span></span>
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> <span data-ttu-id="871ac-143">Du kan använda SSH-nyckel i stället för användarnamn och lösenord när du ansluter till hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="871ac-143">You can use SSH key instead of user name and password when connecting to Raspberry Pi.</span></span> <span data-ttu-id="871ac-144">För att kunna göra detta måste du generera en nyckel med hjälp av **ssh-keygen** och **ssh-kopia-id pi @\<enhetsadress\>**.</span><span class="sxs-lookup"><span data-stu-id="871ac-144">In order to do this you will have to generate the key using **ssh-keygen** and **ssh-copy-id pi@\<device address\>**.</span></span>
>
> <span data-ttu-id="871ac-145">Dessa kommandon är tillgängliga i Windows **Git Bash**.</span><span class="sxs-lookup"><span data-stu-id="871ac-145">On Windows these commands are available in **Git Bash**.</span></span>
>
> <span data-ttu-id="871ac-146">Du behöver köra på MacOS **brew installera ssh-kopia-id**.</span><span class="sxs-lookup"><span data-stu-id="871ac-146">On MacOS you need to run **brew install ssh-copy-id**.</span></span>
>
> <span data-ttu-id="871ac-147">Efter att överföra nyckeln till hallon Pi, Ersätt **device_password** med **device_key_path** egenskap i **config raspberrypi.json**.</span><span class="sxs-lookup"><span data-stu-id="871ac-147">After successfully uploading the key to the Raspberry Pi, replace **device_password** with **device_key_path** property in **config-raspberrypi.json**.</span></span>
>
> <span data-ttu-id="871ac-148">Uppdaterade rader ska se ut som nedan:</span><span class="sxs-lookup"><span data-stu-id="871ac-148">Updated lines should look as below:</span></span>
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

<span data-ttu-id="871ac-149">Grattis!</span><span class="sxs-lookup"><span data-stu-id="871ac-149">Congratulations!</span></span> <span data-ttu-id="871ac-150">Du har skapat det första exempelprogrammet för Pi.</span><span class="sxs-lookup"><span data-stu-id="871ac-150">You've successfully created the first sample application for Pi.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="871ac-151">Distribuera och köra exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="871ac-151">Deploy and run the sample application</span></span>
### <a name="install-nodejs-and-npm-on-pi"></a><span data-ttu-id="871ac-152">Installera Node.js och NPM på Pi</span><span class="sxs-lookup"><span data-stu-id="871ac-152">Install Node.js and NPM on Pi</span></span>
<span data-ttu-id="871ac-153">Installera Node.js och NPM på Pi genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="871ac-153">Install Node.js and NPM on Pi by running the following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="871ac-154">Den här uppgiften kan ta 10 minuter att slutföra den första gången du kör den.</span><span class="sxs-lookup"><span data-stu-id="871ac-154">This task might take 10 minutes to complete the first time you run it.</span></span>

### <a name="deploy-and-run-the-sample-app"></a><span data-ttu-id="871ac-155">Distribuera och köra sample-appen</span><span class="sxs-lookup"><span data-stu-id="871ac-155">Deploy and run the sample app</span></span>
<span data-ttu-id="871ac-156">Distribuera och köra exempelprogrammet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="871ac-156">Deploy and run the sample application by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a><span data-ttu-id="871ac-157">Kontrollera appen fungerar</span><span class="sxs-lookup"><span data-stu-id="871ac-157">Verify the app works</span></span>
<span data-ttu-id="871ac-158">Du bör nu se Indikatorn på Pi blinkande varannan sekund.</span><span class="sxs-lookup"><span data-stu-id="871ac-158">You should now see the LED on Pi blinking every two seconds.</span></span>  <span data-ttu-id="871ac-159">Om du inte ser Indikator blinkar, finns det [felsökningsguide för](iot-hub-raspberry-pi-kit-node-troubleshooting.md) efter lösningar på vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="871ac-159">If you don’t see the LED blinking, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions to common problems.</span></span>
<span data-ttu-id="871ac-160">![Blinka Indikator](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span><span class="sxs-lookup"><span data-stu-id="871ac-160">![LED blinking](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span></span>

## <a name="summary"></a><span data-ttu-id="871ac-161">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="871ac-161">Summary</span></span>
<span data-ttu-id="871ac-162">Du har installerat de verktyg som krävs för att arbeta med Pi och distribuerat ett exempelprogram till Pi blinkar på Indikator.</span><span class="sxs-lookup"><span data-stu-id="871ac-162">You've installed the required tools to work with Pi and deployed a sample application to Pi to blink the LED.</span></span> <span data-ttu-id="871ac-163">Du kan nu skapa, distribuera och köra en annan exempelprogrammet som ansluter Pi till Azure IoT Hub skicka och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="871ac-163">You can now create, deploy, and run another sample application that connects Pi to Azure IoT Hub to send and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="871ac-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="871ac-164">Next steps</span></span>
[<span data-ttu-id="871ac-165">Hämta Azure-verktyg</span><span class="sxs-lookup"><span data-stu-id="871ac-165">Get the Azure tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

