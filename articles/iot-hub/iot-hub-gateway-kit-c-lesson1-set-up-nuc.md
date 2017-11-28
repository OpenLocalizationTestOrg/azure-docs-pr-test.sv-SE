---
title: 'SensorTag enhet & Azure IoT Gateway - lektionen 1: Konfigurera Intel NUC | Microsoft Docs'
description: "Ställ in Intel NUC toowork som en IoT-gateway mellan en sensor och en sensorinformation för Azure IoT Hub toocollect och skicka den tooIoT hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT-gateway, intel nuc nuc dator, DE3815TYKE
ms.assetid: 917090d6-35c2-495b-a620-ca6f9c02b317
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 7c3ab3b014713c7facb86b8e8622d70e60a960e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a><span data-ttu-id="dd372-104">Ställ in Intel NUC som en IoT-gateway</span><span class="sxs-lookup"><span data-stu-id="dd372-104">Set up Intel NUC as an IoT gateway</span></span>
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

## <a name="what-you-will-do"></a><span data-ttu-id="dd372-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="dd372-105">What you will do</span></span>

- <span data-ttu-id="dd372-106">Ställ in Intel NUC som en IoT-gateway.</span><span class="sxs-lookup"><span data-stu-id="dd372-106">Set up Intel NUC as an IoT gateway.</span></span>
- <span data-ttu-id="dd372-107">Installera hello Azure IoT kanten på hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="dd372-107">Install hello Azure IoT Edge package on hello Intel NUC.</span></span>
- <span data-ttu-id="dd372-108">Kör ett exempelprogram för ”hello_world” på hello Intel NUC tooverify hello gateway-funktioner.</span><span class="sxs-lookup"><span data-stu-id="dd372-108">Run a "hello_world" sample application on hello Intel NUC tooverify hello gateway functionality.</span></span>

  > <span data-ttu-id="dd372-109">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="dd372-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="dd372-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="dd372-110">What you will learn</span></span>

<span data-ttu-id="dd372-111">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="dd372-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="dd372-112">Hur tooconnect Intel NUC med kringutrustning.</span><span class="sxs-lookup"><span data-stu-id="dd372-112">How tooconnect Intel NUC with peripherals.</span></span>
- <span data-ttu-id="dd372-113">Hur hello tooinstall och uppdatera hello krävs paket på Intel NUC med Smart Package Manager.</span><span class="sxs-lookup"><span data-stu-id="dd372-113">How tooinstall and update hello required packages on Intel NUC using hello Smart Package Manager.</span></span>
- <span data-ttu-id="dd372-114">Toorun hello ”hello_world” exempel på hur programmet tooverify hello gateway-funktioner.</span><span class="sxs-lookup"><span data-stu-id="dd372-114">How toorun hello "hello_world" sample application tooverify hello gateway functionality.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="dd372-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="dd372-115">What you need</span></span>

