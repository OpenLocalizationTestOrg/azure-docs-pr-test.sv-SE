---
title: 'Simulerade enhet & Azure IoT Gateway - lektionen 1: Konfigurera NUC | Microsoft Docs'
description: "Ställ in Intel NUC ska fungera som en IoT-gateway mellan en sensor och Azure IoT-hubb för att samla in sensorinformation och skicka den till IoT-hubb."
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
ms.openlocfilehash: b87974be9570f7d03fe84ae0a1d1fa7e346ff189
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a><span data-ttu-id="49f1d-104">Ställ in Intel NUC som en IoT-gateway</span><span class="sxs-lookup"><span data-stu-id="49f1d-104">Set up Intel NUC as an IoT gateway</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="49f1d-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="49f1d-105">What you will do</span></span>

- <span data-ttu-id="49f1d-106">Ställ in Intel NUC som en IoT-gateway.</span><span class="sxs-lookup"><span data-stu-id="49f1d-106">Set up Intel NUC as an IoT gateway.</span></span>
- <span data-ttu-id="49f1d-107">Installera Azure IoT kanten på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="49f1d-107">Install the Azure IoT Edge package on Intel NUC.</span></span>
- <span data-ttu-id="49f1d-108">Kör ett exempelprogram för ”hello_world” på Intel NUC att verifiera gateway-funktionerna.</span><span class="sxs-lookup"><span data-stu-id="49f1d-108">Run a "hello_world" sample application on Intel NUC to verify the gateway functionality.</span></span>
<span data-ttu-id="49f1d-109">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="49f1d-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="49f1d-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="49f1d-110">What you will learn</span></span>

<span data-ttu-id="49f1d-111">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="49f1d-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="49f1d-112">Så här ansluter du Intel NUC med kringutrustning.</span><span class="sxs-lookup"><span data-stu-id="49f1d-112">How to connect Intel NUC with peripherals.</span></span>
- <span data-ttu-id="49f1d-113">Hur du installerar och uppdatera de nödvändiga paketen på Intel NUC med Smart Package Manager.</span><span class="sxs-lookup"><span data-stu-id="49f1d-113">How to install and update the required packages on Intel NUC using the Smart Package Manager.</span></span>
- <span data-ttu-id="49f1d-114">Så här kör exempelprogrammet ”hello_world” för att verifiera gateway-funktionerna.</span><span class="sxs-lookup"><span data-stu-id="49f1d-114">How to run the "hello_world" sample application to verify the gateway functionality.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="49f1d-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="49f1d-115">What you need</span></span>

- <span data-ttu-id="49f1d-116">En Intel NUC Kit DE3815TYKE med Intel IoT Gateway-programsviten (vind floden Linux * 7.0.0.13) förinstallerat.</span><span class="sxs-lookup"><span data-stu-id="49f1d-116">An Intel NUC Kit DE3815TYKE with the Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) preinstalled.</span></span>
- <span data-ttu-id="49f1d-117">En Ethernet-kabel.</span><span class="sxs-lookup"><span data-stu-id="49f1d-117">An Ethernet cable.</span></span>
- <span data-ttu-id="49f1d-118">Ett tangentbord.</span><span class="sxs-lookup"><span data-stu-id="49f1d-118">A keyboard.</span></span>
- <span data-ttu-id="49f1d-119">HDMI eller VGA-kabel.</span><span class="sxs-lookup"><span data-stu-id="49f1d-119">An HDMI or VGA cable.</span></span>
- <span data-ttu-id="49f1d-120">En bildskärm med en HDMI eller VGA-port.</span><span class="sxs-lookup"><span data-stu-id="49f1d-120">A monitor with an HDMI or VGA port.</span></span>

