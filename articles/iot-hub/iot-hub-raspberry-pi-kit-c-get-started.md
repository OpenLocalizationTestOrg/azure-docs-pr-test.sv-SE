---
title: aaaRaspberry Pi toocloud (C) - ansluta hallon Pi tooAzure IoT-hubb | Microsoft Docs
description: "Lär dig hur toosetup och ansluta hallon Pi tooAzure IoT-hubb för hallon Pi toosend data toohello Azure-molnplattform i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure iot raspberry pi, raspberry pi iot hub, raspberry pi skicka data toocloud, raspberry pi toocloud
ms.assetid: 68c0e730-1dc8-4e26-ac6b-573b217b302d
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/12/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05086890458e196d7fdc87a53fcabb9386245d6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-c"></a><span data-ttu-id="0df8a-104">Ansluta hallon Pi tooAzure IoT Hub (C)</span><span class="sxs-lookup"><span data-stu-id="0df8a-104">Connect Raspberry Pi tooAzure IoT Hub (C)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="0df8a-105">I den här kursen kan du börja genom att lära dig hello grunderna i att arbeta med hallon Pi med Raspbian.</span><span class="sxs-lookup"><span data-stu-id="0df8a-105">In this tutorial, you begin by learning hello basics of working with Raspberry Pi that's running Raspbian.</span></span> <span data-ttu-id="0df8a-106">Du lär dig hur tooseamlessly ansluta enheter toohello molnet med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="0df8a-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> <span data-ttu-id="0df8a-107">För Windows 10 IoT Core prover gå toohello [Windows Dev Center](http://www.windowsondevices.com/).</span><span class="sxs-lookup"><span data-stu-id="0df8a-107">For Windows 10 IoT Core samples, go toohello [Windows Dev Center](http://www.windowsondevices.com/).</span></span>

<span data-ttu-id="0df8a-108">Har du en kit ännu?</span><span class="sxs-lookup"><span data-stu-id="0df8a-108">Don't have a kit yet?</span></span> <span data-ttu-id="0df8a-109">Försök [hallon Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0df8a-109">Try [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md).</span></span> <span data-ttu-id="0df8a-110">Eller köp en ny kit [här](https://azure.microsoft.com/develop/iot/starter-kits).</span><span class="sxs-lookup"><span data-stu-id="0df8a-110">Or buy a new kit [here](https://azure.microsoft.com/develop/iot/starter-kits).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="0df8a-111">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="0df8a-111">What you do</span></span>

* <span data-ttu-id="0df8a-112">Skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0df8a-112">Create an IoT hub.</span></span>
* <span data-ttu-id="0df8a-113">Registrera en enhet för Pi i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0df8a-113">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="0df8a-114">Konfigurera Raspberry Pi.</span><span class="sxs-lookup"><span data-stu-id="0df8a-114">Setup Raspberry Pi.</span></span>
* <span data-ttu-id="0df8a-115">Kör ett exempelprogram på Pi toosend sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0df8a-115">Run a sample application on Pi toosend sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="0df8a-116">Ansluta hallon Pi tooan IoT-hubb som du skapar.</span><span class="sxs-lookup"><span data-stu-id="0df8a-116">Connect Raspberry Pi tooan IoT hub that you create.</span></span> <span data-ttu-id="0df8a-117">Sedan kör du ett exempelprogram på Pi toocollect temperatur- och fuktighetskonsekvens data från en BME280 sensor.</span><span class="sxs-lookup"><span data-stu-id="0df8a-117">Then you run a sample application on Pi toocollect temperature and humidity data from a BME280 sensor.</span></span> <span data-ttu-id="0df8a-118">Slutligen kan skicka du hello sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0df8a-118">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="0df8a-119">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="0df8a-119">What you learn</span></span>

* <span data-ttu-id="0df8a-120">Hur toocreate en Azure IoT-hubb och få nya enheten anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="0df8a-120">How toocreate an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="0df8a-121">Hur tooconnect Pi med en BME280 sensor.</span><span class="sxs-lookup"><span data-stu-id="0df8a-121">How tooconnect Pi with a BME280 sensor.</span></span>
* <span data-ttu-id="0df8a-122">Hur toocollect sensordata genom att köra ett exempelprogram på Pi.</span><span class="sxs-lookup"><span data-stu-id="0df8a-122">How toocollect sensor data by running a sample application on Pi.</span></span>
* <span data-ttu-id="0df8a-123">Hur toosend sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0df8a-123">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="0df8a-124">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="0df8a-124">What you need</span></span>

![Vad du behöver](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* <span data-ttu-id="0df8a-126">hello hallon Pi 2 eller 3 för hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="0df8a-126">hello Raspberry Pi 2 or Raspberry Pi 3 board.</span></span>
* <span data-ttu-id="0df8a-127">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0df8a-127">An active Azure subscription.</span></span> <span data-ttu-id="0df8a-128">Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="0df8a-128">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="0df8a-129">En Övervakare, en USB-tangentbord och mus som ansluter tooPi.</span><span class="sxs-lookup"><span data-stu-id="0df8a-129">A monitor, a USB keyboard, and mouse that connect tooPi.</span></span>
* <span data-ttu-id="0df8a-130">En Mac- eller en dator som kör Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="0df8a-130">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="0df8a-131">En Internetanslutning.</span><span class="sxs-lookup"><span data-stu-id="0df8a-131">An Internet connection.</span></span>
* <span data-ttu-id="0df8a-132">16 GB eller högre microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="0df8a-132">A 16 GB or above microSD card.</span></span>
* <span data-ttu-id="0df8a-133">En USB-SD-kort eller microSD-kort tooburn hello operativsystemavbildning på hello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="0df8a-133">A USB-SD adapter or microSD card tooburn hello operating system image onto hello microSD card.</span></span>
* <span data-ttu-id="0df8a-134">En v 5-2-amp elkälla med hello 6 fotavtryck micro USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="0df8a-134">A 5-volt 2-amp power supply with hello 6-foot micro USB cable.</span></span>

<span data-ttu-id="0df8a-135">hello följande objekt är valfria:</span><span class="sxs-lookup"><span data-stu-id="0df8a-135">hello following items are optional:</span></span>

* <span data-ttu-id="0df8a-136">En monterad Adafruit BME280 temperatur, tryck och fuktighet sensorn.</span><span class="sxs-lookup"><span data-stu-id="0df8a-136">An assembled Adafruit BME280 temperature, pressure, and humidity sensor.</span></span>
* <span data-ttu-id="0df8a-137">En breadboard.</span><span class="sxs-lookup"><span data-stu-id="0df8a-137">A breadboard.</span></span>
* <span data-ttu-id="0df8a-138">6-F/M omkopplare kablar.</span><span class="sxs-lookup"><span data-stu-id="0df8a-138">6 F/M jumper wires.</span></span>
* <span data-ttu-id="0df8a-139">En indirekt Indikator för 10 mm.</span><span class="sxs-lookup"><span data-stu-id="0df8a-139">A diffused 10-mm LED.</span></span>


> [!NOTE] 
<span data-ttu-id="0df8a-140">Objekten är valfritt eftersom hello kod exempel support simulerade sensordata.</span><span class="sxs-lookup"><span data-stu-id="0df8a-140">These items are optional because hello code sample support simulated sensor data.</span></span>


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a><span data-ttu-id="0df8a-141">Konfigurera Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="0df8a-141">Setup Raspberry Pi</span></span>

### <a name="install-hello-raspbian-operating-system-for-pi"></a><span data-ttu-id="0df8a-142">Installera operativsystemet för hello Raspbian för Pi</span><span class="sxs-lookup"><span data-stu-id="0df8a-142">Install hello Raspbian operating system for Pi</span></span>

<span data-ttu-id="0df8a-143">Förbered hello microSD-kort för installation av hello Raspbian bild.</span><span class="sxs-lookup"><span data-stu-id="0df8a-143">Prepare hello microSD card for installation of hello Raspbian image.</span></span>

1. <span data-ttu-id="0df8a-144">Hämta Raspbian.</span><span class="sxs-lookup"><span data-stu-id="0df8a-144">Download Raspbian.</span></span>
   1. <span data-ttu-id="0df8a-145">[Hämta Raspbian Jessie med skrivbordet](https://www.raspberrypi.org/downloads/raspbian/) (hello ZIP-fil).</span><span class="sxs-lookup"><span data-stu-id="0df8a-145">[Download Raspbian Jessie with Desktop](https://www.raspberrypi.org/downloads/raspbian/) (hello .zip file).</span></span>
   1. <span data-ttu-id="0df8a-146">Extrahera hello Raspbian bild tooa mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="0df8a-146">Extract hello Raspbian image tooa folder on your computer.</span></span>
1. <span data-ttu-id="0df8a-147">Installera Raspbian toohello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="0df8a-147">Install Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="0df8a-148">[Hämta och installera hello Etcher SD-kort brännare verktyget](https://etcher.io/).</span><span class="sxs-lookup"><span data-stu-id="0df8a-148">[Download and install hello Etcher SD card burner utility](https://etcher.io/).</span></span>
   1. <span data-ttu-id="0df8a-149">Kör Etcher och välj hello Raspbian avbildning som du extraherade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="0df8a-149">Run Etcher and select hello Raspbian image that you extracted in step 1.</span></span>
   1. <span data-ttu-id="0df8a-150">Välj enhet för hello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="0df8a-150">Select hello microSD card drive.</span></span> <span data-ttu-id="0df8a-151">Observera att Etcher kanske redan har valt hello rätt enhet.</span><span class="sxs-lookup"><span data-stu-id="0df8a-151">Note that Etcher may have already selected hello correct drive.</span></span>
   1. <span data-ttu-id="0df8a-152">Klicka på Flash tooinstall Raspbian toohello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="0df8a-152">Click Flash tooinstall Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="0df8a-153">Ta bort hello microSD-kort från datorn när installationen är klar.</span><span class="sxs-lookup"><span data-stu-id="0df8a-153">Remove hello microSD card from your computer when installation is complete.</span></span> <span data-ttu-id="0df8a-154">Det är säker tooremove hello microSD-kort direkt eftersom Etcher automatiskt matar ut eller demonterar hello microSD-kort när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="0df8a-154">It's safe tooremove hello microSD card directly because Etcher automatically ejects or unmounts hello microSD card upon completion.</span></span>
   1. <span data-ttu-id="0df8a-155">Infoga hello microSD-kort i Pi.</span><span class="sxs-lookup"><span data-stu-id="0df8a-155">Insert hello microSD card into Pi.</span></span>

### <a name="enable-ssh-and-spi"></a><span data-ttu-id="0df8a-156">Aktivera SSH och SPI</span><span class="sxs-lookup"><span data-stu-id="0df8a-156">Enable SSH and SPI</span></span>

1. <span data-ttu-id="0df8a-157">Ansluta Pi toohello bildskärm, tangentbord och mus, starta Pi och logga sedan in Raspbian med hjälp av `pi` som hello användarnamn och `raspberry` som hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="0df8a-157">Connect Pi toohello monitor, keyboard and mouse, start Pi and then log in Raspbian by using `pi` as hello user name and `raspberry` as hello password.</span></span>
1. <span data-ttu-id="0df8a-158">Klicka på hello Raspberry ikonen > **inställningar** > **hallon Pi Configuration**.</span><span class="sxs-lookup"><span data-stu-id="0df8a-158">Click hello Raspberry icon > **Preferences** > **Raspberry Pi Configuration**.</span></span>

   ![Hej Raspbian Inställningar-menyn](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. <span data-ttu-id="0df8a-160">På hello **gränssnitt** ställer du in **SPI** och **SSH** för**aktivera**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0df8a-160">On hello **Interfaces** tab, set **SPI** and **SSH** too**Enable**, and then click **OK**.</span></span> <span data-ttu-id="0df8a-161">Om du inte har fysisk sensorer och vill toouse simulerade sensordata, är det här steget valfritt.</span><span class="sxs-lookup"><span data-stu-id="0df8a-161">If you don't have physical sensors and want toouse simulated sensor data, this step is optional.</span></span>

   ![Aktivera SPI och SSH på Raspberry Pi](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
<span data-ttu-id="0df8a-163">tooenable SSH och SPI hittar du mer referensdokument på [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) och [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).</span><span class="sxs-lookup"><span data-stu-id="0df8a-163">tooenable SSH and SPI, you can find more reference documents on [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) and [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).</span></span>

### <a name="connect-hello-sensor-toopi"></a><span data-ttu-id="0df8a-164">Ansluta hello sensor tooPi</span><span class="sxs-lookup"><span data-stu-id="0df8a-164">Connect hello sensor tooPi</span></span>

<span data-ttu-id="0df8a-165">Använd hello breadboard och omkopplare kablar tooconnect en Indikator och en BME280 tooPi på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="0df8a-165">Use hello breadboard and jumper wires tooconnect an LED and a BME280 tooPi as follows.</span></span> <span data-ttu-id="0df8a-166">Om du inte har hello sensor [hoppa över det här avsnittet](#connect-pi-to-the-network).</span><span class="sxs-lookup"><span data-stu-id="0df8a-166">If you don’t have hello sensor, [skip this section](#connect-pi-to-the-network).</span></span>

![hello hallon Pi och sensor-anslutning](media/iot-hub-raspberry-pi-kit-c-get-started/3_raspberry-pi-sensor-connection.png)

<span data-ttu-id="0df8a-168">Hej BME280 sensor kan samla in data för temperatur- och fuktighetskonsekvens.</span><span class="sxs-lookup"><span data-stu-id="0df8a-168">hello BME280 sensor can collect temperature and humidity data.</span></span> <span data-ttu-id="0df8a-169">Och hello Indikator blinkar om det finns en kommunikation mellan enheten och hello moln.</span><span class="sxs-lookup"><span data-stu-id="0df8a-169">And hello LED will blink if there is a communication between device and hello cloud.</span></span> 

<span data-ttu-id="0df8a-170">För sensor PIN-koder, använder du följande kablar hello:</span><span class="sxs-lookup"><span data-stu-id="0df8a-170">For sensor pins, use hello following wiring:</span></span>

| <span data-ttu-id="0df8a-171">Starta (Sensor & Indikator)</span><span class="sxs-lookup"><span data-stu-id="0df8a-171">Start (Sensor & LED)</span></span>     | <span data-ttu-id="0df8a-172">END (moderkort)</span><span class="sxs-lookup"><span data-stu-id="0df8a-172">End (Board)</span></span>            | <span data-ttu-id="0df8a-173">Kabel färg</span><span class="sxs-lookup"><span data-stu-id="0df8a-173">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="0df8a-174">Indikator VDD (PIN-kod 5G)</span><span class="sxs-lookup"><span data-stu-id="0df8a-174">LED VDD (Pin 5G)</span></span>         | <span data-ttu-id="0df8a-175">GPIO 4 (PIN-kod 7)</span><span class="sxs-lookup"><span data-stu-id="0df8a-175">GPIO 4 (Pin 7)</span></span>         | <span data-ttu-id="0df8a-176">Vita kabeln</span><span class="sxs-lookup"><span data-stu-id="0df8a-176">White cable</span></span>   |
| <span data-ttu-id="0df8a-177">Indikator jord (PIN-kod 6G)</span><span class="sxs-lookup"><span data-stu-id="0df8a-177">LED GND (Pin 6G)</span></span>         | <span data-ttu-id="0df8a-178">JORD (PIN-kod 6)</span><span class="sxs-lookup"><span data-stu-id="0df8a-178">GND (Pin 6)</span></span>            | <span data-ttu-id="0df8a-179">Svart kabel</span><span class="sxs-lookup"><span data-stu-id="0df8a-179">Black cable</span></span>   |
| <span data-ttu-id="0df8a-180">VDD (PIN-kod 18F)</span><span class="sxs-lookup"><span data-stu-id="0df8a-180">VDD (Pin 18F)</span></span>            | <span data-ttu-id="0df8a-181">3, 3v PWR (PIN-kod 17)</span><span class="sxs-lookup"><span data-stu-id="0df8a-181">3.3V PWR (Pin 17)</span></span>      | <span data-ttu-id="0df8a-182">Vita kabeln</span><span class="sxs-lookup"><span data-stu-id="0df8a-182">White cable</span></span>   |
| <span data-ttu-id="0df8a-183">JORD (PIN-kod 20F)</span><span class="sxs-lookup"><span data-stu-id="0df8a-183">GND (Pin 20F)</span></span>            | <span data-ttu-id="0df8a-184">JORD (PIN-kod 20)</span><span class="sxs-lookup"><span data-stu-id="0df8a-184">GND (Pin 20)</span></span>           | <span data-ttu-id="0df8a-185">Svart kabel</span><span class="sxs-lookup"><span data-stu-id="0df8a-185">Black cable</span></span>   |
| <span data-ttu-id="0df8a-186">SCK (PIN-kod 21F)</span><span class="sxs-lookup"><span data-stu-id="0df8a-186">SCK (Pin 21F)</span></span>            | <span data-ttu-id="0df8a-187">SPI0 SCLK (PIN-kod 23)</span><span class="sxs-lookup"><span data-stu-id="0df8a-187">SPI0 SCLK (Pin 23)</span></span>     | <span data-ttu-id="0df8a-188">Orange kabel</span><span class="sxs-lookup"><span data-stu-id="0df8a-188">Orange cable</span></span>  |
| <span data-ttu-id="0df8a-189">SDO (PIN-kod 22F)</span><span class="sxs-lookup"><span data-stu-id="0df8a-189">SDO (Pin 22F)</span></span>            | <span data-ttu-id="0df8a-190">SPI0 MISO (PIN-kod 21)</span><span class="sxs-lookup"><span data-stu-id="0df8a-190">SPI0 MISO (Pin 21)</span></span>     | <span data-ttu-id="0df8a-191">Gult kabel</span><span class="sxs-lookup"><span data-stu-id="0df8a-191">Yellow cable</span></span>  |
| <span data-ttu-id="0df8a-192">SDI (PIN-kod 23F)</span><span class="sxs-lookup"><span data-stu-id="0df8a-192">SDI (Pin 23F)</span></span>            | <span data-ttu-id="0df8a-193">SPI0 MOSI (PIN-kod 19)</span><span class="sxs-lookup"><span data-stu-id="0df8a-193">SPI0 MOSI (Pin 19)</span></span>     | <span data-ttu-id="0df8a-194">Grön kabel</span><span class="sxs-lookup"><span data-stu-id="0df8a-194">Green cable</span></span>   |
| <span data-ttu-id="0df8a-195">CS (PIN-kod 24F)</span><span class="sxs-lookup"><span data-stu-id="0df8a-195">CS (Pin 24F)</span></span>             | <span data-ttu-id="0df8a-196">SPI0 CS (PIN-kod 24)</span><span class="sxs-lookup"><span data-stu-id="0df8a-196">SPI0 CS (Pin 24)</span></span>       | <span data-ttu-id="0df8a-197">Blå kabeln</span><span class="sxs-lookup"><span data-stu-id="0df8a-197">Blue cable</span></span>    |

<span data-ttu-id="0df8a-198">Klicka på tooview [hallon Pi 2 och 3 PIN-kod mappningar](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) referens.</span><span class="sxs-lookup"><span data-stu-id="0df8a-198">Click tooview [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.</span></span>

<span data-ttu-id="0df8a-199">När du har anslutit BME280 tooyour hallon Pi, ska den som under bilden.</span><span class="sxs-lookup"><span data-stu-id="0df8a-199">After you've successfully connected BME280 tooyour Raspberry Pi, it should be like below image.</span></span>

![Anslutna Pi och BME280](media/iot-hub-raspberry-pi-kit-c-get-started/4_connected-pi.jpg)

### <a name="connect-pi-toohello-network"></a><span data-ttu-id="0df8a-201">Ansluta Pi toohello nätverk</span><span class="sxs-lookup"><span data-stu-id="0df8a-201">Connect Pi toohello network</span></span>

<span data-ttu-id="0df8a-202">Aktivera Pi med hjälp av hello micro USB-kabel och hello strömförsörjning.</span><span class="sxs-lookup"><span data-stu-id="0df8a-202">Turn on Pi by using hello micro USB cable and hello power supply.</span></span> <span data-ttu-id="0df8a-203">Använd hello Ethernet-kabel tooconnect Pi tooyour kabelanslutna nätverk eller följa hello [instruktioner från hello hallon Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour trådlöst nätverk.</span><span class="sxs-lookup"><span data-stu-id="0df8a-203">Use hello Ethernet cable tooconnect Pi tooyour wired network or follow hello [instructions from hello Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour wireless network.</span></span> <span data-ttu-id="0df8a-204">När din Pi har varit ansluten toohello nätverk, måste tootake anteckna hello [IP-adressen för din Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span><span class="sxs-lookup"><span data-stu-id="0df8a-204">After your Pi has been successfully connected toohello network, you need tootake a note of hello [IP address of your Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span></span>

![Anslutna toowired nätverk](media/iot-hub-raspberry-pi-kit-c-get-started/5_power-on-pi.jpg)


## <a name="run-a-sample-application-on-pi"></a><span data-ttu-id="0df8a-206">Kör ett exempelprogram på Pi</span><span class="sxs-lookup"><span data-stu-id="0df8a-206">Run a sample application on Pi</span></span>

### <a name="install-hello-prerequisite-packages"></a><span data-ttu-id="0df8a-207">Installera nödvändiga hello-paket</span><span class="sxs-lookup"><span data-stu-id="0df8a-207">Install hello prerequisite packages</span></span>

1. <span data-ttu-id="0df8a-208">Använd någon av följande SSH-klienter från din värd datorn tooconnect tooyour hallon Pi hello.</span><span class="sxs-lookup"><span data-stu-id="0df8a-208">Use one of hello following SSH clients from your host computer tooconnect tooyour Raspberry Pi.</span></span>
   
   <span data-ttu-id="0df8a-209">**Windows-användare**</span><span class="sxs-lookup"><span data-stu-id="0df8a-209">**Windows Users**</span></span>
   1. <span data-ttu-id="0df8a-210">Hämta och installera [PuTTY](http://www.putty.org/) för Windows.</span><span class="sxs-lookup"><span data-stu-id="0df8a-210">Download and install [PuTTY](http://www.putty.org/) for Windows.</span></span> 
   1. <span data-ttu-id="0df8a-211">Kopiera hello IP-adressen för ditt avsnitt Pi till hello värden namn (eller IP-adress) och välj SSH som hello anslutningstypen.</span><span class="sxs-lookup"><span data-stu-id="0df8a-211">Copy hello IP address of your Pi into hello Host name (or IP address) section and select SSH as hello connection type.</span></span>
   
   ![PuTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   <span data-ttu-id="0df8a-213">**Mac- och Ubuntu användare**</span><span class="sxs-lookup"><span data-stu-id="0df8a-213">**Mac and Ubuntu Users**</span></span>
   
   <span data-ttu-id="0df8a-214">Använd hello inbyggda SSH-klienten på Ubuntu eller macOS.</span><span class="sxs-lookup"><span data-stu-id="0df8a-214">Use hello built-in SSH client on Ubuntu or macOS.</span></span> <span data-ttu-id="0df8a-215">Du kan behöva toorun `ssh pi@<ip address of pi>` tooconnect Pi via SSH.</span><span class="sxs-lookup"><span data-stu-id="0df8a-215">You might need toorun `ssh pi@<ip address of pi>` tooconnect Pi via SSH.</span></span>
   > [!NOTE] 
   <span data-ttu-id="0df8a-216">hello Standardanvändarnamnet är `pi` , och hello lösenordet är `raspberry`.</span><span class="sxs-lookup"><span data-stu-id="0df8a-216">hello default username is `pi` , and hello password is `raspberry`.</span></span>

1. <span data-ttu-id="0df8a-217">Installera nödvändiga hello-paket för hello Microsoft Azure IoT-enhet SDK för C och Cmake genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="0df8a-217">Install hello prerequisite packages for hello Microsoft Azure IoT Device SDK for C and Cmake by running hello following commands:</span></span>

   ```bash
   grep -q -F 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
   grep -q -F 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
   sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys FDA6A393E4C2257F
   sudo apt-get update
   sudo apt-get install -y azure-iot-sdk-c-dev cmake libcurl4-openssl-dev git-core
   git clone git://git.drogon.net/wiringPi
   cd ./wiringPi
   ./build
   ```


### <a name="configure-hello-sample-application"></a><span data-ttu-id="0df8a-218">Konfigurera hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="0df8a-218">Configure hello sample application</span></span>

1. <span data-ttu-id="0df8a-219">Klona hello exempelprogrammet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0df8a-219">Clone hello sample application by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-client-app
   ```
1. <span data-ttu-id="0df8a-220">Öppna hello config-fil genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="0df8a-220">Open hello config file by running hello following commands:</span></span>

   ```bash
   cd iot-hub-c-raspberrypi-client-app
   nano config.h
   ```

   ![Config-fil](media/iot-hub-raspberry-pi-kit-c-get-started/6_config-file.png)

   <span data-ttu-id="0df8a-222">Det finns två makron i den här filen kan du configurate.</span><span class="sxs-lookup"><span data-stu-id="0df8a-222">There are two macros in this file you can configurate.</span></span> <span data-ttu-id="0df8a-223">hello först en är `INTERVAL`, som definierar hello tidsintervall (i millisekunder) mellan två meddelanden som skickar toocloud.</span><span class="sxs-lookup"><span data-stu-id="0df8a-223">hello first one is `INTERVAL`, which defines hello time interval (in milliseconds) between two messages that send toocloud.</span></span> <span data-ttu-id="0df8a-224">Hej andra `SIMULATED_DATA`, vilket är ett booleskt värde för om toouse simulerade sensordata eller inte.</span><span class="sxs-lookup"><span data-stu-id="0df8a-224">hello second one `SIMULATED_DATA`,which is a Boolean value for whether toouse simulated sensor data or not.</span></span>

   <span data-ttu-id="0df8a-225">Om du **saknar hello sensor**, Ställ in hello `SIMULATED_DATA` värdet för`1` toomake hello exempelprogrammet skapa och använda simulerade sensordata.</span><span class="sxs-lookup"><span data-stu-id="0df8a-225">If you **don't have hello sensor**, set hello `SIMULATED_DATA` value too`1` toomake hello sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="0df8a-226">Spara och avsluta genom att trycka på CTRL-O > Ange > kontrollen X.</span><span class="sxs-lookup"><span data-stu-id="0df8a-226">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="build-and-run-hello-sample-application"></a><span data-ttu-id="0df8a-227">Skapa och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="0df8a-227">Build and run hello sample application</span></span>

1. <span data-ttu-id="0df8a-228">Skapa hello exempelprogrammet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0df8a-228">Build hello sample application by running hello following command:</span></span>

   ```bash
   cmake . && make
   ```
   ![Skapa utdata](media/iot-hub-raspberry-pi-kit-c-get-started/7_build-output.png)

1. <span data-ttu-id="0df8a-230">Kör exempelprogrammet hello genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0df8a-230">Run hello sample application by running hello following command:</span></span>

   ```bash
   sudo ./app '<DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   <span data-ttu-id="0df8a-231">Kontrollera att du kopiera / klistra in anslutningssträngen för hello enhet till hello enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="0df8a-231">Make sure you copy-paste hello device connection string into hello single quotes.</span></span>


<span data-ttu-id="0df8a-232">Du bör se hello följande utdata som visar hello sensor data och hello meddelanden som skickas tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0df8a-232">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub.</span></span>

![Utdata - sensordata som skickas från hallon Pi tooyour IoT-hubb](media/iot-hub-raspberry-pi-kit-c-get-started/8_run-output.png)

## <a name="next-steps"></a><span data-ttu-id="0df8a-234">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0df8a-234">Next steps</span></span>

<span data-ttu-id="0df8a-235">Du har kört ett program toocollect sensor exempeldata och skicka den tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0df8a-235">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span> <span data-ttu-id="0df8a-236">toosee hälsningsmeddelande som din hallon Pi skickats tooyour IoT-hubb eller skicka meddelanden tooyour hallon Pi i ett kommandoradsgränssnitt finns hello [hantera moln enhet meddelanden med iothub explorer kursen](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span><span class="sxs-lookup"><span data-stu-id="0df8a-236">toosee hello messages that your Raspberry Pi has sent tooyour IoT hub or send messages tooyour Raspberry Pi in a command line interface, see hello [Manage cloud device messaging with iothub-explorer tutorial](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