- <span data-ttu-id="dd372-116">En Intel NUC Kit DE3815TYKE med hello Intel IoT Gateway-programsviten (vind floden Linux * 7.0.0.13) förinstallerat.</span><span class="sxs-lookup"><span data-stu-id="dd372-116">An Intel NUC Kit DE3815TYKE with hello Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) preinstalled.</span></span> <span data-ttu-id="dd372-117">[Klicka här toopurchase Groove IoT kommersiella Gateway Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).</span><span class="sxs-lookup"><span data-stu-id="dd372-117">[Click here toopurchase Grove IoT Commercial Gateway Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).</span></span>
- <span data-ttu-id="dd372-118">En Ethernet-kabel.</span><span class="sxs-lookup"><span data-stu-id="dd372-118">An Ethernet cable.</span></span>
- <span data-ttu-id="dd372-119">Ett tangentbord.</span><span class="sxs-lookup"><span data-stu-id="dd372-119">A keyboard.</span></span>
- <span data-ttu-id="dd372-120">HDMI eller VGA-kabel.</span><span class="sxs-lookup"><span data-stu-id="dd372-120">An HDMI or VGA cable.</span></span>
- <span data-ttu-id="dd372-121">En bildskärm med en HDMI eller VGA-port.</span><span class="sxs-lookup"><span data-stu-id="dd372-121">A monitor with an HDMI or VGA port.</span></span>
- <span data-ttu-id="dd372-122">Valfritt: [Texas Instruments Sensor Tag (CC2650STK)](http://www.ti.com/tool/cc2650stk)</span><span class="sxs-lookup"><span data-stu-id="dd372-122">Optional: [Texas Instruments Sensor Tag (CC2650STK)](http://www.ti.com/tool/cc2650stk)</span></span>

![Gateway-Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-hello-peripherals"></a><span data-ttu-id="dd372-124">Anslut Intel NUC med hello kringutrustning</span><span class="sxs-lookup"><span data-stu-id="dd372-124">Connect Intel NUC with hello peripherals</span></span>

<span data-ttu-id="dd372-125">hello bilden nedan är ett exempel på Intel NUC som är kopplad till olika kringutrustning:</span><span class="sxs-lookup"><span data-stu-id="dd372-125">hello image below is an example of Intel NUC that is connected with various peripherals:</span></span>

1. <span data-ttu-id="dd372-126">Anslutna tooa tangentbordet.</span><span class="sxs-lookup"><span data-stu-id="dd372-126">Connected tooa keyboard.</span></span>
2. <span data-ttu-id="dd372-127">Tooa bildskärm är ansluten till en VGA- eller HDMI kabel.</span><span class="sxs-lookup"><span data-stu-id="dd372-127">Connected tooa monitor with a VGA cable or HDMI cable.</span></span>
3. <span data-ttu-id="dd372-128">Anslutna tooa kabelanslutet nätverk med en Ethernet-kabel.</span><span class="sxs-lookup"><span data-stu-id="dd372-128">Connected tooa wired network with an Ethernet cable.</span></span>
4. <span data-ttu-id="dd372-129">Anslutna tooa strömförsörjning med en power-kabel.</span><span class="sxs-lookup"><span data-stu-id="dd372-129">Connected tooa power supply with a power cable.</span></span>

![Intel NUC anslutna tooperipherals](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-toohello-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a><span data-ttu-id="dd372-131">Ansluta toohello Intel NUC system från värddatorn via SSH (Secure Shell)</span><span class="sxs-lookup"><span data-stu-id="dd372-131">Connect toohello Intel NUC system from host computer via Secure Shell (SSH)</span></span>

<span data-ttu-id="dd372-132">Du behöver ett tangentbord och en bildskärm tooget hello IP-adress av Intel NUC enheten.</span><span class="sxs-lookup"><span data-stu-id="dd372-132">You will need a keyboard and a monitor tooget hello IP address of your Intel NUC device.</span></span> <span data-ttu-id="dd372-133">Om du redan vet hello IP-adress, kan du hoppa över ahead toostep 3 i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="dd372-133">If you already know hello IP address, you can skip ahead toostep 3 in this section.</span></span>

1. <span data-ttu-id="dd372-134">Aktivera hello Intel NUC genom att trycka på strömknappen hello och logga sedan in.</span><span class="sxs-lookup"><span data-stu-id="dd372-134">Turn on hello Intel NUC by pressing hello power button and then log in.</span></span>

   <span data-ttu-id="dd372-135">hello standardanvändarnamn och lösenord är båda `root`.</span><span class="sxs-lookup"><span data-stu-id="dd372-135">hello default user name and password are both `root`.</span></span>

       > Hit hello enter key on your keyboard if you see either of hello following errors when you boot: 'A TPM error (7) occurred attempting tooread a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. <span data-ttu-id="dd372-136">Hämta hello IP-adressen för hello Intel NUC genom att köra hello `ifconfig` på hello Intel NUC enhet.</span><span class="sxs-lookup"><span data-stu-id="dd372-136">Get hello IP address of hello Intel NUC by running hello `ifconfig` command on hello Intel NUC device.</span></span>

   <span data-ttu-id="dd372-137">Här är ett exempel på utdata från kommandot hello.</span><span class="sxs-lookup"><span data-stu-id="dd372-137">Here is an example of hello command output.</span></span>

   ![ifconfig utdata visar Intel NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   <span data-ttu-id="dd372-139">I det här exemplet hello värdet som följer `inet addr:` hello IP-adress som du behöver när ansluter toohello Intel NUC från en värddator.</span><span class="sxs-lookup"><span data-stu-id="dd372-139">In this example, hello value that follows `inet addr:` is hello IP address that you need when connect toohello Intel NUC from a host computer.</span></span>

3. <span data-ttu-id="dd372-140">Använd någon av följande SSH-klienter från din värd datorn tooconnect tooIntel NUC hello.</span><span class="sxs-lookup"><span data-stu-id="dd372-140">Use one of hello following SSH clients from your host computer tooconnect tooIntel NUC.</span></span>

    - <span data-ttu-id="dd372-141">[PuTTY](http://www.putty.org/) för Windows.</span><span class="sxs-lookup"><span data-stu-id="dd372-141">[PuTTY](http://www.putty.org/) for Windows.</span></span>
    - <span data-ttu-id="dd372-142">hello inbyggda SSH-klienten på Ubuntu eller macOS.</span><span class="sxs-lookup"><span data-stu-id="dd372-142">hello built-in SSH client on Ubuntu or macOS.</span></span>

   <span data-ttu-id="dd372-143">Det är mer effektivt och effektiv toooperate en Intel NUC från en värddator.</span><span class="sxs-lookup"><span data-stu-id="dd372-143">It is more efficient and productive toooperate an Intel NUC from a host computer.</span></span> <span data-ttu-id="dd372-144">Intel NUC IP-adress, kommer måste hello tooit för användare och lösenord tooconnect via en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="dd372-144">You'll need hello Intel NUC's IP address, user name and password tooconnect tooit via an SSH client.</span></span> <span data-ttu-id="dd372-145">Här är ett exempel som använder en SSH-klient på macOS.</span><span class="sxs-lookup"><span data-stu-id="dd372-145">Here is an example that uses an SSH client on macOS.</span></span>
   <span data-ttu-id="dd372-146">![SSH-klienten körs på macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span><span class="sxs-lookup"><span data-stu-id="dd372-146">![SSH client running on macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span></span>

## <a name="install-hello-azure-iot-edge-package"></a><span data-ttu-id="dd372-147">Installera hello Azure IoT kant paketet</span><span class="sxs-lookup"><span data-stu-id="dd372-147">Install hello Azure IoT Edge package</span></span>

<span data-ttu-id="dd372-148">hello Azure IoT kant paketet innehåller hello före kompilerade binärfilerna för IoT kant och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="dd372-148">hello Azure IoT Edge package contains hello pre-compiled binaries of IoT Edge and its dependencies.</span></span> <span data-ttu-id="dd372-149">Dessa binärfiler är Azure IoT kant, hello Azure IoT-SDK och hello motsvarande verktyg.</span><span class="sxs-lookup"><span data-stu-id="dd372-149">These binaries are Azure IoT Edge, hello Azure IoT SDK and hello corresponding tools.</span></span> <span data-ttu-id="dd372-150">hello paketet innehåller också en ”hello_world” exempelprogrammet är används toovalidate hello gateway-funktioner.</span><span class="sxs-lookup"><span data-stu-id="dd372-150">hello package also contains a "hello_world" sample application is used toovalidate hello gateway functionality.</span></span> <span data-ttu-id="dd372-151">IoT-Edge tillhör hello core hello gateway.</span><span class="sxs-lookup"><span data-stu-id="dd372-151">IoT Edge is hello core part of hello gateway.</span></span> 

<span data-ttu-id="dd372-152">Följ dessa steg tooinstall hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="dd372-152">Follow these steps tooinstall hello package.</span></span>

1. <span data-ttu-id="dd372-153">Lägg till hello IoT moln databasen genom att köra följande kommandon i ett terminalfönster hello:</span><span class="sxs-lookup"><span data-stu-id="dd372-153">Add hello IoT Cloud repository by running hello following commands in a terminal window:</span></span>

   ```bash
   rpm --import https://iotdk.intel.com/misc/iot_pub2.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
   ```

   > <span data-ttu-id="dd372-154">Ange ”y” när du uppmanas du too'Include den här kanalen ”?</span><span class="sxs-lookup"><span data-stu-id="dd372-154">Enter 'y', when it prompts you too'Include this channel?'</span></span>
   
   <span data-ttu-id="dd372-155">Om du får ett `import read failed(-1)` fel, Använd hello följande kommandon tooresolve hello problemet:</span><span class="sxs-lookup"><span data-stu-id="dd372-155">If you receive an `import read failed(-1)` error, use hello following commands tooresolve hello issue:</span></span>
   ```bash
   wget http://iotdk.intel.com/misc/iot_pub2.key 
   rpm --import iot_pub2.key  
   ```

   <span data-ttu-id="dd372-156">Hej `rpm` kommandot import hello rpm-nyckel.</span><span class="sxs-lookup"><span data-stu-id="dd372-156">hello `rpm` command imports hello rpm key.</span></span> <span data-ttu-id="dd372-157">Hej `smart channel` kommando lägger till hello rpm kanaler toohello Smart Package Manager.</span><span class="sxs-lookup"><span data-stu-id="dd372-157">hello `smart channel` command adds hello rpm channel toohello Smart Package Manager.</span></span> <span data-ttu-id="dd372-158">Innan du kör hello `smart update` kommandot visas utdata som nedan.</span><span class="sxs-lookup"><span data-stu-id="dd372-158">Before you run hello `smart update` command, you will see an output like below.</span></span>

   ![rpm och smarta kanal kommandon utdata](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. <span data-ttu-id="dd372-160">Kör hello smart update-kommandot:</span><span class="sxs-lookup"><span data-stu-id="dd372-160">Execute hello smart update command:</span></span>

   ```bash
   smart update
   ```

3. <span data-ttu-id="dd372-161">Installera hello Azure IoT Gateway-paketet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="dd372-161">Install hello Azure IoT Gateway package by running hello following command:</span></span>

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   <span data-ttu-id="dd372-162">`packagegroup-cloud-azure`är hello namn hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="dd372-162">`packagegroup-cloud-azure` is hello name of hello package.</span></span> <span data-ttu-id="dd372-163">Hej `smart install` kommandot är används tooinstall hello paketet.</span><span class="sxs-lookup"><span data-stu-id="dd372-163">hello `smart install` command is used tooinstall hello package.</span></span>

    > <span data-ttu-id="dd372-164">Hello kör följande kommando om du ser felet: 'offentlig nyckel som är inte tillgänglig ”</span><span class="sxs-lookup"><span data-stu-id="dd372-164">Run hello following command if you see this error: 'public key not available'</span></span>

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
    > <span data-ttu-id="dd372-165">Starta om hello Intel NUC om du ser felet: 'inga erbjuder util-linux-dev'</span><span class="sxs-lookup"><span data-stu-id="dd372-165">Reboot hello Intel NUC if you see this error: 'no package provides util-linux-dev'</span></span>

   <span data-ttu-id="dd372-166">När hello-paket installeras är Intel NUC klar toofunction som en gateway.</span><span class="sxs-lookup"><span data-stu-id="dd372-166">After hello package is installed, Intel NUC is ready toofunction as a gateway.</span></span>

## <a name="run-hello-azure-iot-edge-helloworld-sample-application"></a><span data-ttu-id="dd372-167">Köra hello Azure IoT kant ”hello_world” exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="dd372-167">Run hello Azure IoT Edge "hello_world" sample application</span></span>

<span data-ttu-id="dd372-168">hello följande exempelprogrammet skapar en gateway från en `hello_world.json` filen och använder hello grundläggande komponenter för Azure IoT kant arkitektur toolog en hello world tooa meddelandefil (log.txt) var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="dd372-168">hello following sample application creates a gateway from a `hello_world.json` file and uses hello fundamental components of Azure IoT Edge architecture toolog a hello world message tooa file (log.txt) every 5 seconds.</span></span>

<span data-ttu-id="dd372-169">Du kan köra hello Hello World-exempel genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="dd372-169">You can run hello Hello World sample by executing hello following commands:</span></span>

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

<span data-ttu-id="dd372-170">Låt Hello World hello programmet kör några minuter och sedan trycker hello RETUR viktiga toostop den.</span><span class="sxs-lookup"><span data-stu-id="dd372-170">Let hello Hello World application run for a few minutes and then hit hello Enter key toostop it.</span></span>
<span data-ttu-id="dd372-171">![programutdata](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)</span><span class="sxs-lookup"><span data-stu-id="dd372-171">![application output](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)</span></span>

> <span data-ttu-id="dd372-172">Du kan ignorera eventuella: Ogiltigt argument handle(NULL)'-fel som visas när du klickar på RETUR.</span><span class="sxs-lookup"><span data-stu-id="dd372-172">You can ignore any 'invalid argument handle(NULL)' errors that appear after you hit Enter.</span></span>

<span data-ttu-id="dd372-173">Du kan verifiera att hello-gateway har körts genom att öppna hello log.txt som är nu i mappen hello_world ![log.txt directory vy](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)</span><span class="sxs-lookup"><span data-stu-id="dd372-173">You can verify that hello gateway ran successfully by opening hello log.txt file that is now in your hello_world folder ![log.txt directory view](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)</span></span>

<span data-ttu-id="dd372-174">Öppna log.txt med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="dd372-174">Open log.txt using hello following command:</span></span>

```bash
vim log.txt
```

<span data-ttu-id="dd372-175">Sedan visas hello innehållet i log.txt som ska vara en JSON-formaterade utdata från hello loggningsmeddelanden som skrivits var femte sekund av Hello World hello gateway-modulen.</span><span class="sxs-lookup"><span data-stu-id="dd372-175">You will then see hello contents of log.txt, which will be a JSON formatted output of hello logging messages that were written every 5 seconds by hello gateway Hello World module.</span></span>
<span data-ttu-id="dd372-176">![log.txt directory vy](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)</span><span class="sxs-lookup"><span data-stu-id="dd372-176">![log.txt directory view](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)</span></span>

<span data-ttu-id="dd372-177">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="dd372-177">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="summary"></a><span data-ttu-id="dd372-178">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="dd372-178">Summary</span></span>

<span data-ttu-id="dd372-179">Grattis!</span><span class="sxs-lookup"><span data-stu-id="dd372-179">Congratulations!</span></span> <span data-ttu-id="dd372-180">Du har slutfört konfigurationen Intel NUC som en gateway.</span><span class="sxs-lookup"><span data-stu-id="dd372-180">You've finished setting up Intel NUC as a gateway.</span></span> <span data-ttu-id="dd372-181">Nu är du redo toomove på toohello nästa lektionen tooset in värddatorn, skapa en Azure IoT-hubb och registrera din logiska Azure IoT Hub-enhet.</span><span class="sxs-lookup"><span data-stu-id="dd372-181">Now you're ready toomove on toohello next lesson tooset up your host computer, create an Azure IoT Hub and register your Azure IoT Hub logical device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd372-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd372-182">Next steps</span></span>
[<span data-ttu-id="dd372-183">Använda en enhet tooAzure IoT-hubb för tooconnect en IoT-gateway</span><span class="sxs-lookup"><span data-stu-id="dd372-183">Use an IoT gateway tooconnect a device tooAzure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

