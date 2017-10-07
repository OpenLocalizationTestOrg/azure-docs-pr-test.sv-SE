---
title: "aaaRaspberry Pi toocloud (Node.js) – ansluta hallon Pi tooAzure IoT-hubb | Microsoft Docs"
description: "Ansluta hallon Pi tooAzure IoT-hubb för hallon Pi toosend data toohello Azure-molnet."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Azure iot raspberry pi, raspberry pi iot hub, raspberry pi skicka data toocloud, raspberry pi toocloud
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/27/2017
ms.author: xshi
ms.openlocfilehash: 07bc66983c427eab8118be18d91abb25deb03ad3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-nodejs"></a><span data-ttu-id="948c3-104">Ansluta hallon Pi tooAzure IoT-hubb (Node.js)</span><span class="sxs-lookup"><span data-stu-id="948c3-104">Connect Raspberry Pi tooAzure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="948c3-105">I den här kursen kan du börja genom att lära dig hello grunderna i att arbeta med hallon Pi med Raspbian.</span><span class="sxs-lookup"><span data-stu-id="948c3-105">In this tutorial, you begin by learning hello basics of working with Raspberry Pi that's running Raspbian.</span></span> <span data-ttu-id="948c3-106">Du lär dig hur tooseamlessly ansluta enheter toohello molnet med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="948c3-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> <span data-ttu-id="948c3-107">För Windows 10 IoT Core prover gå toohello [Windows Dev Center](http://www.windowsondevices.com/).</span><span class="sxs-lookup"><span data-stu-id="948c3-107">For Windows 10 IoT Core samples, go toohello [Windows Dev Center](http://www.windowsondevices.com/).</span></span>

<span data-ttu-id="948c3-108">Har du en kit ännu?</span><span class="sxs-lookup"><span data-stu-id="948c3-108">Don't have a kit yet?</span></span> <span data-ttu-id="948c3-109">Försök hello [hallon Pi 3 emulatorn](https://blogs.msdn.microsoft.com/iliast/2016/11/10/how-to-emulate-raspberry-pi/).</span><span class="sxs-lookup"><span data-stu-id="948c3-109">Try hello [Raspberry Pi 3 emulator](https://blogs.msdn.microsoft.com/iliast/2016/11/10/how-to-emulate-raspberry-pi/).</span></span> <span data-ttu-id="948c3-110">Eller köp en ny kit [här](https://azure.microsoft.com/develop/iot/starter-kits).</span><span class="sxs-lookup"><span data-stu-id="948c3-110">Or buy a new kit [here](https://azure.microsoft.com/develop/iot/starter-kits).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="948c3-111">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="948c3-111">What you do</span></span>

* <span data-ttu-id="948c3-112">Konfigurera Raspberry Pi.</span><span class="sxs-lookup"><span data-stu-id="948c3-112">Setup Raspberry Pi.</span></span>
* <span data-ttu-id="948c3-113">Skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="948c3-113">Create an IoT hub.</span></span>
* <span data-ttu-id="948c3-114">Registrera en enhet för Pi i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="948c3-114">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="948c3-115">Kör ett exempelprogram på Pi toosend sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="948c3-115">Run a sample application on Pi toosend sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="948c3-116">Ansluta hallon Pi tooan IoT-hubb som du skapar.</span><span class="sxs-lookup"><span data-stu-id="948c3-116">Connect Raspberry Pi tooan IoT hub that you create.</span></span> <span data-ttu-id="948c3-117">Sedan kör du ett exempelprogram på Pi toocollect temperatur- och fuktighetskonsekvens data från en BME280 sensor.</span><span class="sxs-lookup"><span data-stu-id="948c3-117">Then you run a sample application on Pi toocollect temperature and humidity data from a BME280 sensor.</span></span> <span data-ttu-id="948c3-118">Slutligen kan skicka du hello sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="948c3-118">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="948c3-119">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="948c3-119">What you learn</span></span>

* <span data-ttu-id="948c3-120">Hur toocreate en Azure IoT-hubb och få nya enheten anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="948c3-120">How toocreate an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="948c3-121">Hur tooconnect Pi med en BME280 sensor.</span><span class="sxs-lookup"><span data-stu-id="948c3-121">How tooconnect Pi with a BME280 sensor.</span></span>
* <span data-ttu-id="948c3-122">Hur toocollect sensordata genom att köra ett exempelprogram på Pi.</span><span class="sxs-lookup"><span data-stu-id="948c3-122">How toocollect sensor data by running a sample application on Pi.</span></span>
* <span data-ttu-id="948c3-123">Hur toosend sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="948c3-123">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="948c3-124">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="948c3-124">What you need</span></span>

![Vad du behöver](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* <span data-ttu-id="948c3-126">hello hallon Pi 2 eller 3 för hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="948c3-126">hello Raspberry Pi 2 or Raspberry Pi 3 board.</span></span>
* <span data-ttu-id="948c3-127">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="948c3-127">An active Azure subscription.</span></span> <span data-ttu-id="948c3-128">Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="948c3-128">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="948c3-129">En Övervakare, en USB-tangentbord och mus som ansluter tooPi.</span><span class="sxs-lookup"><span data-stu-id="948c3-129">A monitor, a USB keyboard, and mouse that connect tooPi.</span></span>
* <span data-ttu-id="948c3-130">En Mac- eller en dator som kör Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="948c3-130">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="948c3-131">En Internetanslutning.</span><span class="sxs-lookup"><span data-stu-id="948c3-131">An Internet connection.</span></span>
* <span data-ttu-id="948c3-132">16 GB eller högre microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="948c3-132">A 16 GB or above microSD card.</span></span>
* <span data-ttu-id="948c3-133">En USB-SD-kort eller microSD-kort tooburn hello operativsystemavbildning på hello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="948c3-133">A USB-SD adapter or microSD card tooburn hello operating system image onto hello microSD card.</span></span>
* <span data-ttu-id="948c3-134">En v 5-2-amp elkälla med hello 6 fotavtryck micro USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="948c3-134">A 5-volt 2-amp power supply with hello 6-foot micro USB cable.</span></span>

<span data-ttu-id="948c3-135">hello följande objekt är valfria:</span><span class="sxs-lookup"><span data-stu-id="948c3-135">hello following items are optional:</span></span>

* <span data-ttu-id="948c3-136">En monterad Adafruit BME280 temperatur, tryck och fuktighet sensorn.</span><span class="sxs-lookup"><span data-stu-id="948c3-136">An assembled Adafruit BME280 temperature, pressure, and humidity sensor.</span></span>
* <span data-ttu-id="948c3-137">En breadboard.</span><span class="sxs-lookup"><span data-stu-id="948c3-137">A breadboard.</span></span>
* <span data-ttu-id="948c3-138">6-F/M omkopplare kablar.</span><span class="sxs-lookup"><span data-stu-id="948c3-138">6 F/M jumper wires.</span></span>
* <span data-ttu-id="948c3-139">En indirekt Indikator för 10 mm.</span><span class="sxs-lookup"><span data-stu-id="948c3-139">A diffused 10-mm LED.</span></span>

  > [!NOTE] 
  <span data-ttu-id="948c3-140">Objekten är valfritt eftersom hello kod exempel support simulerade sensordata.</span><span class="sxs-lookup"><span data-stu-id="948c3-140">These items are optional because hello code sample support simulated sensor data.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a><span data-ttu-id="948c3-141">Konfigurera Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="948c3-141">Setup Raspberry Pi</span></span>

### <a name="install-hello-raspbian-operating-system-for-pi"></a><span data-ttu-id="948c3-142">Installera operativsystemet för hello Raspbian för Pi</span><span class="sxs-lookup"><span data-stu-id="948c3-142">Install hello Raspbian operating system for Pi</span></span>

<span data-ttu-id="948c3-143">Förbered hello microSD-kort för installation av hello Raspbian bild.</span><span class="sxs-lookup"><span data-stu-id="948c3-143">Prepare hello microSD card for installation of hello Raspbian image.</span></span>

1. <span data-ttu-id="948c3-144">Hämta Raspbian.</span><span class="sxs-lookup"><span data-stu-id="948c3-144">Download Raspbian.</span></span>
   1. <span data-ttu-id="948c3-145">[Hämta Raspbian Jessie med Pixel](https://www.raspberrypi.org/downloads/raspbian/) (hello ZIP-fil).</span><span class="sxs-lookup"><span data-stu-id="948c3-145">[Download Raspbian Jessie with Pixel](https://www.raspberrypi.org/downloads/raspbian/) (hello .zip file).</span></span>
   1. <span data-ttu-id="948c3-146">Extrahera hello Raspbian bild tooa mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="948c3-146">Extract hello Raspbian image tooa folder on your computer.</span></span>
1. <span data-ttu-id="948c3-147">Installera Raspbian toohello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="948c3-147">Install Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="948c3-148">[Hämta och installera hello Etcher SD-kort brännare verktyget](https://etcher.io/).</span><span class="sxs-lookup"><span data-stu-id="948c3-148">[Download and install hello Etcher SD card burner utility](https://etcher.io/).</span></span>
   1. <span data-ttu-id="948c3-149">Kör Etcher och välj hello Raspbian avbildning som du extraherade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="948c3-149">Run Etcher and select hello Raspbian image that you extracted in step 1.</span></span>
   1. <span data-ttu-id="948c3-150">Välj enhet för hello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="948c3-150">Select hello microSD card drive.</span></span> <span data-ttu-id="948c3-151">Observera att Etcher kanske redan har valt hello rätt enhet.</span><span class="sxs-lookup"><span data-stu-id="948c3-151">Note that Etcher may have already selected hello correct drive.</span></span>
   1. <span data-ttu-id="948c3-152">Klicka på Flash tooinstall Raspbian toohello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="948c3-152">Click Flash tooinstall Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="948c3-153">Ta bort hello microSD-kort från datorn när installationen är klar.</span><span class="sxs-lookup"><span data-stu-id="948c3-153">Remove hello microSD card from your computer when installation is complete.</span></span> <span data-ttu-id="948c3-154">Det är säker tooremove hello microSD-kort direkt eftersom Etcher automatiskt matar ut eller demonterar hello microSD-kort när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="948c3-154">It's safe tooremove hello microSD card directly because Etcher automatically ejects or unmounts hello microSD card upon completion.</span></span>
   1. <span data-ttu-id="948c3-155">Infoga hello microSD-kort i Pi.</span><span class="sxs-lookup"><span data-stu-id="948c3-155">Insert hello microSD card into Pi.</span></span>

### <a name="enable-ssh-and-i2c"></a><span data-ttu-id="948c3-156">Aktivera SSH och I2C</span><span class="sxs-lookup"><span data-stu-id="948c3-156">Enable SSH and I2C</span></span>

1. <span data-ttu-id="948c3-157">Ansluta Pi toohello bildskärm, tangentbord och mus, starta Pi och logga sedan in Raspbian med hjälp av `pi` som hello användarnamn och `raspberry` som hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="948c3-157">Connect Pi toohello monitor, keyboard and mouse, start Pi and then log in Raspbian by using `pi` as hello user name and `raspberry` as hello password.</span></span>
1. <span data-ttu-id="948c3-158">Klicka på hello Raspberry ikonen > **inställningar** > **hallon Pi Configuration**.</span><span class="sxs-lookup"><span data-stu-id="948c3-158">Click hello Raspberry icon > **Preferences** > **Raspberry Pi Configuration**.</span></span>

   ![Hej Raspbian Inställningar-menyn](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

1. <span data-ttu-id="948c3-160">På hello **gränssnitt** ställer du in **I2C** och **SSH** för**aktivera**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="948c3-160">On hello **Interfaces** tab, set **I2C** and **SSH** too**Enable**, and then click **OK**.</span></span> <span data-ttu-id="948c3-161">Om du inte har fysisk sensorer och vill toouse simulerade sensordata, är det här steget valfritt.</span><span class="sxs-lookup"><span data-stu-id="948c3-161">If you don't have physical sensors and want toouse simulated sensor data, this step is optional.</span></span>

   ![Aktivera I2C och SSH på Raspberry Pi](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
<span data-ttu-id="948c3-163">tooenable SSH och I2C hittar du mer referensdokument på [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) och [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span><span class="sxs-lookup"><span data-stu-id="948c3-163">tooenable SSH and I2C, you can find more reference documents on [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) and [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span></span>

### <a name="connect-hello-sensor-toopi"></a><span data-ttu-id="948c3-164">Ansluta hello sensor tooPi</span><span class="sxs-lookup"><span data-stu-id="948c3-164">Connect hello sensor tooPi</span></span>

<span data-ttu-id="948c3-165">Använd hello breadboard och omkopplare kablar tooconnect en Indikator och en BME280 tooPi på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="948c3-165">Use hello breadboard and jumper wires tooconnect an LED and a BME280 tooPi as follows.</span></span> <span data-ttu-id="948c3-166">Om du inte har hello sensor, hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="948c3-166">If you don’t have hello sensor, skip this section.</span></span>

![hello hallon Pi och sensor-anslutning](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

<span data-ttu-id="948c3-168">Hej BME280 sensor kan samla in data för temperatur- och fuktighetskonsekvens.</span><span class="sxs-lookup"><span data-stu-id="948c3-168">hello BME280 sensor can collect temperature and humidity data.</span></span> <span data-ttu-id="948c3-169">Och hello Indikator blinkar om det finns en kommunikation mellan enheten och hello moln.</span><span class="sxs-lookup"><span data-stu-id="948c3-169">And hello LED will blink if there is a communication between device and hello cloud.</span></span> 

<span data-ttu-id="948c3-170">För sensor PIN-koder, använder du följande kablar hello:</span><span class="sxs-lookup"><span data-stu-id="948c3-170">For sensor pins, use hello following wiring:</span></span>

| <span data-ttu-id="948c3-171">Starta (Sensor & Indikator)</span><span class="sxs-lookup"><span data-stu-id="948c3-171">Start (Sensor & LED)</span></span>     | <span data-ttu-id="948c3-172">END (moderkort)</span><span class="sxs-lookup"><span data-stu-id="948c3-172">End (Board)</span></span>            | <span data-ttu-id="948c3-173">Kabel färg</span><span class="sxs-lookup"><span data-stu-id="948c3-173">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="948c3-174">VDD (PIN-kod 5G)</span><span class="sxs-lookup"><span data-stu-id="948c3-174">VDD (Pin 5G)</span></span>             | <span data-ttu-id="948c3-175">3, 3v PWR (PIN-kod 1)</span><span class="sxs-lookup"><span data-stu-id="948c3-175">3.3V PWR (Pin 1)</span></span>       | <span data-ttu-id="948c3-176">Vita kabeln</span><span class="sxs-lookup"><span data-stu-id="948c3-176">White cable</span></span>   |
| <span data-ttu-id="948c3-177">JORD (PIN-kod 7G)</span><span class="sxs-lookup"><span data-stu-id="948c3-177">GND (Pin 7G)</span></span>             | <span data-ttu-id="948c3-178">JORD (PIN-kod 6)</span><span class="sxs-lookup"><span data-stu-id="948c3-178">GND (Pin 6)</span></span>            | <span data-ttu-id="948c3-179">Bruna kabel</span><span class="sxs-lookup"><span data-stu-id="948c3-179">Brown cable</span></span>   |
| <span data-ttu-id="948c3-180">SCK (PIN-kod 8G)</span><span class="sxs-lookup"><span data-stu-id="948c3-180">SCK (Pin 8G)</span></span>             | <span data-ttu-id="948c3-181">I2C1 SDA (PIN-kod 3)</span><span class="sxs-lookup"><span data-stu-id="948c3-181">I2C1 SDA (Pin 3)</span></span>       | <span data-ttu-id="948c3-182">Orange kabel</span><span class="sxs-lookup"><span data-stu-id="948c3-182">Orange cable</span></span>  |
| <span data-ttu-id="948c3-183">SDI (PIN-kod 10G)</span><span class="sxs-lookup"><span data-stu-id="948c3-183">SDI (Pin 10G)</span></span>            | <span data-ttu-id="948c3-184">I2C1 SCL (PIN-kod 5)</span><span class="sxs-lookup"><span data-stu-id="948c3-184">I2C1 SCL (Pin 5)</span></span>       | <span data-ttu-id="948c3-185">Röd kabel</span><span class="sxs-lookup"><span data-stu-id="948c3-185">Red cable</span></span>     |
| <span data-ttu-id="948c3-186">Indikator VDD (PIN-kod 18F)</span><span class="sxs-lookup"><span data-stu-id="948c3-186">LED VDD (Pin 18F)</span></span>        | <span data-ttu-id="948c3-187">GPIO 24 (PIN-kod 18)</span><span class="sxs-lookup"><span data-stu-id="948c3-187">GPIO 24 (Pin 18)</span></span>       | <span data-ttu-id="948c3-188">Vita kabeln</span><span class="sxs-lookup"><span data-stu-id="948c3-188">White cable</span></span>   |
| <span data-ttu-id="948c3-189">Indikator jord (PIN-kod 17F)</span><span class="sxs-lookup"><span data-stu-id="948c3-189">LED GND (Pin 17F)</span></span>        | <span data-ttu-id="948c3-190">JORD (PIN-kod 20)</span><span class="sxs-lookup"><span data-stu-id="948c3-190">GND (Pin 20)</span></span>           | <span data-ttu-id="948c3-191">Svart kabel</span><span class="sxs-lookup"><span data-stu-id="948c3-191">Black cable</span></span>   |

<span data-ttu-id="948c3-192">Klicka på tooview [hallon Pi 2 och 3 PIN-kod mappningar](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) referens.</span><span class="sxs-lookup"><span data-stu-id="948c3-192">Click tooview [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.</span></span>

<span data-ttu-id="948c3-193">När du har anslutit BME280 tooyour hallon Pi, ska den som under bilden.</span><span class="sxs-lookup"><span data-stu-id="948c3-193">After you've successfully connected BME280 tooyour Raspberry Pi, it should be like below image.</span></span>

![Anslutna Pi och BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-toohello-network"></a><span data-ttu-id="948c3-195">Ansluta Pi toohello nätverk</span><span class="sxs-lookup"><span data-stu-id="948c3-195">Connect Pi toohello network</span></span>

<span data-ttu-id="948c3-196">Aktivera Pi med hjälp av hello micro USB-kabel och hello strömförsörjning.</span><span class="sxs-lookup"><span data-stu-id="948c3-196">Turn on Pi by using hello micro USB cable and hello power supply.</span></span> <span data-ttu-id="948c3-197">Använd hello Ethernet-kabel tooconnect Pi tooyour kabelanslutna nätverk eller följa hello [instruktioner från hello hallon Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour trådlöst nätverk.</span><span class="sxs-lookup"><span data-stu-id="948c3-197">Use hello Ethernet cable tooconnect Pi tooyour wired network or follow hello [instructions from hello Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour wireless network.</span></span> <span data-ttu-id="948c3-198">När din Pi har varit ansluten toohello nätverk, måste tootake anteckna hello [IP-adressen för din Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span><span class="sxs-lookup"><span data-stu-id="948c3-198">After your Pi has been successfully connected toohello network, you need tootake a note of hello [IP address of your Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span></span>

![Anslutna toowired nätverk](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> <span data-ttu-id="948c3-200">Se till att Pi är anslutna toohello samma nätverk som datorn.</span><span class="sxs-lookup"><span data-stu-id="948c3-200">Make sure that Pi is connected toohello same network as your computer.</span></span> <span data-ttu-id="948c3-201">Till exempel om datorn är ansluten tooa trådlösa nätverk medan Pi är anslutna tooa kabelanslutet nätverk kan du kanske inte se hello IP-adress i hello devdisco utdata.</span><span class="sxs-lookup"><span data-stu-id="948c3-201">For example, if your computer is connected tooa wireless network while Pi is connected tooa wired network, you might not see hello IP address in hello devdisco output.</span></span>

## <a name="run-a-sample-application-on-pi"></a><span data-ttu-id="948c3-202">Kör ett exempelprogram på Pi</span><span class="sxs-lookup"><span data-stu-id="948c3-202">Run a sample application on Pi</span></span>

### <a name="clone-sample-application-and-install-hello-prerequisite-packages"></a><span data-ttu-id="948c3-203">Klona exempelprogrammet och installera nödvändiga hello-paket</span><span class="sxs-lookup"><span data-stu-id="948c3-203">Clone sample application and install hello prerequisite packages</span></span>

1. <span data-ttu-id="948c3-204">Använd någon av följande SSH-klienter från din värd datorn tooconnect tooyour hallon Pi hello.</span><span class="sxs-lookup"><span data-stu-id="948c3-204">Use one of hello following SSH clients from your host computer tooconnect tooyour Raspberry Pi.</span></span>
    - <span data-ttu-id="948c3-205">[PuTTY](http://www.putty.org/) för Windows.</span><span class="sxs-lookup"><span data-stu-id="948c3-205">[PuTTY](http://www.putty.org/) for Windows.</span></span> <span data-ttu-id="948c3-206">Du behöver hello IP-adressen för Pi-tooconnect den via SSH.</span><span class="sxs-lookup"><span data-stu-id="948c3-206">You need hello IP address of your Pi tooconnect it via SSH.</span></span>
    - <span data-ttu-id="948c3-207">hello inbyggda SSH-klienten på Ubuntu eller macOS.</span><span class="sxs-lookup"><span data-stu-id="948c3-207">hello built-in SSH client on Ubuntu or macOS.</span></span> <span data-ttu-id="948c3-208">Du måste köra `ssh pi@<ip address of pi>` tooconnect Pi via SSH.</span><span class="sxs-lookup"><span data-stu-id="948c3-208">You might need run `ssh pi@<ip address of pi>` tooconnect Pi via SSH.</span></span>

   > [!NOTE] 
   <span data-ttu-id="948c3-209">hello Standardanvändarnamnet är `pi` , och hello lösenordet är `raspberry`.</span><span class="sxs-lookup"><span data-stu-id="948c3-209">hello default username is `pi` , and hello password is `raspberry`.</span></span>

1. <span data-ttu-id="948c3-210">Installera Node.js och NPM tooyour Pi.</span><span class="sxs-lookup"><span data-stu-id="948c3-210">Install Node.js and NPM tooyour Pi.</span></span>
   
   <span data-ttu-id="948c3-211">Först bör du kontrollera din Node.js-version med hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="948c3-211">First you should check your Node.js version with hello following command.</span></span> 
   
   ```bash
   node -v
   ```

   <span data-ttu-id="948c3-212">Om hello-versionen är lägre än 4.x eller om det finns inga Node.js på din Pi, kör följande kommando tooinstall hello eller uppdatera Node.js.</span><span class="sxs-lookup"><span data-stu-id="948c3-212">If hello version is lower than 4.x or there is no Node.js on your Pi, then run hello following command tooinstall or update Node.js.</span></span>

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

1. <span data-ttu-id="948c3-213">Klona hello exempelprogrammet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="948c3-213">Clone hello sample application by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

1. <span data-ttu-id="948c3-214">Installera alla paket med följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="948c3-214">Install all packages by hello following command.</span></span> <span data-ttu-id="948c3-215">Det innehåller Azure IoT-enhet SDK, BME280 Sensor bibliotek och kabeldragning Pi-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="948c3-215">It includes Azure IoT device SDK, BME280 Sensor library and Wiring Pi library.</span></span>

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   <span data-ttu-id="948c3-216">Det kan ta flera minuter toofinish den här processen denpening för installation på din nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="948c3-216">It might take several minutes toofinish this installation process denpening on your network connection.</span></span>

### <a name="configure-hello-sample-application"></a><span data-ttu-id="948c3-217">Konfigurera hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="948c3-217">Configure hello sample application</span></span>

1. <span data-ttu-id="948c3-218">Öppna hello config-fil genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="948c3-218">Open hello config file by running hello following commands:</span></span>

   ```bash
   nano config.json
   ```

   ![Config-fil](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   <span data-ttu-id="948c3-220">Det finns två objekt i den här filen kan du configurate.</span><span class="sxs-lookup"><span data-stu-id="948c3-220">There are two items in this file you can configurate.</span></span> <span data-ttu-id="948c3-221">hello först en är `interval`, som definierar hello tidsintervallet mellan två meddelanden som skickar toocloud.</span><span class="sxs-lookup"><span data-stu-id="948c3-221">hello first one is `interval`, which defines hello time interval between two messages that send toocloud.</span></span> <span data-ttu-id="948c3-222">Hej andra `simulatedData`, vilket är ett booleskt värde för om toouse simulerade sensordata eller inte.</span><span class="sxs-lookup"><span data-stu-id="948c3-222">hello second one `simulatedData`,which is a Boolean value for whether toouse simulated sensor data or not.</span></span>

   <span data-ttu-id="948c3-223">Om du **saknar hello sensor**, Ställ in hello `simulatedData` värdet för`true` toomake hello exempelprogrammet skapa och använda simulerade sensordata.</span><span class="sxs-lookup"><span data-stu-id="948c3-223">If you **don't have hello sensor**, set hello `simulatedData` value too`true` toomake hello sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="948c3-224">Spara och avsluta genom att trycka på CTRL-O > Ange > kontrollen X.</span><span class="sxs-lookup"><span data-stu-id="948c3-224">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="run-hello-sample-application"></a><span data-ttu-id="948c3-225">Köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="948c3-225">Run hello sample application</span></span>

1. <span data-ttu-id="948c3-226">Kör exempelprogrammet hello genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="948c3-226">Run hello sample application by running hello following command:</span></span>

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   <span data-ttu-id="948c3-227">Kontrollera att du kopiera / klistra in anslutningssträngen för hello enhet till hello enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="948c3-227">Make sure you copy-paste hello device connection string into hello single quotes.</span></span>


<span data-ttu-id="948c3-228">Du bör se hello följande utdata som visar hello sensor data och hello meddelanden som skickas tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="948c3-228">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub.</span></span>

![Utdata - sensordata som skickas från hallon Pi tooyour IoT-hubb](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a><span data-ttu-id="948c3-230">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="948c3-230">Next steps</span></span>

<span data-ttu-id="948c3-231">Du har kört ett program toocollect sensor exempeldata och skicka den tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="948c3-231">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]