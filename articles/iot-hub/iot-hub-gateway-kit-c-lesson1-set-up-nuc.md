---
title: 'SensorTag enhet & Azure IoT Gateway - lektionen 1: Konfigurera Intel NUC | Microsoft Docs'
description: "Ställ in Intel NUC ska fungera som en IoT-gateway mellan en sensor och Azure IoT-hubb för att samla in sensorinformation och skicka den till IoT-hubb."
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
ms.openlocfilehash: 1a3a92ab8d08c6ed6f047208217c46022027157e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a><span data-ttu-id="5a4d6-104">Ställ in Intel NUC som en IoT-gateway</span><span class="sxs-lookup"><span data-stu-id="5a4d6-104">Set up Intel NUC as an IoT gateway</span></span>
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

## <a name="what-you-will-do"></a><span data-ttu-id="5a4d6-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="5a4d6-105">What you will do</span></span>

- <span data-ttu-id="5a4d6-106">Ställ in Intel NUC som en IoT-gateway.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-106">Set up Intel NUC as an IoT gateway.</span></span>
- <span data-ttu-id="5a4d6-107">Installera Azure IoT kanten på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-107">Install the Azure IoT Edge package on the Intel NUC.</span></span>
- <span data-ttu-id="5a4d6-108">Kör ett exempelprogram för ”hello_world” på Intel NUC att verifiera gateway-funktionerna.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-108">Run a "hello_world" sample application on the Intel NUC to verify the gateway functionality.</span></span>

  > <span data-ttu-id="5a4d6-109">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="5a4d6-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="5a4d6-110">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="5a4d6-110">What you will learn</span></span>

<span data-ttu-id="5a4d6-111">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="5a4d6-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="5a4d6-112">Så här ansluter du Intel NUC med kringutrustning.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-112">How to connect Intel NUC with peripherals.</span></span>
- <span data-ttu-id="5a4d6-113">Hur du installerar och uppdatera de nödvändiga paketen på Intel NUC med Smart Package Manager.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-113">How to install and update the required packages on Intel NUC using the Smart Package Manager.</span></span>
- <span data-ttu-id="5a4d6-114">Så här kör exempelprogrammet ”hello_world” för att verifiera gateway-funktionerna.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-114">How to run the "hello_world" sample application to verify the gateway functionality.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5a4d6-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="5a4d6-115">What you need</span></span>

