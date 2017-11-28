---
title: 'Simulerade enhet & Azure IoT Gateway - lektionen 1: Konfigurera NUC | Microsoft Docs'
description: "Ställ in Intel NUC toowork som en IoT-gateway mellan en sensor och en sensorinformation för Azure IoT Hub toocollect och skicka den tooIoT hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: yjianfeng
tags: 
keywords: IOT-gateway, intel nuc nuc dator, DE3815TYKE
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: f41d6b2e-9b00-40df-90eb-17d824bea883
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c2146c7cd8ca54adbd0af279364ec8484be1b45b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a><span data-ttu-id="39569-104">Ställ in Intel NUC som en IoT-gateway</span><span class="sxs-lookup"><span data-stu-id="39569-104">Set up Intel NUC as an IoT gateway</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="39569-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="39569-105">What you will do</span></span>

- <span data-ttu-id="39569-106">Ställ in Intel NUC som en IoT-gateway.</span><span class="sxs-lookup"><span data-stu-id="39569-106">Set up Intel NUC as an IoT gateway.</span></span>
- <span data-ttu-id="39569-107">Installera hello Azure IoT kanten på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="39569-107">Install hello Azure IoT Edge package on Intel NUC.</span></span>
- <span data-ttu-id="39569-108">Kör ett exempelprogram för ”hello_world” på Intel NUC tooverify hello gateway-funktioner.</span><span class="sxs-lookup"><span data-stu-id="39569-108">Run a "hello_world" sample application on Intel NUC tooverify hello gateway functionality.</span></span>
<span data-ttu-id="39569-109">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="39569-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="39569-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="39569-110">What you will learn</span></span>

<span data-ttu-id="39569-111">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="39569-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="39569-112">Hur tooconnect Intel NUC med kringutrustning.</span><span class="sxs-lookup"><span data-stu-id="39569-112">How tooconnect Intel NUC with peripherals.</span></span>
- <span data-ttu-id="39569-113">Hur hello tooinstall och uppdatera hello krävs paket på Intel NUC med Smart Package Manager.</span><span class="sxs-lookup"><span data-stu-id="39569-113">How tooinstall and update hello required packages on Intel NUC using hello Smart Package Manager.</span></span>
- <span data-ttu-id="39569-114">Toorun hello ”hello_world” exempel på hur programmet tooverify hello gateway-funktioner.</span><span class="sxs-lookup"><span data-stu-id="39569-114">How toorun hello "hello_world" sample application tooverify hello gateway functionality.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="39569-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="39569-115">What you need</span></span>

- <span data-ttu-id="39569-116">En Intel NUC Kit DE3815TYKE med hello Intel IoT Gateway-programsviten (vind floden Linux * 7.0.0.13) förinstallerat.</span><span class="sxs-lookup"><span data-stu-id="39569-116">An Intel NUC Kit DE3815TYKE with hello Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) preinstalled.</span></span>
- <span data-ttu-id="39569-117">En Ethernet-kabel.</span><span class="sxs-lookup"><span data-stu-id="39569-117">An Ethernet cable.</span></span>
- <span data-ttu-id="39569-118">Ett tangentbord.</span><span class="sxs-lookup"><span data-stu-id="39569-118">A keyboard.</span></span>
- <span data-ttu-id="39569-119">HDMI eller VGA-kabel.</span><span class="sxs-lookup"><span data-stu-id="39569-119">An HDMI or VGA cable.</span></span>
- <span data-ttu-id="39569-120">En bildskärm med en HDMI eller VGA-port.</span><span class="sxs-lookup"><span data-stu-id="39569-120">A monitor with an HDMI or VGA port.</span></span>

![Gateway-Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit_without_sensortag.png)

## <a name="connect-intel-nuc-with-hello-peripherals"></a><span data-ttu-id="39569-122">Anslut Intel NUC med hello kringutrustning</span><span class="sxs-lookup"><span data-stu-id="39569-122">Connect Intel NUC with hello peripherals</span></span>

