---
title: "aaaRaspberry Pi toocloud (Node.js) – ansluta hallon Pi tooAzure IoT-hubb | Microsoft Docs"
description: "Lär dig hur toosetup och ansluta hallon Pi tooAzure IoT-hubb för hallon Pi toosend data toohello Azure-molnplattform i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure iot raspberry pi, raspberry pi iot hub, raspberry pi skicka data toocloud, raspberry pi toocloud
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/27/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 57a15ab3984021a9c18ff0aa1316a4d4c6ebdec1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-nodejs"></a><span data-ttu-id="80707-104">Ansluta hallon Pi tooAzure IoT-hubb (Node.js)</span><span class="sxs-lookup"><span data-stu-id="80707-104">Connect Raspberry Pi tooAzure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="80707-105">I den här kursen kan du börja genom att lära dig hello grunderna i att arbeta med hallon Pi med Raspbian.</span><span class="sxs-lookup"><span data-stu-id="80707-105">In this tutorial, you begin by learning hello basics of working with Raspberry Pi that's running Raspbian.</span></span> <span data-ttu-id="80707-106">Du lär dig hur tooseamlessly ansluta enheter toohello molnet med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="80707-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> <span data-ttu-id="80707-107">För Windows 10 IoT Core prover gå toohello [Windows Dev Center](http://www.windowsondevices.com/).</span><span class="sxs-lookup"><span data-stu-id="80707-107">For Windows 10 IoT Core samples, go toohello [Windows Dev Center](http://www.windowsondevices.com/).</span></span>

<span data-ttu-id="80707-108">Har du en kit ännu?</span><span class="sxs-lookup"><span data-stu-id="80707-108">Don't have a kit yet?</span></span> <span data-ttu-id="80707-109">Försök [hallon Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="80707-109">Try [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md).</span></span> <span data-ttu-id="80707-110">Eller köp en ny kit [här](https://azure.microsoft.com/develop/iot/starter-kits).</span><span class="sxs-lookup"><span data-stu-id="80707-110">Or buy a new kit [here](https://azure.microsoft.com/develop/iot/starter-kits).</span></span>


## <a name="what-you-do"></a><span data-ttu-id="80707-111">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="80707-111">What you do</span></span>

* <span data-ttu-id="80707-112">Skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80707-112">Create an IoT hub.</span></span>
* <span data-ttu-id="80707-113">Registrera en enhet för Pi i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80707-113">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="80707-114">Konfigurera Raspberry Pi.</span><span class="sxs-lookup"><span data-stu-id="80707-114">Setup Raspberry Pi.</span></span>
* <span data-ttu-id="80707-115">Kör ett exempelprogram på Pi toosend sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80707-115">Run a sample application on Pi toosend sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="80707-116">Ansluta hallon Pi tooan IoT-hubb som du skapar.</span><span class="sxs-lookup"><span data-stu-id="80707-116">Connect Raspberry Pi tooan IoT hub that you create.</span></span> <span data-ttu-id="80707-117">Sedan kör du ett exempelprogram på Pi toocollect temperatur- och fuktighetskonsekvens data från en BME280 sensor.</span><span class="sxs-lookup"><span data-stu-id="80707-117">Then you run a sample application on Pi toocollect temperature and humidity data from a BME280 sensor.</span></span> <span data-ttu-id="80707-118">Slutligen kan skicka du hello sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80707-118">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="80707-119">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="80707-119">What you learn</span></span>

* <span data-ttu-id="80707-120">Hur toocreate en Azure IoT-hubb och få nya enheten anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="80707-120">How toocreate an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="80707-121">Hur tooconnect Pi med en BME280 sensor.</span><span class="sxs-lookup"><span data-stu-id="80707-121">How tooconnect Pi with a BME280 sensor.</span></span>
* <span data-ttu-id="80707-122">Hur toocollect sensordata genom att köra ett exempelprogram på Pi.</span><span class="sxs-lookup"><span data-stu-id="80707-122">How toocollect sensor data by running a sample application on Pi.</span></span>
* <span data-ttu-id="80707-123">Hur toosend sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80707-123">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="80707-124">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="80707-124">What you need</span></span>

![Vad du behöver](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* <span data-ttu-id="80707-126">hello hallon Pi 2 eller 3 för hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="80707-126">hello Raspberry Pi 2 or Raspberry Pi 3 board.</span></span>
* <span data-ttu-id="80707-127">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="80707-127">An active Azure subscription.</span></span> <span data-ttu-id="80707-128">Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="80707-128">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="80707-129">En Övervakare, en USB-tangentbord och mus som ansluter tooPi.</span><span class="sxs-lookup"><span data-stu-id="80707-129">A monitor, a USB keyboard, and mouse that connect tooPi.</span></span>
* <span data-ttu-id="80707-130">En Mac- eller en dator som kör Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="80707-130">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="80707-131">En Internetanslutning.</span><span class="sxs-lookup"><span data-stu-id="80707-131">An Internet connection.</span></span>
* <span data-ttu-id="80707-132">16 GB eller högre microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="80707-132">A 16 GB or above microSD card.</span></span>
* <span data-ttu-id="80707-133">En USB-SD-kort eller microSD-kort tooburn hello operativsystemavbildning på hello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="80707-133">A USB-SD adapter or microSD card tooburn hello operating system image onto hello microSD card.</span></span>
* <span data-ttu-id="80707-134">En v 5-2-amp elkälla med hello 6 fotavtryck micro USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="80707-134">A 5-volt 2-amp power supply with hello 6-foot micro USB cable.</span></span>

<span data-ttu-id="80707-135">hello följande objekt är valfria:</span><span class="sxs-lookup"><span data-stu-id="80707-135">hello following items are optional:</span></span>

* <span data-ttu-id="80707-136">En monterad Adafruit BME280 temperatur, tryck och fuktighet sensorn.</span><span class="sxs-lookup"><span data-stu-id="80707-136">An assembled Adafruit BME280 temperature, pressure, and humidity sensor.</span></span>
* <span data-ttu-id="80707-137">En breadboard.</span><span class="sxs-lookup"><span data-stu-id="80707-137">A breadboard.</span></span>
* <span data-ttu-id="80707-138">6-F/M omkopplare kablar.</span><span class="sxs-lookup"><span data-stu-id="80707-138">6 F/M jumper wires.</span></span>
* <span data-ttu-id="80707-139">En indirekt Indikator för 10 mm.</span><span class="sxs-lookup"><span data-stu-id="80707-139">A diffused 10-mm LED.</span></span>


> [!NOTE] 
<span data-ttu-id="80707-140">Objekten är valfritt eftersom hello kod exempel support simulerade sensordata.</span><span class="sxs-lookup"><span data-stu-id="80707-140">These items are optional because hello code sample support simulated sensor data.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a><span data-ttu-id="80707-141">Konfigurera Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="80707-141">Setup Raspberry Pi</span></span>

### <a name="install-hello-raspbian-operating-system-for-pi"></a><span data-ttu-id="80707-142">Installera operativsystemet för hello Raspbian för Pi</span><span class="sxs-lookup"><span data-stu-id="80707-142">Install hello Raspbian operating system for Pi</span></span>

<span data-ttu-id="80707-143">Förbered hello microSD-kort för installation av hello Raspbian bild.</span><span class="sxs-lookup"><span data-stu-id="80707-143">Prepare hello microSD card for installation of hello Raspbian image.</span></span>

1. <span data-ttu-id="80707-144">Hämta Raspbian.</span><span class="sxs-lookup"><span data-stu-id="80707-144">Download Raspbian.</span></span>
   1. <span data-ttu-id="80707-145">[Hämta Raspbian Jessie med skrivbordet](https://www.raspberrypi.org/downloads/raspbian/) (hello ZIP-fil).</span><span class="sxs-lookup"><span data-stu-id="80707-145">[Download Raspbian Jessie with Desktop](https://www.raspberrypi.org/downloads/raspbian/) (hello .zip file).</span></span>
   1. <span data-ttu-id="80707-146">Extrahera hello Raspbian bild tooa mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="80707-146">Extract hello Raspbian image tooa folder on your computer.</span></span>
1. <span data-ttu-id="80707-147">Installera Raspbian toohello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="80707-147">Install Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="80707-148">[Hämta och installera hello Etcher SD-kort brännare verktyget](https://etcher.io/).</span><span class="sxs-lookup"><span data-stu-id="80707-148">[Download and install hello Etcher SD card burner utility](https://etcher.io/).</span></span>
   1. <span data-ttu-id="80707-149">Kör Etcher och välj hello Raspbian avbildning som du extraherade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="80707-149">Run Etcher and select hello Raspbian image that you extracted in step 1.</span></span>
   1. <span data-ttu-id="80707-150">Välj enhet för hello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="80707-150">Select hello microSD card drive.</span></span> <span data-ttu-id="80707-151">Observera att Etcher kanske redan har valt hello rätt enhet.</span><span class="sxs-lookup"><span data-stu-id="80707-151">Note that Etcher may have already selected hello correct drive.</span></span>
   1. <span data-ttu-id="80707-152">Klicka på Flash tooinstall Raspbian toohello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="80707-152">Click Flash tooinstall Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="80707-153">Ta bort hello microSD-kort från datorn när installationen är klar.</span><span class="sxs-lookup"><span data-stu-id="80707-153">Remove hello microSD card from your computer when installation is complete.</span></span> <span data-ttu-id="80707-154">Det är säker tooremove hello microSD-kort direkt eftersom Etcher automatiskt matar ut eller demonterar hello microSD-kort när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="80707-154">It's safe tooremove hello microSD card directly because Etcher automatically ejects or unmounts hello microSD card upon completion.</span></span>
   1. <span data-ttu-id="80707-155">Infoga hello microSD-kort i Pi.</span><span class="sxs-lookup"><span data-stu-id="80707-155">Insert hello microSD card into Pi.</span></span>

### <a name="enable-ssh-and-i2c"></a><span data-ttu-id="80707-156">Aktivera SSH och I2C</span><span class="sxs-lookup"><span data-stu-id="80707-156">Enable SSH and I2C</span></span>

1. <span data-ttu-id="80707-157">Ansluta Pi toohello bildskärm, tangentbord och mus, starta Pi och logga sedan in Raspbian med hjälp av `pi` som hello användarnamn och `raspberry` som hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="80707-157">Connect Pi toohello monitor, keyboard and mouse, start Pi and then log in Raspbian by using `pi` as hello user name and `raspberry` as hello password.</span></span>
1. <span data-ttu-id="80707-158">Klicka på hello Raspberry ikonen > **inställningar** > **hallon Pi Configuration**.</span><span class="sxs-lookup"><span data-stu-id="80707-158">Click hello Raspberry icon > **Preferences** > **Raspberry Pi Configuration**.</span></span>

   ![Hej Raspbian Inställningar-menyn](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

1. <span data-ttu-id="80707-160">På hello **gränssnitt** ställer du in **I2C** och **SSH** för**aktivera**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="80707-160">On hello **Interfaces** tab, set **I2C** and **SSH** too**Enable**, and then click **OK**.</span></span> <span data-ttu-id="80707-161">Om du inte har fysisk sensorer och vill toouse simulerade sensordata, är det här steget valfritt.</span><span class="sxs-lookup"><span data-stu-id="80707-161">If you don't have physical sensors and want toouse simulated sensor data, this step is optional.</span></span>

   ![Aktivera I2C och SSH på Raspberry Pi](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
<span data-ttu-id="80707-163">tooenable SSH och I2C hittar du mer referensdokument på [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) och [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span><span class="sxs-lookup"><span data-stu-id="80707-163">tooenable SSH and I2C, you can find more reference documents on [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) and [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span></span>

### <a name="connect-hello-sensor-toopi"></a><span data-ttu-id="80707-164">Ansluta hello sensor tooPi</span><span class="sxs-lookup"><span data-stu-id="80707-164">Connect hello sensor tooPi</span></span>

<span data-ttu-id="80707-165">Använd hello breadboard och omkopplare kablar tooconnect en Indikator och en BME280 tooPi på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="80707-165">Use hello breadboard and jumper wires tooconnect an LED and a BME280 tooPi as follows.</span></span> <span data-ttu-id="80707-166">Om du inte har hello sensor [hoppa över det här avsnittet](#connect-pi-to-the-network).</span><span class="sxs-lookup"><span data-stu-id="80707-166">If you don’t have hello sensor, [skip this section](#connect-pi-to-the-network).</span></span>

![hello hallon Pi och sensor-anslutning](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

<span data-ttu-id="80707-168">Hej BME280 sensor kan samla in data för temperatur- och fuktighetskonsekvens.</span><span class="sxs-lookup"><span data-stu-id="80707-168">hello BME280 sensor can collect temperature and humidity data.</span></span> <span data-ttu-id="80707-169">Och hello Indikator blinkar om det finns en kommunikation mellan enheten och hello moln.</span><span class="sxs-lookup"><span data-stu-id="80707-169">And hello LED will blink if there is a communication between device and hello cloud.</span></span> 

<span data-ttu-id="80707-170">För sensor PIN-koder, använder du följande kablar hello:</span><span class="sxs-lookup"><span data-stu-id="80707-170">For sensor pins, use hello following wiring:</span></span>

| <span data-ttu-id="80707-171">Starta (Sensor & Indikator)</span><span class="sxs-lookup"><span data-stu-id="80707-171">Start (Sensor & LED)</span></span>     | <span data-ttu-id="80707-172">END (moderkort)</span><span class="sxs-lookup"><span data-stu-id="80707-172">End (Board)</span></span>            | <span data-ttu-id="80707-173">Kabel färg</span><span class="sxs-lookup"><span data-stu-id="80707-173">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="80707-174">VDD (PIN-kod 5G)</span><span class="sxs-lookup"><span data-stu-id="80707-174">VDD (Pin 5G)</span></span>             | <span data-ttu-id="80707-175">3, 3v PWR (PIN-kod 1)</span><span class="sxs-lookup"><span data-stu-id="80707-175">3.3V PWR (Pin 1)</span></span>       | <span data-ttu-id="80707-176">Vita kabeln</span><span class="sxs-lookup"><span data-stu-id="80707-176">White cable</span></span>   |
| <span data-ttu-id="80707-177">JORD (PIN-kod 7G)</span><span class="sxs-lookup"><span data-stu-id="80707-177">GND (Pin 7G)</span></span>             | <span data-ttu-id="80707-178">JORD (PIN-kod 6)</span><span class="sxs-lookup"><span data-stu-id="80707-178">GND (Pin 6)</span></span>            | <span data-ttu-id="80707-179">Bruna kabel</span><span class="sxs-lookup"><span data-stu-id="80707-179">Brown cable</span></span>   |
| <span data-ttu-id="80707-180">SDI (PIN-kod 10G)</span><span class="sxs-lookup"><span data-stu-id="80707-180">SDI (Pin 10G)</span></span>            | <span data-ttu-id="80707-181">I2C1 SDA (PIN-kod 3)</span><span class="sxs-lookup"><span data-stu-id="80707-181">I2C1 SDA (Pin 3)</span></span>       | <span data-ttu-id="80707-182">Röd kabel</span><span class="sxs-lookup"><span data-stu-id="80707-182">Red cable</span></span>     |
| <span data-ttu-id="80707-183">SCK (PIN-kod 8G)</span><span class="sxs-lookup"><span data-stu-id="80707-183">SCK (Pin 8G)</span></span>             | <span data-ttu-id="80707-184">I2C1 SCL (PIN-kod 5)</span><span class="sxs-lookup"><span data-stu-id="80707-184">I2C1 SCL (Pin 5)</span></span>       | <span data-ttu-id="80707-185">Orange kabel</span><span class="sxs-lookup"><span data-stu-id="80707-185">Orange cable</span></span>  |
| <span data-ttu-id="80707-186">Indikator VDD (PIN-kod 18F)</span><span class="sxs-lookup"><span data-stu-id="80707-186">LED VDD (Pin 18F)</span></span>        | <span data-ttu-id="80707-187">GPIO 24 (PIN-kod 18)</span><span class="sxs-lookup"><span data-stu-id="80707-187">GPIO 24 (Pin 18)</span></span>       | <span data-ttu-id="80707-188">Vita kabeln</span><span class="sxs-lookup"><span data-stu-id="80707-188">White cable</span></span>   |
| <span data-ttu-id="80707-189">Indikator jord (PIN-kod 17F)</span><span class="sxs-lookup"><span data-stu-id="80707-189">LED GND (Pin 17F)</span></span>        | <span data-ttu-id="80707-190">JORD (PIN-kod 20)</span><span class="sxs-lookup"><span data-stu-id="80707-190">GND (Pin 20)</span></span>           | <span data-ttu-id="80707-191">Svart kabel</span><span class="sxs-lookup"><span data-stu-id="80707-191">Black cable</span></span>   |

<span data-ttu-id="80707-192">Klicka på tooview [hallon Pi 2 och 3 PIN-kod mappningar](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) referens.</span><span class="sxs-lookup"><span data-stu-id="80707-192">Click tooview [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.</span></span>

<span data-ttu-id="80707-193">När du har anslutit BME280 tooyour hallon Pi, ska den som under bilden.</span><span class="sxs-lookup"><span data-stu-id="80707-193">After you've successfully connected BME280 tooyour Raspberry Pi, it should be like below image.</span></span>

![Anslutna Pi och BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-toohello-network"></a><span data-ttu-id="80707-195">Ansluta Pi toohello nätverk</span><span class="sxs-lookup"><span data-stu-id="80707-195">Connect Pi toohello network</span></span>

<span data-ttu-id="80707-196">Aktivera Pi med hjälp av hello micro USB-kabel och hello strömförsörjning.</span><span class="sxs-lookup"><span data-stu-id="80707-196">Turn on Pi by using hello micro USB cable and hello power supply.</span></span> <span data-ttu-id="80707-197">Använd hello Ethernet-kabel tooconnect Pi tooyour kabelanslutna nätverk eller följa hello [instruktioner från hello hallon Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour trådlöst nätverk.</span><span class="sxs-lookup"><span data-stu-id="80707-197">Use hello Ethernet cable tooconnect Pi tooyour wired network or follow hello [instructions from hello Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour wireless network.</span></span> <span data-ttu-id="80707-198">När din Pi har varit ansluten toohello nätverk, måste tootake anteckna hello [IP-adressen för din Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span><span class="sxs-lookup"><span data-stu-id="80707-198">After your Pi has been successfully connected toohello network, you need tootake a note of hello [IP address of your Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span></span>

![Anslutna toowired nätverk](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> <span data-ttu-id="80707-200">Se till att Pi är anslutna toohello samma nätverk som datorn.</span><span class="sxs-lookup"><span data-stu-id="80707-200">Make sure that Pi is connected toohello same network as your computer.</span></span> <span data-ttu-id="80707-201">Till exempel om datorn är ansluten tooa trådlösa nätverk medan Pi är anslutna tooa kabelanslutet nätverk kan du kanske inte se hello IP-adress i hello devdisco utdata.</span><span class="sxs-lookup"><span data-stu-id="80707-201">For example, if your computer is connected tooa wireless network while Pi is connected tooa wired network, you might not see hello IP address in hello devdisco output.</span></span>

## <a name="run-a-sample-application-on-pi"></a><span data-ttu-id="80707-202">Kör ett exempelprogram på Pi</span><span class="sxs-lookup"><span data-stu-id="80707-202">Run a sample application on Pi</span></span>

### <a name="clone-sample-application-and-install-hello-prerequisite-packages"></a><span data-ttu-id="80707-203">Klona exempelprogrammet och installera nödvändiga hello-paket</span><span class="sxs-lookup"><span data-stu-id="80707-203">Clone sample application and install hello prerequisite packages</span></span>

1. <span data-ttu-id="80707-204">Använd någon av följande SSH-klienter från din värd datorn tooconnect tooyour hallon Pi hello.</span><span class="sxs-lookup"><span data-stu-id="80707-204">Use one of hello following SSH clients from your host computer tooconnect tooyour Raspberry Pi.</span></span>
   
   <span data-ttu-id="80707-205">**Windows-användare**</span><span class="sxs-lookup"><span data-stu-id="80707-205">**Windows Users**</span></span>
   1. <span data-ttu-id="80707-206">Hämta och installera [PuTTY](http://www.putty.org/) för Windows.</span><span class="sxs-lookup"><span data-stu-id="80707-206">Download and install [PuTTY](http://www.putty.org/) for Windows.</span></span> 
   1. <span data-ttu-id="80707-207">Kopiera hello IP-adressen för ditt avsnitt Pi till hello värden namn (eller IP-adress) och välj SSH som hello anslutningstypen.</span><span class="sxs-lookup"><span data-stu-id="80707-207">Copy hello IP address of your Pi into hello Host name (or IP address) section and select SSH as hello connection type.</span></span>
   
   ![PuTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   <span data-ttu-id="80707-209">**Mac- och Ubuntu användare**</span><span class="sxs-lookup"><span data-stu-id="80707-209">**Mac and Ubuntu Users**</span></span>
   
   <span data-ttu-id="80707-210">Använd hello inbyggda SSH-klienten på Ubuntu eller macOS.</span><span class="sxs-lookup"><span data-stu-id="80707-210">Use hello built-in SSH client on Ubuntu or macOS.</span></span> <span data-ttu-id="80707-211">Du kan behöva toorun `ssh pi@<ip address of pi>` tooconnect Pi via SSH.</span><span class="sxs-lookup"><span data-stu-id="80707-211">You might need toorun `ssh pi@<ip address of pi>` tooconnect Pi via SSH.</span></span>
   > [!NOTE] 
   <span data-ttu-id="80707-212">hello Standardanvändarnamnet är `pi` , och hello lösenordet är `raspberry`.</span><span class="sxs-lookup"><span data-stu-id="80707-212">hello default username is `pi` , and hello password is `raspberry`.</span></span>

1. <span data-ttu-id="80707-213">Installera Node.js och NPM tooyour Pi.</span><span class="sxs-lookup"><span data-stu-id="80707-213">Install Node.js and NPM tooyour Pi.</span></span>
   
   <span data-ttu-id="80707-214">Först bör du kontrollera din Node.js-version med hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="80707-214">First you should check your Node.js version with hello following command.</span></span> 
   
   ```bash
   node -v
   ```

   <span data-ttu-id="80707-215">Om hello-versionen är lägre än 4.x eller om det finns inga Node.js på din Pi, kör följande kommando tooinstall hello eller uppdatera Node.js.</span><span class="sxs-lookup"><span data-stu-id="80707-215">If hello version is lower than 4.x or there is no Node.js on your Pi, then run hello following command tooinstall or update Node.js.</span></span>

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

1. <span data-ttu-id="80707-216">Klona hello exempelprogrammet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="80707-216">Clone hello sample application by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

1. <span data-ttu-id="80707-217">Installera alla paket med följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="80707-217">Install all packages by hello following command.</span></span> <span data-ttu-id="80707-218">Det innehåller Azure IoT-enhet SDK, BME280 Sensor bibliotek och kabeldragning Pi-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="80707-218">It includes Azure IoT device SDK, BME280 Sensor library and Wiring Pi library.</span></span>

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   <span data-ttu-id="80707-219">Det kan ta flera minuter toofinish denna installationsprocess beroende på nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="80707-219">It might take several minutes toofinish this installation process depending on your network connection.</span></span>

### <a name="configure-hello-sample-application"></a><span data-ttu-id="80707-220">Konfigurera hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="80707-220">Configure hello sample application</span></span>

1. <span data-ttu-id="80707-221">Öppna hello config-fil genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="80707-221">Open hello config file by running hello following commands:</span></span>

   ```bash
   nano config.json
   ```

   ![Config-fil](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   <span data-ttu-id="80707-223">Det finns två objekt i den här filen kan du configurate.</span><span class="sxs-lookup"><span data-stu-id="80707-223">There are two items in this file you can configurate.</span></span> <span data-ttu-id="80707-224">hello först en är `interval`, som definierar hello tidsintervall (i millisekunder) mellan två meddelanden som skickar toocloud.</span><span class="sxs-lookup"><span data-stu-id="80707-224">hello first one is `interval`, which defines hello time interval (in milliseconds) between two messages that send toocloud.</span></span> <span data-ttu-id="80707-225">Hej andra `simulatedData`, vilket är ett booleskt värde för om toouse simulerade sensordata eller inte.</span><span class="sxs-lookup"><span data-stu-id="80707-225">hello second one `simulatedData`,which is a Boolean value for whether toouse simulated sensor data or not.</span></span>

   <span data-ttu-id="80707-226">Om du **saknar hello sensor**, Ställ in hello `simulatedData` värdet för`true` toomake hello exempelprogrammet skapa och använda simulerade sensordata.</span><span class="sxs-lookup"><span data-stu-id="80707-226">If you **don't have hello sensor**, set hello `simulatedData` value too`true` toomake hello sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="80707-227">Spara och avsluta genom att trycka på CTRL-O > Ange > kontrollen X.</span><span class="sxs-lookup"><span data-stu-id="80707-227">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="run-hello-sample-application"></a><span data-ttu-id="80707-228">Köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="80707-228">Run hello sample application</span></span>

<span data-ttu-id="80707-229">Kör exempelprogrammet hello genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="80707-229">Run hello sample application by running hello following command:</span></span>

   ```bash
   sudo node index.js '<YOUR AZURE IOT HUB DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   <span data-ttu-id="80707-230">Kontrollera att du kopiera / klistra in anslutningssträngen för hello enhet till hello enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="80707-230">Make sure you copy-paste hello device connection string into hello single quotes.</span></span>


<span data-ttu-id="80707-231">Du bör se hello följande utdata som visar hello sensor data och hello meddelanden som skickas tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80707-231">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub.</span></span>

![Utdata - sensordata som skickas från hallon Pi tooyour IoT-hubb](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a><span data-ttu-id="80707-233">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="80707-233">Next steps</span></span>

<span data-ttu-id="80707-234">Du har kört ett program toocollect sensor exempeldata och skicka den tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80707-234">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span> <span data-ttu-id="80707-235">toosee hälsningsmeddelande som din hallon Pi skickats tooyour IoT-hubb eller skicka meddelanden tooyour hallon Pi i ett kommandoradsgränssnitt finns hello [hantera moln enhet meddelanden med iothub explorer kursen](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span><span class="sxs-lookup"><span data-stu-id="80707-235">toosee hello messages that your Raspberry Pi has sent tooyour IoT hub or send messages tooyour Raspberry Pi in a command line interface, see hello [Manage cloud device messaging with iothub-explorer tutorial](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