- <span data-ttu-id="5a4d6-116">En Intel NUC Kit DE3815TYKE med Intel IoT Gateway-programsviten (vind floden Linux * 7.0.0.13) förinstallerat.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-116">An Intel NUC Kit DE3815TYKE with the Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) preinstalled.</span></span> <span data-ttu-id="5a4d6-117">[Klicka här om du vill köpa Groove IoT kommersiella Gateway Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).</span><span class="sxs-lookup"><span data-stu-id="5a4d6-117">[Click here to purchase Grove IoT Commercial Gateway Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).</span></span>
- <span data-ttu-id="5a4d6-118">En Ethernet-kabel.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-118">An Ethernet cable.</span></span>
- <span data-ttu-id="5a4d6-119">Ett tangentbord.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-119">A keyboard.</span></span>
- <span data-ttu-id="5a4d6-120">HDMI eller VGA-kabel.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-120">An HDMI or VGA cable.</span></span>
- <span data-ttu-id="5a4d6-121">En bildskärm med en HDMI eller VGA-port.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-121">A monitor with an HDMI or VGA port.</span></span>
- <span data-ttu-id="5a4d6-122">Valfritt: [Texas Instruments Sensor Tag (CC2650STK)](http://www.ti.com/tool/cc2650stk)</span><span class="sxs-lookup"><span data-stu-id="5a4d6-122">Optional: [Texas Instruments Sensor Tag (CC2650STK)](http://www.ti.com/tool/cc2650stk)</span></span>

![Gateway-Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a><span data-ttu-id="5a4d6-124">Anslut Intel NUC med kringutrustning</span><span class="sxs-lookup"><span data-stu-id="5a4d6-124">Connect Intel NUC with the peripherals</span></span>

<span data-ttu-id="5a4d6-125">På bilden nedan är ett exempel på Intel NUC som är kopplad till olika kringutrustning:</span><span class="sxs-lookup"><span data-stu-id="5a4d6-125">The image below is an example of Intel NUC that is connected with various peripherals:</span></span>

1. <span data-ttu-id="5a4d6-126">Ansluten till ett tangentbord.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-126">Connected to a keyboard.</span></span>
2. <span data-ttu-id="5a4d6-127">Ansluten till en bildskärm med en VGA- eller HDMI kabel.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-127">Connected to a monitor with a VGA cable or HDMI cable.</span></span>
3. <span data-ttu-id="5a4d6-128">Ansluten till ett kabelanslutet nätverk med en Ethernet-kabel.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-128">Connected to a wired network with an Ethernet cable.</span></span>
4. <span data-ttu-id="5a4d6-129">Ansluten till en strömförsörjning med en power-kabel.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-129">Connected to a power supply with a power cable.</span></span>

![Intel NUC ansluten till annan kringutrustning](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a><span data-ttu-id="5a4d6-131">Ansluta till Intel NUC system från värddatorn via SSH (Secure Shell)</span><span class="sxs-lookup"><span data-stu-id="5a4d6-131">Connect to the Intel NUC system from host computer via Secure Shell (SSH)</span></span>

<span data-ttu-id="5a4d6-132">Du behöver ett tangentbord och en Övervakare för att få IP-adressen till enheten Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-132">You will need a keyboard and a monitor to get the IP address of your Intel NUC device.</span></span> <span data-ttu-id="5a4d6-133">Om du redan vet IP-adress kan gå du vidare till steg 3 i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-133">If you already know the IP address, you can skip ahead to step 3 in this section.</span></span>

1. <span data-ttu-id="5a4d6-134">Aktivera Intel NUC genom att trycka på strömknappen och logga sedan in.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-134">Turn on the Intel NUC by pressing the power button and then log in.</span></span>

   <span data-ttu-id="5a4d6-135">Standard-användarnamn och lösenord är båda `root`.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-135">The default user name and password are both `root`.</span></span>

       > Hit the enter key on your keyboard if you see either of the following errors when you boot: 'A TPM error (7) occurred attempting to read a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. <span data-ttu-id="5a4d6-136">Hämta IP-adressen för Intel-NUC genom att köra den `ifconfig` på Intel NUC enheten.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-136">Get the IP address of the Intel NUC by running the `ifconfig` command on the Intel NUC device.</span></span>

   <span data-ttu-id="5a4d6-137">Här är ett exempel på utdata från.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-137">Here is an example of the command output.</span></span>

   ![ifconfig utdata visar Intel NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   <span data-ttu-id="5a4d6-139">I detta exempel värdet som följer `inet addr:` är IP-adressen som du behöver när ansluter till Intel NUC från en värddator.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-139">In this example, the value that follows `inet addr:` is the IP address that you need when connect to the Intel NUC from a host computer.</span></span>

3. <span data-ttu-id="5a4d6-140">Använd en av följande SSH-klienter från värddatorn för att ansluta till Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-140">Use one of the following SSH clients from your host computer to connect to Intel NUC.</span></span>

    - <span data-ttu-id="5a4d6-141">[PuTTY](http://www.putty.org/) för Windows.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-141">[PuTTY](http://www.putty.org/) for Windows.</span></span>
    - <span data-ttu-id="5a4d6-142">Den inbyggda SSH-klienten på Ubuntu eller macOS.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-142">The built-in SSH client on Ubuntu or macOS.</span></span>

   <span data-ttu-id="5a4d6-143">Det är mer effektivt och effektiv att driva en Intel NUC från en värddator.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-143">It is more efficient and productive to operate an Intel NUC from a host computer.</span></span> <span data-ttu-id="5a4d6-144">Du behöver Intel-NUC IP-adress, användarnamn och lösenord för att ansluta till den via en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-144">You'll need the Intel NUC's IP address, user name and password to connect to it via an SSH client.</span></span> <span data-ttu-id="5a4d6-145">Här är ett exempel som använder en SSH-klient på macOS.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-145">Here is an example that uses an SSH client on macOS.</span></span>
   <span data-ttu-id="5a4d6-146">![SSH-klienten körs på macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span><span class="sxs-lookup"><span data-stu-id="5a4d6-146">![SSH client running on macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span></span>

## <a name="install-the-azure-iot-edge-package"></a><span data-ttu-id="5a4d6-147">Installera Azure IoT kant-paket</span><span class="sxs-lookup"><span data-stu-id="5a4d6-147">Install the Azure IoT Edge package</span></span>

<span data-ttu-id="5a4d6-148">Azure IoT kant-paketet innehåller före kompilerade binärfilerna för IoT kant och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-148">The Azure IoT Edge package contains the pre-compiled binaries of IoT Edge and its dependencies.</span></span> <span data-ttu-id="5a4d6-149">Dessa binärfiler är Azure IoT kant, Azure IoT-SDK och motsvarande verktyg.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-149">These binaries are Azure IoT Edge, the Azure IoT SDK and the corresponding tools.</span></span> <span data-ttu-id="5a4d6-150">Paketet innehåller också en ”hello_world” exempelprogrammet används för att verifiera gateway-funktionerna.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-150">The package also contains a "hello_world" sample application is used to validate the gateway functionality.</span></span> <span data-ttu-id="5a4d6-151">IoT-Edge ingår kärnor i gatewayen.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-151">IoT Edge is the core part of the gateway.</span></span> 

<span data-ttu-id="5a4d6-152">Följ dessa steg för att installera paketet.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-152">Follow these steps to install the package.</span></span>

1. <span data-ttu-id="5a4d6-153">Lägg till molnet för IoT-databasen genom att köra följande kommandon i ett terminalfönster:</span><span class="sxs-lookup"><span data-stu-id="5a4d6-153">Add the IoT Cloud repository by running the following commands in a terminal window:</span></span>

   ```bash
   rpm --import https://iotdk.intel.com/misc/iot_pub2.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
   ```

   > <span data-ttu-id="5a4d6-154">Ange ”y” när du uppmanas du att inkludera på den här kanalen?</span><span class="sxs-lookup"><span data-stu-id="5a4d6-154">Enter 'y', when it prompts you to 'Include this channel?'</span></span>
   
   <span data-ttu-id="5a4d6-155">Om du får ett `import read failed(-1)` fel, Använd följande kommandon för att lösa problemet:</span><span class="sxs-lookup"><span data-stu-id="5a4d6-155">If you receive an `import read failed(-1)` error, use the following commands to resolve the issue:</span></span>
   ```bash
   wget http://iotdk.intel.com/misc/iot_pub2.key 
   rpm --import iot_pub2.key  
   ```

   <span data-ttu-id="5a4d6-156">Den `rpm` kommando importerar rpm-nyckel.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-156">The `rpm` command imports the rpm key.</span></span> <span data-ttu-id="5a4d6-157">Den `smart channel` kommando lägger till rpm-kanal till Smart Package Manager.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-157">The `smart channel` command adds the rpm channel to the Smart Package Manager.</span></span> <span data-ttu-id="5a4d6-158">Innan du kör den `smart update` kommandot visas utdata som nedan.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-158">Before you run the `smart update` command, you will see an output like below.</span></span>

   ![rpm och smarta kanal kommandon utdata](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. <span data-ttu-id="5a4d6-160">Kör kommandot smart update:</span><span class="sxs-lookup"><span data-stu-id="5a4d6-160">Execute the smart update command:</span></span>

   ```bash
   smart update
   ```

3. <span data-ttu-id="5a4d6-161">Installera Azure IoT Gateway-paketet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5a4d6-161">Install the Azure IoT Gateway package by running the following command:</span></span>

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   <span data-ttu-id="5a4d6-162">`packagegroup-cloud-azure`är namnet på paketet.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-162">`packagegroup-cloud-azure` is the name of the package.</span></span> <span data-ttu-id="5a4d6-163">Den `smart install` kommandot används för att installera paketet.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-163">The `smart install` command is used to install the package.</span></span>

    > <span data-ttu-id="5a4d6-164">Kör följande kommando om du ser felet: 'offentlig nyckel som är inte tillgänglig ”</span><span class="sxs-lookup"><span data-stu-id="5a4d6-164">Run the following command if you see this error: 'public key not available'</span></span>

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
    > <span data-ttu-id="5a4d6-165">Starta om Intel NUC om du ser felet: 'inga erbjuder util-linux-dev'</span><span class="sxs-lookup"><span data-stu-id="5a4d6-165">Reboot the Intel NUC if you see this error: 'no package provides util-linux-dev'</span></span>

   <span data-ttu-id="5a4d6-166">När paketet har installerats, är Intel NUC redo att fungera som en gateway.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-166">After the package is installed, Intel NUC is ready to function as a gateway.</span></span>

## <a name="run-the-azure-iot-edge-helloworld-sample-application"></a><span data-ttu-id="5a4d6-167">Kör exempelprogrammet Azure IoT kant ”hello_world”</span><span class="sxs-lookup"><span data-stu-id="5a4d6-167">Run the Azure IoT Edge "hello_world" sample application</span></span>

<span data-ttu-id="5a4d6-168">Följande exempelprogrammet skapar en gateway från en `hello_world.json` filen och använder de grundläggande komponenter av arkitekturen i Azure IoT kant för att logga en hello world-meddelande till en fil (log.txt) var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-168">The following sample application creates a gateway from a `hello_world.json` file and uses the fundamental components of Azure IoT Edge architecture to log a hello world message to a file (log.txt) every 5 seconds.</span></span>

<span data-ttu-id="5a4d6-169">Du kan köra Hello World-exempel genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="5a4d6-169">You can run the Hello World sample by executing the following commands:</span></span>

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

<span data-ttu-id="5a4d6-170">Kan programmet Hello World kör några minuter och tryck sedan på RETUR om du vill stoppa den.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-170">Let the Hello World application run for a few minutes and then hit the Enter key to stop it.</span></span>
<span data-ttu-id="5a4d6-171">![programutdata](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)</span><span class="sxs-lookup"><span data-stu-id="5a4d6-171">![application output](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)</span></span>

> <span data-ttu-id="5a4d6-172">Du kan ignorera eventuella: Ogiltigt argument handle(NULL)'-fel som visas när du klickar på RETUR.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-172">You can ignore any 'invalid argument handle(NULL)' errors that appear after you hit Enter.</span></span>

<span data-ttu-id="5a4d6-173">Du kan kontrollera att gatewayen har körts genom att öppna filen log.txt som nu finns i mappen hello_world ![log.txt directory vy](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)</span><span class="sxs-lookup"><span data-stu-id="5a4d6-173">You can verify that the gateway ran successfully by opening the log.txt file that is now in your hello_world folder ![log.txt directory view](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)</span></span>

<span data-ttu-id="5a4d6-174">Öppna log.txt med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5a4d6-174">Open log.txt using the following command:</span></span>

```bash
vim log.txt
```

<span data-ttu-id="5a4d6-175">Sedan visas innehållet i log.txt som ska vara en JSON-formaterade utdata loggning meddelanden som har skrivits var femte sekund av gateway Hello World-modulen.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-175">You will then see the contents of log.txt, which will be a JSON formatted output of the logging messages that were written every 5 seconds by the gateway Hello World module.</span></span>
<span data-ttu-id="5a4d6-176">![log.txt directory vy](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)</span><span class="sxs-lookup"><span data-stu-id="5a4d6-176">![log.txt directory view](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)</span></span>

<span data-ttu-id="5a4d6-177">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="5a4d6-177">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="summary"></a><span data-ttu-id="5a4d6-178">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5a4d6-178">Summary</span></span>

<span data-ttu-id="5a4d6-179">Grattis!</span><span class="sxs-lookup"><span data-stu-id="5a4d6-179">Congratulations!</span></span> <span data-ttu-id="5a4d6-180">Du har slutfört konfigurationen Intel NUC som en gateway.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-180">You've finished setting up Intel NUC as a gateway.</span></span> <span data-ttu-id="5a4d6-181">Nu är du redo att gå vidare till nästa lektionen att konfigurera värddatorn, skapa en Azure IoT-hubb och registrera din logiska Azure IoT Hub-enhet.</span><span class="sxs-lookup"><span data-stu-id="5a4d6-181">Now you're ready to move on to the next lesson to set up your host computer, create an Azure IoT Hub and register your Azure IoT Hub logical device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a4d6-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5a4d6-182">Next steps</span></span>
[<span data-ttu-id="5a4d6-183">Använda en IoT-gateway för att ansluta en enhet till Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="5a4d6-183">Use an IoT gateway to connect a device to Azure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