<span data-ttu-id="39569-123">hello bilden nedan är ett exempel på Intel NUC som är kopplad till olika kringutrustning:</span><span class="sxs-lookup"><span data-stu-id="39569-123">hello image below is an example of Intel NUC that is connected with various peripherals:</span></span>

1. <span data-ttu-id="39569-124">Anslutna tooa tangentbordet.</span><span class="sxs-lookup"><span data-stu-id="39569-124">Connected tooa keyboard.</span></span>
2. <span data-ttu-id="39569-125">Ansluten toohello Övervakare av en VGA- eller HDMI kabel.</span><span class="sxs-lookup"><span data-stu-id="39569-125">Connected toohello monitor by a VGA cable or HDMI cable.</span></span>
3. <span data-ttu-id="39569-126">Anslutna tooa kabelanslutna nätverket av en Ethernet-kabel.</span><span class="sxs-lookup"><span data-stu-id="39569-126">Connected tooa wired network by an Ethernet cable.</span></span>
4. <span data-ttu-id="39569-127">Anslutna toohello strömförsörjning med en power-kabel.</span><span class="sxs-lookup"><span data-stu-id="39569-127">Connected toohello power supply with a power cable.</span></span>

![Intel NUC anslutna tooperipherals](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-toohello-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a><span data-ttu-id="39569-129">Ansluta toohello Intel NUC system från värddatorn via SSH (Secure Shell)</span><span class="sxs-lookup"><span data-stu-id="39569-129">Connect toohello Intel NUC system from host computer via Secure Shell (SSH)</span></span>

<span data-ttu-id="39569-130">Du måste här tangentbord och bildskärm tooget hello IP-adressen till enheten NUC.</span><span class="sxs-lookup"><span data-stu-id="39569-130">Here you need keyboard and monitor tooget hello IP address of your NUC device.</span></span> <span data-ttu-id="39569-131">Om du redan vet hello IP-adress, kan du hoppa över toostep 3 i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="39569-131">If you already know hello IP address, you can skip toostep 3 in this section.</span></span>

1. <span data-ttu-id="39569-132">Aktivera Intel NUC genom att trycka på strömknappen hello och loggfiler i hello system.</span><span class="sxs-lookup"><span data-stu-id="39569-132">Turn on Intel NUC by pressing hello Power button and log in hello system.</span></span>

   <span data-ttu-id="39569-133">hello standardanvändarnamn och lösenord är båda `root`.</span><span class="sxs-lookup"><span data-stu-id="39569-133">hello default user name and password are both `root`.</span></span>

2. <span data-ttu-id="39569-134">Hämta hello IP-adressen för NUC genom att köra hello `ifconfig` kommando.</span><span class="sxs-lookup"><span data-stu-id="39569-134">Get hello IP address of NUC by running hello `ifconfig` command.</span></span> <span data-ttu-id="39569-135">Detta görs på hello NUC enhet.</span><span class="sxs-lookup"><span data-stu-id="39569-135">This step is done on hello NUC device.</span></span>

   <span data-ttu-id="39569-136">Här är ett exempel på utdata från kommandot hello.</span><span class="sxs-lookup"><span data-stu-id="39569-136">Here is an example of hello command output.</span></span>

   ![ifconfig utdata visar NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   <span data-ttu-id="39569-138">I det här exemplet hello värdet som följer `inet addr:` hello IP-adress som du behöver när du planerar tooconnect via en fjärranslutning från en värd datorn tooIntel NUC.</span><span class="sxs-lookup"><span data-stu-id="39569-138">In this example, hello value that follows `inet addr:` is hello IP address that you need when you plan tooconnect remotely from a host computer tooIntel NUC.</span></span>

3. <span data-ttu-id="39569-139">Använd någon av följande SSH-klienter från din värd datorn tooconnect tooIntel NUC hello.</span><span class="sxs-lookup"><span data-stu-id="39569-139">Use one of hello following SSH clients from your host machine tooconnect tooIntel NUC.</span></span>

   - <span data-ttu-id="39569-140">[PuTTY](http://www.putty.org/) för Windows.</span><span class="sxs-lookup"><span data-stu-id="39569-140">[PuTTY](http://www.putty.org/) for Windows.</span></span>
   - <span data-ttu-id="39569-141">hello inbyggda SSH-klienten på Ubuntu eller macOS.</span><span class="sxs-lookup"><span data-stu-id="39569-141">hello build-in SSH client on Ubuntu or macOS.</span></span>

   <span data-ttu-id="39569-142">Det är mer effektivt och effektiv toooperate på Intel NUC från en värddator.</span><span class="sxs-lookup"><span data-stu-id="39569-142">It is more efficient and productive toooperate on Intel NUC from a host computer.</span></span> <span data-ttu-id="39569-143">Du behöver hello hello IP-adress, användarnamn och lösenord tooconnect hello NUC via SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="39569-143">You need hello hello IP address, user name and password tooconnect hello NUC via SSH client.</span></span> <span data-ttu-id="39569-144">Här är hello exempel Använd SSH-klienten på macOS.</span><span class="sxs-lookup"><span data-stu-id="39569-144">Here is hello example use SSH client on macOS.</span></span>
   <span data-ttu-id="39569-145">![SSH-klienten körs på macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span><span class="sxs-lookup"><span data-stu-id="39569-145">![SSH client running on macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span></span>

## <a name="install-hello-azure-iot-edge-package"></a><span data-ttu-id="39569-146">Installera hello Azure IoT kant paketet</span><span class="sxs-lookup"><span data-stu-id="39569-146">Install hello Azure IoT Edge package</span></span>

<span data-ttu-id="39569-147">hello Azure IoT kant paketet innehåller hello före kompilerade binärfilerna för hello SDK och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="39569-147">hello Azure IoT Edge package contains hello pre-compiled binaries of hello SDK and its dependencies.</span></span> <span data-ttu-id="39569-148">Dessa binärfiler är Azure IoT kant, hello Azure IoT-SDK och hello motsvarande verktyg.</span><span class="sxs-lookup"><span data-stu-id="39569-148">These binaries are Azure IoT Edge, hello Azure IoT SDK and hello corresponding tools.</span></span> <span data-ttu-id="39569-149">hello-paketet innehåller också ett exempelprogram för ”hello_world” som har använt toovalidate hello gateway-funktioner.</span><span class="sxs-lookup"><span data-stu-id="39569-149">hello package also contains a "hello_world" sample application that is used toovalidate hello gateway functionality.</span></span> <span data-ttu-id="39569-150">IoT-Edge tillhör hello core hello gateway.</span><span class="sxs-lookup"><span data-stu-id="39569-150">IoT Edge is hello core part of hello gateway.</span></span> <span data-ttu-id="39569-151">tooinstall hello paketet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="39569-151">tooinstall hello package, follow these steps:</span></span>

1. <span data-ttu-id="39569-152">Lägg till hello IoT moln databasen genom att köra följande kommandon i ett terminalfönster hello:</span><span class="sxs-lookup"><span data-stu-id="39569-152">Add hello IoT cloud repository by running hello following commands in a terminal window:</span></span>

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   <span data-ttu-id="39569-153">Hej `rpm` kommandot import hello rpm-nyckel.</span><span class="sxs-lookup"><span data-stu-id="39569-153">hello `rpm` command imports hello rpm key.</span></span> <span data-ttu-id="39569-154">Hej `smart channel` kommando lägger till hello rpm kanaler toohello Smart Package Manager.</span><span class="sxs-lookup"><span data-stu-id="39569-154">hello `smart channel` command adds hello rpm channel toohello Smart Package Manager.</span></span> <span data-ttu-id="39569-155">Innan du kör hello `smart update` kommando, se utdata som liknar nedan.</span><span class="sxs-lookup"><span data-stu-id="39569-155">Before you run hello `smart update` command, you see an output like below.</span></span>

   ![rpm och smarta kanal kommandon utdata](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

   ```bash
   smart update
   ```

2. <span data-ttu-id="39569-157">Installera hello paketet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="39569-157">Install hello package by running hello following command:</span></span>

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   <span data-ttu-id="39569-158">`packagegroup-cloud-azure`är hello namn hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="39569-158">`packagegroup-cloud-azure` is hello name of hello package.</span></span> <span data-ttu-id="39569-159">Hej `smart install` kommandot är används tooinstall hello paketet.</span><span class="sxs-lookup"><span data-stu-id="39569-159">hello `smart install` command is used tooinstall hello package.</span></span>

   <span data-ttu-id="39569-160">När hello-paket installeras är Intel NUC förväntade toowork som en gateway.</span><span class="sxs-lookup"><span data-stu-id="39569-160">After hello package is installed, Intel NUC is expected toowork as a gateway.</span></span>

## <a name="run-hello-azure-iot-edge-helloworld-sample-application"></a><span data-ttu-id="39569-161">Köra hello Azure IoT kant ”hello_world” exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="39569-161">Run hello Azure IoT Edge "hello_world" sample application</span></span>

<span data-ttu-id="39569-162">Gå för`azureiotgatewaysdk/samples` och köra hello exempelprogrammet ”hello_world” exempel.</span><span class="sxs-lookup"><span data-stu-id="39569-162">Go too`azureiotgatewaysdk/samples` and run hello sample "hello_world" sample application.</span></span> <span data-ttu-id="39569-163">Det här exempelprogrammet skapar en gateway från hello `hello_world.json` filen och använder hello grundläggande komponenter för hello Azure IoT kant arkitektur toolog en hello world tooa meddelandefilen 5 sekunder.</span><span class="sxs-lookup"><span data-stu-id="39569-163">This sample application creates a gateway from hello `hello_world.json` file and uses hello fundamental components of hello Azure IoT Edge architecture toolog a hello world message tooa file every 5 seconds.</span></span>

<span data-ttu-id="39569-164">Du kan köra hello exempelprogrammet ”hello_world” exempel genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="39569-164">You can run hello sample "hello_world" sample application by running hello following command:</span></span>

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

<span data-ttu-id="39569-165">hello exempelprogrammet ger hello följande utdata om hello gateway fungerar korrekt:</span><span class="sxs-lookup"><span data-stu-id="39569-165">hello sample application produces hello following output if hello gateway functionality is working correctly:</span></span>

![programutdata](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

<span data-ttu-id="39569-167">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="39569-167">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="summary"></a><span data-ttu-id="39569-168">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="39569-168">Summary</span></span>

<span data-ttu-id="39569-169">Grattis!</span><span class="sxs-lookup"><span data-stu-id="39569-169">Congratulations!</span></span> <span data-ttu-id="39569-170">Du har slutfört konfigurationen Intel NUC som en gateway.</span><span class="sxs-lookup"><span data-stu-id="39569-170">You've finished setting up Intel NUC as a gateway.</span></span> <span data-ttu-id="39569-171">Nu är du redo toomove på toohello nästa lektionen tooset datorn värden skapar en Azure IoT-hubb och registrera dina Azure IoT hub-logisk enhet.</span><span class="sxs-lookup"><span data-stu-id="39569-171">Now you're ready toomove on toohello next lesson tooset up your host computer, create an Azure IoT hub and register your Azure IoT hub logical device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="39569-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="39569-172">Next steps</span></span>
[<span data-ttu-id="39569-173">Förbereda din värddatorn och Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="39569-173">Get your host computer and Azure IoT hub ready</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