![Gateway-Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit_without_sensortag.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a><span data-ttu-id="49f1d-122">Anslut Intel NUC med kringutrustning</span><span class="sxs-lookup"><span data-stu-id="49f1d-122">Connect Intel NUC with the peripherals</span></span>

<span data-ttu-id="49f1d-123">På bilden nedan är ett exempel på Intel NUC som är kopplad till olika kringutrustning:</span><span class="sxs-lookup"><span data-stu-id="49f1d-123">The image below is an example of Intel NUC that is connected with various peripherals:</span></span>

1. <span data-ttu-id="49f1d-124">Ansluten till ett tangentbord.</span><span class="sxs-lookup"><span data-stu-id="49f1d-124">Connected to a keyboard.</span></span>
2. <span data-ttu-id="49f1d-125">Anslutna till övervakaren med en VGA- eller HDMI kabel.</span><span class="sxs-lookup"><span data-stu-id="49f1d-125">Connected to the monitor by a VGA cable or HDMI cable.</span></span>
3. <span data-ttu-id="49f1d-126">Anslutna till ett kabelanslutet nätverk med en Ethernet-kabel.</span><span class="sxs-lookup"><span data-stu-id="49f1d-126">Connected to a wired network by an Ethernet cable.</span></span>
4. <span data-ttu-id="49f1d-127">Ansluten till strömförsörjning med en power-kabel.</span><span class="sxs-lookup"><span data-stu-id="49f1d-127">Connected to the power supply with a power cable.</span></span>

![Intel NUC ansluten till annan kringutrustning](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a><span data-ttu-id="49f1d-129">Ansluta till Intel NUC system från värddatorn via SSH (Secure Shell)</span><span class="sxs-lookup"><span data-stu-id="49f1d-129">Connect to the Intel NUC system from host computer via Secure Shell (SSH)</span></span>

<span data-ttu-id="49f1d-130">Du måste här tangentbord och bildskärm för att hämta IP-adressen till enheten NUC.</span><span class="sxs-lookup"><span data-stu-id="49f1d-130">Here you need keyboard and monitor to get the IP address of your NUC device.</span></span> <span data-ttu-id="49f1d-131">Om du redan vet IP-adress kan du gå vidare till steg 3 i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="49f1d-131">If you already know the IP address, you can skip to step 3 in this section.</span></span>

1. <span data-ttu-id="49f1d-132">Aktivera Intel NUC genom att trycka på strömknappen och logga in i systemet.</span><span class="sxs-lookup"><span data-stu-id="49f1d-132">Turn on Intel NUC by pressing the Power button and log in the system.</span></span>

   <span data-ttu-id="49f1d-133">Standard-användarnamn och lösenord är båda `root`.</span><span class="sxs-lookup"><span data-stu-id="49f1d-133">The default user name and password are both `root`.</span></span>

2. <span data-ttu-id="49f1d-134">Hämta IP-adressen för NUC genom att köra den `ifconfig` kommando.</span><span class="sxs-lookup"><span data-stu-id="49f1d-134">Get the IP address of NUC by running the `ifconfig` command.</span></span> <span data-ttu-id="49f1d-135">Detta görs på NUC enheten.</span><span class="sxs-lookup"><span data-stu-id="49f1d-135">This step is done on the NUC device.</span></span>

   <span data-ttu-id="49f1d-136">Här är ett exempel på utdata från.</span><span class="sxs-lookup"><span data-stu-id="49f1d-136">Here is an example of the command output.</span></span>

   ![ifconfig utdata visar NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   <span data-ttu-id="49f1d-138">I detta exempel värdet som följer `inet addr:` är IP-adressen som du behöver när du planerar att fjärransluta till Intel NUC från en värddator.</span><span class="sxs-lookup"><span data-stu-id="49f1d-138">In this example, the value that follows `inet addr:` is the IP address that you need when you plan to connect remotely from a host computer to Intel NUC.</span></span>

3. <span data-ttu-id="49f1d-139">Med någon av följande SSH-klienter från värddatorn för att ansluta till Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="49f1d-139">Use one of the following SSH clients from your host machine to connect to Intel NUC.</span></span>

   - <span data-ttu-id="49f1d-140">[PuTTY](http://www.putty.org/) för Windows.</span><span class="sxs-lookup"><span data-stu-id="49f1d-140">[PuTTY](http://www.putty.org/) for Windows.</span></span>
   - <span data-ttu-id="49f1d-141">Inbyggda SSH-klienten på Ubuntu eller macOS.</span><span class="sxs-lookup"><span data-stu-id="49f1d-141">The build-in SSH client on Ubuntu or macOS.</span></span>

   <span data-ttu-id="49f1d-142">Det är mer effektivt och effektiv ska fungera på Intel NUC från en värddator.</span><span class="sxs-lookup"><span data-stu-id="49f1d-142">It is more efficient and productive to operate on Intel NUC from a host computer.</span></span> <span data-ttu-id="49f1d-143">Du behöver den IP-adress, användarnamn och lösenord för att ansluta NUC via SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="49f1d-143">You need the the IP address, user name and password to connect the NUC via SSH client.</span></span> <span data-ttu-id="49f1d-144">Här är exempel Använd SSH-klienten på macOS.</span><span class="sxs-lookup"><span data-stu-id="49f1d-144">Here is the example use SSH client on macOS.</span></span>
   <span data-ttu-id="49f1d-145">![SSH-klienten körs på macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span><span class="sxs-lookup"><span data-stu-id="49f1d-145">![SSH client running on macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span></span>

## <a name="install-the-azure-iot-edge-package"></a><span data-ttu-id="49f1d-146">Installera Azure IoT kant-paket</span><span class="sxs-lookup"><span data-stu-id="49f1d-146">Install the Azure IoT Edge package</span></span>

<span data-ttu-id="49f1d-147">Azure IoT kant-paketet innehåller före kompilerade binärfilerna för SDK och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="49f1d-147">The Azure IoT Edge package contains the pre-compiled binaries of the SDK and its dependencies.</span></span> <span data-ttu-id="49f1d-148">Dessa binärfiler är Azure IoT kant, Azure IoT-SDK och motsvarande verktyg.</span><span class="sxs-lookup"><span data-stu-id="49f1d-148">These binaries are Azure IoT Edge, the Azure IoT SDK and the corresponding tools.</span></span> <span data-ttu-id="49f1d-149">Paketet innehåller också ett ”hello_world” exempelprogram som används för att verifiera gateway-funktionerna.</span><span class="sxs-lookup"><span data-stu-id="49f1d-149">The package also contains a "hello_world" sample application that is used to validate the gateway functionality.</span></span> <span data-ttu-id="49f1d-150">IoT-Edge ingår kärnor i gatewayen.</span><span class="sxs-lookup"><span data-stu-id="49f1d-150">IoT Edge is the core part of the gateway.</span></span> <span data-ttu-id="49f1d-151">Följ dessa steg för att installera paketet:</span><span class="sxs-lookup"><span data-stu-id="49f1d-151">To install the package, follow these steps:</span></span>

1. <span data-ttu-id="49f1d-152">Lägg till IoT moln databasen genom att köra följande kommandon i ett terminalfönster:</span><span class="sxs-lookup"><span data-stu-id="49f1d-152">Add the IoT cloud repository by running the following commands in a terminal window:</span></span>

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   <span data-ttu-id="49f1d-153">Den `rpm` kommando importerar rpm-nyckel.</span><span class="sxs-lookup"><span data-stu-id="49f1d-153">The `rpm` command imports the rpm key.</span></span> <span data-ttu-id="49f1d-154">Den `smart channel` kommando lägger till rpm-kanal till Smart Package Manager.</span><span class="sxs-lookup"><span data-stu-id="49f1d-154">The `smart channel` command adds the rpm channel to the Smart Package Manager.</span></span> <span data-ttu-id="49f1d-155">Innan du kör den `smart update` kommando, se utdata som liknar nedan.</span><span class="sxs-lookup"><span data-stu-id="49f1d-155">Before you run the `smart update` command, you see an output like below.</span></span>

   ![rpm och smarta kanal kommandon utdata](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

   ```bash
   smart update
   ```

2. <span data-ttu-id="49f1d-157">Installera paketet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="49f1d-157">Install the package by running the following command:</span></span>

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   <span data-ttu-id="49f1d-158">`packagegroup-cloud-azure`är namnet på paketet.</span><span class="sxs-lookup"><span data-stu-id="49f1d-158">`packagegroup-cloud-azure` is the name of the package.</span></span> <span data-ttu-id="49f1d-159">Den `smart install` kommandot används för att installera paketet.</span><span class="sxs-lookup"><span data-stu-id="49f1d-159">The `smart install` command is used to install the package.</span></span>

   <span data-ttu-id="49f1d-160">När paketet har installerats, förväntas Intel NUC fungera som en gateway.</span><span class="sxs-lookup"><span data-stu-id="49f1d-160">After the package is installed, Intel NUC is expected to work as a gateway.</span></span>

## <a name="run-the-azure-iot-edge-helloworld-sample-application"></a><span data-ttu-id="49f1d-161">Kör exempelprogrammet Azure IoT kant ”hello_world”</span><span class="sxs-lookup"><span data-stu-id="49f1d-161">Run the Azure IoT Edge "hello_world" sample application</span></span>

<span data-ttu-id="49f1d-162">Gå till `azureiotgatewaysdk/samples` och kör exempelprogrammet exempel ”hello_world”.</span><span class="sxs-lookup"><span data-stu-id="49f1d-162">Go to `azureiotgatewaysdk/samples` and run the sample "hello_world" sample application.</span></span> <span data-ttu-id="49f1d-163">Det här exempelprogrammet skapar en gateway från den `hello_world.json` filen och använder de grundläggande komponenter för Azure IoT kant-arkitektur för att logga en hello world-meddelande till en fil var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="49f1d-163">This sample application creates a gateway from the `hello_world.json` file and uses the fundamental components of the Azure IoT Edge architecture to log a hello world message to a file every 5 seconds.</span></span>

<span data-ttu-id="49f1d-164">Du kan köra exempelprogrammet ”hello_world” exempel genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="49f1d-164">You can run the sample "hello_world" sample application by running the following command:</span></span>

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

<span data-ttu-id="49f1d-165">Exempelprogrammet resulterar i följande utdata om gateway-funktionerna fungerar korrekt:</span><span class="sxs-lookup"><span data-stu-id="49f1d-165">The sample application produces the following output if the gateway functionality is working correctly:</span></span>

![programutdata](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

<span data-ttu-id="49f1d-167">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="49f1d-167">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="summary"></a><span data-ttu-id="49f1d-168">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="49f1d-168">Summary</span></span>

<span data-ttu-id="49f1d-169">Grattis!</span><span class="sxs-lookup"><span data-stu-id="49f1d-169">Congratulations!</span></span> <span data-ttu-id="49f1d-170">Du har slutfört konfigurationen Intel NUC som en gateway.</span><span class="sxs-lookup"><span data-stu-id="49f1d-170">You've finished setting up Intel NUC as a gateway.</span></span> <span data-ttu-id="49f1d-171">Nu är du redo att gå vidare till nästa lektionen konfigurera värddatorn, skapa en Azure IoT-hubb och registrera dina Azure IoT hub-logisk enhet.</span><span class="sxs-lookup"><span data-stu-id="49f1d-171">Now you're ready to move on to the next lesson to set up your host computer, create an Azure IoT hub and register your Azure IoT hub logical device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="49f1d-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49f1d-172">Next steps</span></span>
[<span data-ttu-id="49f1d-173">Förbereda din värddatorn och Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="49f1d-173">Get your host computer and Azure IoT hub ready</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
