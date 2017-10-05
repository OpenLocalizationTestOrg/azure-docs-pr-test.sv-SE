---
title: "Raspberry Pi till molnet (Node.js) – ansluta hallon Pi till Azure IoT Hub | Microsoft Docs"
description: "Lär dig mer om att konfigurera och ansluta hallon Pi till Azure IoT-hubb för hallon Pi att skicka data till Azure-molnplattform i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure iot raspberry pi, raspberry pi iot-hubb, raspberry pi skickar data till molnet, raspberry pi till molnet
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/27/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f48c4bd27b1df1d02090ed51172f943e50c76c3e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-nodejs"></a><span data-ttu-id="80d70-104">Ansluta Raspberry Pi till Azure IoT-hubb (Node.js)</span><span class="sxs-lookup"><span data-stu-id="80d70-104">Connect Raspberry Pi to Azure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="80d70-105">I den här kursen kan du börja lära dig grunderna i att arbeta med hallon Pi med Raspbian.</span><span class="sxs-lookup"><span data-stu-id="80d70-105">In this tutorial, you begin by learning the basics of working with Raspberry Pi that's running Raspbian.</span></span> <span data-ttu-id="80d70-106">Du lär dig sedan sömlöst ansluter enheterna till molnet med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="80d70-106">You then learn how to seamlessly connect your devices to the cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> <span data-ttu-id="80d70-107">För Windows 10 IoT Core sampel, gå till den [Windows Dev Center](http://www.windowsondevices.com/).</span><span class="sxs-lookup"><span data-stu-id="80d70-107">For Windows 10 IoT Core samples, go to the [Windows Dev Center](http://www.windowsondevices.com/).</span></span>

<span data-ttu-id="80d70-108">Har du en kit ännu?</span><span class="sxs-lookup"><span data-stu-id="80d70-108">Don't have a kit yet?</span></span> <span data-ttu-id="80d70-109">Försök [hallon Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="80d70-109">Try [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md).</span></span> <span data-ttu-id="80d70-110">Eller köp en ny kit [här](https://azure.microsoft.com/develop/iot/starter-kits).</span><span class="sxs-lookup"><span data-stu-id="80d70-110">Or buy a new kit [here](https://azure.microsoft.com/develop/iot/starter-kits).</span></span>


## <a name="what-you-do"></a><span data-ttu-id="80d70-111">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="80d70-111">What you do</span></span>

* <span data-ttu-id="80d70-112">Skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80d70-112">Create an IoT hub.</span></span>
* <span data-ttu-id="80d70-113">Registrera en enhet för Pi i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80d70-113">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="80d70-114">Konfigurera Raspberry Pi.</span><span class="sxs-lookup"><span data-stu-id="80d70-114">Setup Raspberry Pi.</span></span>
* <span data-ttu-id="80d70-115">Kör ett exempelprogram på Pi sensordata ska skickas till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80d70-115">Run a sample application on Pi to send sensor data to your IoT hub.</span></span>

<span data-ttu-id="80d70-116">Ansluta hallon Pi till en IoT-hubb som du skapar.</span><span class="sxs-lookup"><span data-stu-id="80d70-116">Connect Raspberry Pi to an IoT hub that you create.</span></span> <span data-ttu-id="80d70-117">Sedan kör du ett exempelprogram på Pi för att samla in data för temperatur- och fuktighetskonsekvens från en BME280 sensor.</span><span class="sxs-lookup"><span data-stu-id="80d70-117">Then you run a sample application on Pi to collect temperature and humidity data from a BME280 sensor.</span></span> <span data-ttu-id="80d70-118">Slutligen kan du skicka dessa sensordata för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80d70-118">Finally, you send the sensor data to your IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="80d70-119">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="80d70-119">What you learn</span></span>

* <span data-ttu-id="80d70-120">Hur du skapar en Azure IoT-hubb och hämta din nya anslutningssträngen för enheten.</span><span class="sxs-lookup"><span data-stu-id="80d70-120">How to create an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="80d70-121">Så här ansluter du Pi med en BME280 sensor.</span><span class="sxs-lookup"><span data-stu-id="80d70-121">How to connect Pi with a BME280 sensor.</span></span>
* <span data-ttu-id="80d70-122">Hur du samlar in sensordata genom att köra ett exempelprogram på Pi.</span><span class="sxs-lookup"><span data-stu-id="80d70-122">How to collect sensor data by running a sample application on Pi.</span></span>
* <span data-ttu-id="80d70-123">Hur du skickar sensordata till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80d70-123">How to send sensor data to your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="80d70-124">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="80d70-124">What you need</span></span>

![Vad du behöver](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* <span data-ttu-id="80d70-126">Hallon Pi 2 eller 3 för hallon Pi-kort.</span><span class="sxs-lookup"><span data-stu-id="80d70-126">The Raspberry Pi 2 or Raspberry Pi 3 board.</span></span>
* <span data-ttu-id="80d70-127">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="80d70-127">An active Azure subscription.</span></span> <span data-ttu-id="80d70-128">Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="80d70-128">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="80d70-129">En Övervakare, en USB-tangentbord och mus som ansluter till Pi.</span><span class="sxs-lookup"><span data-stu-id="80d70-129">A monitor, a USB keyboard, and mouse that connect to Pi.</span></span>
* <span data-ttu-id="80d70-130">En Mac- eller en dator som kör Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="80d70-130">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="80d70-131">En Internetanslutning.</span><span class="sxs-lookup"><span data-stu-id="80d70-131">An Internet connection.</span></span>
* <span data-ttu-id="80d70-132">16 GB eller högre microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="80d70-132">A 16 GB or above microSD card.</span></span>
* <span data-ttu-id="80d70-133">En USB-SD-kort eller microSD-kort att bränna operativsystemavbildning på microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="80d70-133">A USB-SD adapter or microSD card to burn the operating system image onto the microSD card.</span></span>
* <span data-ttu-id="80d70-134">En v 5-2-amp elkälla med 6 fotavtryck micro USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="80d70-134">A 5-volt 2-amp power supply with the 6-foot micro USB cable.</span></span>

<span data-ttu-id="80d70-135">Följande objekt är valfria:</span><span class="sxs-lookup"><span data-stu-id="80d70-135">The following items are optional:</span></span>

* <span data-ttu-id="80d70-136">En monterad Adafruit BME280 temperatur, tryck och fuktighet sensorn.</span><span class="sxs-lookup"><span data-stu-id="80d70-136">An assembled Adafruit BME280 temperature, pressure, and humidity sensor.</span></span>
* <span data-ttu-id="80d70-137">En breadboard.</span><span class="sxs-lookup"><span data-stu-id="80d70-137">A breadboard.</span></span>
* <span data-ttu-id="80d70-138">6-F/M omkopplare kablar.</span><span class="sxs-lookup"><span data-stu-id="80d70-138">6 F/M jumper wires.</span></span>
* <span data-ttu-id="80d70-139">En indirekt Indikator för 10 mm.</span><span class="sxs-lookup"><span data-stu-id="80d70-139">A diffused 10-mm LED.</span></span>


> [!NOTE] 
<span data-ttu-id="80d70-140">Objekten är valfritt eftersom kod exempel support simulerade sensordata.</span><span class="sxs-lookup"><span data-stu-id="80d70-140">These items are optional because the code sample support simulated sensor data.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a><span data-ttu-id="80d70-141">Konfigurera Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="80d70-141">Setup Raspberry Pi</span></span>

### <a name="install-the-raspbian-operating-system-for-pi"></a><span data-ttu-id="80d70-142">Installera operativsystemet Raspbian för Pi</span><span class="sxs-lookup"><span data-stu-id="80d70-142">Install the Raspbian operating system for Pi</span></span>

<span data-ttu-id="80d70-143">Förbered microSD-kort för installation av Raspbian bilden.</span><span class="sxs-lookup"><span data-stu-id="80d70-143">Prepare the microSD card for installation of the Raspbian image.</span></span>

1. <span data-ttu-id="80d70-144">Hämta Raspbian.</span><span class="sxs-lookup"><span data-stu-id="80d70-144">Download Raspbian.</span></span>
   1. <span data-ttu-id="80d70-145">[Hämta Raspbian Jessie med skrivbordet](https://www.raspberrypi.org/downloads/raspbian/) (.zip-fil).</span><span class="sxs-lookup"><span data-stu-id="80d70-145">[Download Raspbian Jessie with Desktop](https://www.raspberrypi.org/downloads/raspbian/) (the .zip file).</span></span>
   1. <span data-ttu-id="80d70-146">Extrahera Raspbian-avbildning till en mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="80d70-146">Extract the Raspbian image to a folder on your computer.</span></span>
1. <span data-ttu-id="80d70-147">Installera Raspbian microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="80d70-147">Install Raspbian to the microSD card.</span></span>
   1. <span data-ttu-id="80d70-148">[Hämta och installera verktyget Etcher SD-kort brännare](https://etcher.io/).</span><span class="sxs-lookup"><span data-stu-id="80d70-148">[Download and install the Etcher SD card burner utility](https://etcher.io/).</span></span>
   1. <span data-ttu-id="80d70-149">Kör Etcher och välj Raspbian bilden som du extraherade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="80d70-149">Run Etcher and select the Raspbian image that you extracted in step 1.</span></span>
   1. <span data-ttu-id="80d70-150">Välj enhet för microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="80d70-150">Select the microSD card drive.</span></span> <span data-ttu-id="80d70-151">Observera att Etcher kanske redan har valt rätt enhet.</span><span class="sxs-lookup"><span data-stu-id="80d70-151">Note that Etcher may have already selected the correct drive.</span></span>
   1. <span data-ttu-id="80d70-152">Klicka på Flash för att installera Raspbian till microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="80d70-152">Click Flash to install Raspbian to the microSD card.</span></span>
   1. <span data-ttu-id="80d70-153">Ta bort microSD-kort från datorn när installationen är klar.</span><span class="sxs-lookup"><span data-stu-id="80d70-153">Remove the microSD card from your computer when installation is complete.</span></span> <span data-ttu-id="80d70-154">Det är säkert att ta bort microSD-kort direkt eftersom Etcher automatiskt matar ut eller demonterar microSD-kort när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="80d70-154">It's safe to remove the microSD card directly because Etcher automatically ejects or unmounts the microSD card upon completion.</span></span>
   1. <span data-ttu-id="80d70-155">Infoga microSD-kort i Pi.</span><span class="sxs-lookup"><span data-stu-id="80d70-155">Insert the microSD card into Pi.</span></span>

### <a name="enable-ssh-and-i2c"></a><span data-ttu-id="80d70-156">Aktivera SSH och I2C</span><span class="sxs-lookup"><span data-stu-id="80d70-156">Enable SSH and I2C</span></span>

1. <span data-ttu-id="80d70-157">Ansluta Pi till bildskärm, tangentbord och mus, starta Pi och logga sedan in Raspbian med hjälp av `pi` som användarnamn och `raspberry` som lösenord.</span><span class="sxs-lookup"><span data-stu-id="80d70-157">Connect Pi to the monitor, keyboard and mouse, start Pi and then log in Raspbian by using `pi` as the user name and `raspberry` as the password.</span></span>
1. <span data-ttu-id="80d70-158">Klicka på ikonen Raspberry > **inställningar** > **hallon Pi Configuration**.</span><span class="sxs-lookup"><span data-stu-id="80d70-158">Click the Raspberry icon > **Preferences** > **Raspberry Pi Configuration**.</span></span>

   ![Menyn Raspbian inställningar](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

1. <span data-ttu-id="80d70-160">På den **gränssnitt** ställer du in **I2C** och **SSH** till **aktivera**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="80d70-160">On the **Interfaces** tab, set **I2C** and **SSH** to **Enable**, and then click **OK**.</span></span> <span data-ttu-id="80d70-161">Om du inte har fysisk sensorer och vill använda simulerade sensordata, är det här steget valfritt.</span><span class="sxs-lookup"><span data-stu-id="80d70-161">If you don't have physical sensors and want to use simulated sensor data, this step is optional.</span></span>

   ![Aktivera I2C och SSH på Raspberry Pi](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
<span data-ttu-id="80d70-163">Om du vill aktivera SSH och I2C hittar du mer referensdokument på [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) och [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span><span class="sxs-lookup"><span data-stu-id="80d70-163">To enable SSH and I2C, you can find more reference documents on [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) and [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span></span>

### <a name="connect-the-sensor-to-pi"></a><span data-ttu-id="80d70-164">Ansluta sensorn till Pi</span><span class="sxs-lookup"><span data-stu-id="80d70-164">Connect the sensor to Pi</span></span>

<span data-ttu-id="80d70-165">Använd breadboard och omkopplare kablar för att ansluta en bildskärm och en BME280 till Pi på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="80d70-165">Use the breadboard and jumper wires to connect an LED and a BME280 to Pi as follows.</span></span> <span data-ttu-id="80d70-166">Om du inte har sensorn [hoppa över det här avsnittet](#connect-pi-to-the-network).</span><span class="sxs-lookup"><span data-stu-id="80d70-166">If you don’t have the sensor, [skip this section](#connect-pi-to-the-network).</span></span>

![Hallon Pi och sensor-anslutning](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

<span data-ttu-id="80d70-168">BME280 sensor kan samla in data för temperatur- och fuktighetskonsekvens.</span><span class="sxs-lookup"><span data-stu-id="80d70-168">The BME280 sensor can collect temperature and humidity data.</span></span> <span data-ttu-id="80d70-169">Och Indikatorn blinkar om det finns en kommunikation mellan enheten och i molnet.</span><span class="sxs-lookup"><span data-stu-id="80d70-169">And the LED will blink if there is a communication between device and the cloud.</span></span> 

<span data-ttu-id="80d70-170">Använd följande-kablar för sensor PIN:</span><span class="sxs-lookup"><span data-stu-id="80d70-170">For sensor pins, use the following wiring:</span></span>

| <span data-ttu-id="80d70-171">Starta (Sensor & Indikator)</span><span class="sxs-lookup"><span data-stu-id="80d70-171">Start (Sensor & LED)</span></span>     | <span data-ttu-id="80d70-172">END (moderkort)</span><span class="sxs-lookup"><span data-stu-id="80d70-172">End (Board)</span></span>            | <span data-ttu-id="80d70-173">Kabel färg</span><span class="sxs-lookup"><span data-stu-id="80d70-173">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="80d70-174">VDD (PIN-kod 5G)</span><span class="sxs-lookup"><span data-stu-id="80d70-174">VDD (Pin 5G)</span></span>             | <span data-ttu-id="80d70-175">3, 3v PWR (PIN-kod 1)</span><span class="sxs-lookup"><span data-stu-id="80d70-175">3.3V PWR (Pin 1)</span></span>       | <span data-ttu-id="80d70-176">Vita kabeln</span><span class="sxs-lookup"><span data-stu-id="80d70-176">White cable</span></span>   |
| <span data-ttu-id="80d70-177">JORD (PIN-kod 7G)</span><span class="sxs-lookup"><span data-stu-id="80d70-177">GND (Pin 7G)</span></span>             | <span data-ttu-id="80d70-178">JORD (PIN-kod 6)</span><span class="sxs-lookup"><span data-stu-id="80d70-178">GND (Pin 6)</span></span>            | <span data-ttu-id="80d70-179">Bruna kabel</span><span class="sxs-lookup"><span data-stu-id="80d70-179">Brown cable</span></span>   |
| <span data-ttu-id="80d70-180">SDI (PIN-kod 10G)</span><span class="sxs-lookup"><span data-stu-id="80d70-180">SDI (Pin 10G)</span></span>            | <span data-ttu-id="80d70-181">I2C1 SDA (PIN-kod 3)</span><span class="sxs-lookup"><span data-stu-id="80d70-181">I2C1 SDA (Pin 3)</span></span>       | <span data-ttu-id="80d70-182">Röd kabel</span><span class="sxs-lookup"><span data-stu-id="80d70-182">Red cable</span></span>     |
| <span data-ttu-id="80d70-183">SCK (PIN-kod 8G)</span><span class="sxs-lookup"><span data-stu-id="80d70-183">SCK (Pin 8G)</span></span>             | <span data-ttu-id="80d70-184">I2C1 SCL (PIN-kod 5)</span><span class="sxs-lookup"><span data-stu-id="80d70-184">I2C1 SCL (Pin 5)</span></span>       | <span data-ttu-id="80d70-185">Orange kabel</span><span class="sxs-lookup"><span data-stu-id="80d70-185">Orange cable</span></span>  |
| <span data-ttu-id="80d70-186">Indikator VDD (PIN-kod 18F)</span><span class="sxs-lookup"><span data-stu-id="80d70-186">LED VDD (Pin 18F)</span></span>        | <span data-ttu-id="80d70-187">GPIO 24 (PIN-kod 18)</span><span class="sxs-lookup"><span data-stu-id="80d70-187">GPIO 24 (Pin 18)</span></span>       | <span data-ttu-id="80d70-188">Vita kabeln</span><span class="sxs-lookup"><span data-stu-id="80d70-188">White cable</span></span>   |
| <span data-ttu-id="80d70-189">Indikator jord (PIN-kod 17F)</span><span class="sxs-lookup"><span data-stu-id="80d70-189">LED GND (Pin 17F)</span></span>        | <span data-ttu-id="80d70-190">JORD (PIN-kod 20)</span><span class="sxs-lookup"><span data-stu-id="80d70-190">GND (Pin 20)</span></span>           | <span data-ttu-id="80d70-191">Svart kabel</span><span class="sxs-lookup"><span data-stu-id="80d70-191">Black cable</span></span>   |

<span data-ttu-id="80d70-192">Klicka för att visa [hallon Pi 2 och 3 PIN-kod mappningar](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) referens.</span><span class="sxs-lookup"><span data-stu-id="80d70-192">Click to view [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.</span></span>

<span data-ttu-id="80d70-193">När du har anslutit BME280 till din hallon Pi, ska den som under bilden.</span><span class="sxs-lookup"><span data-stu-id="80d70-193">After you've successfully connected BME280 to your Raspberry Pi, it should be like below image.</span></span>

![Anslutna Pi och BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a><span data-ttu-id="80d70-195">Ansluta Pi till nätverket</span><span class="sxs-lookup"><span data-stu-id="80d70-195">Connect Pi to the network</span></span>

<span data-ttu-id="80d70-196">Aktivera Pi med hjälp av micro USB-kabel och strömförsörjningen.</span><span class="sxs-lookup"><span data-stu-id="80d70-196">Turn on Pi by using the micro USB cable and the power supply.</span></span> <span data-ttu-id="80d70-197">Använd Ethernet-kabel för att ansluta Pi till ett kabelanslutet nätverk eller följa den [instruktioner från grunden hallon Pi](https://www.raspberrypi.org/learning/software-guide/wifi/) ansluta Pi till det trådlösa nätverket.</span><span class="sxs-lookup"><span data-stu-id="80d70-197">Use the Ethernet cable to connect Pi to your wired network or follow the [instructions from the Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) to connect Pi to your wireless network.</span></span> <span data-ttu-id="80d70-198">När din Pi har lyckats ansluta till nätverket, måste du anteckna den [IP-adressen för din Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span><span class="sxs-lookup"><span data-stu-id="80d70-198">After your Pi has been successfully connected to the network, you need to take a note of the [IP address of your Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span></span>

![Ansluten till kabelanslutna nätverk](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> <span data-ttu-id="80d70-200">Kontrollera att Pi är ansluten till samma nätverk som datorn.</span><span class="sxs-lookup"><span data-stu-id="80d70-200">Make sure that Pi is connected to the same network as your computer.</span></span> <span data-ttu-id="80d70-201">Om datorn är ansluten till ett trådlöst nätverk när Pi är ansluten till ett kabelanslutet nätverk, kan du inte se IP-adressen i devdisco utdata.</span><span class="sxs-lookup"><span data-stu-id="80d70-201">For example, if your computer is connected to a wireless network while Pi is connected to a wired network, you might not see the IP address in the devdisco output.</span></span>

## <a name="run-a-sample-application-on-pi"></a><span data-ttu-id="80d70-202">Kör ett exempelprogram på Pi</span><span class="sxs-lookup"><span data-stu-id="80d70-202">Run a sample application on Pi</span></span>

### <a name="clone-sample-application-and-install-the-prerequisite-packages"></a><span data-ttu-id="80d70-203">Klona exempelprogrammet och installera de nödvändiga paketen</span><span class="sxs-lookup"><span data-stu-id="80d70-203">Clone sample application and install the prerequisite packages</span></span>

1. <span data-ttu-id="80d70-204">Använd en av följande SSH-klienter från värddatorn för att ansluta till din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="80d70-204">Use one of the following SSH clients from your host computer to connect to your Raspberry Pi.</span></span>
   
   <span data-ttu-id="80d70-205">**Windows-användare**</span><span class="sxs-lookup"><span data-stu-id="80d70-205">**Windows Users**</span></span>
   1. <span data-ttu-id="80d70-206">Hämta och installera [PuTTY](http://www.putty.org/) för Windows.</span><span class="sxs-lookup"><span data-stu-id="80d70-206">Download and install [PuTTY](http://www.putty.org/) for Windows.</span></span> 
   1. <span data-ttu-id="80d70-207">Kopiera IP-adressen för ditt avsnitt Pi till värden (eller IP-adress) och välj SSH som anslutningstypen.</span><span class="sxs-lookup"><span data-stu-id="80d70-207">Copy the IP address of your Pi into the Host name (or IP address) section and select SSH as the connection type.</span></span>
   
   ![PuTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   <span data-ttu-id="80d70-209">**Mac- och Ubuntu användare**</span><span class="sxs-lookup"><span data-stu-id="80d70-209">**Mac and Ubuntu Users**</span></span>
   
   <span data-ttu-id="80d70-210">Använd den inbyggda SSH-klienten på Ubuntu eller macOS.</span><span class="sxs-lookup"><span data-stu-id="80d70-210">Use the built-in SSH client on Ubuntu or macOS.</span></span> <span data-ttu-id="80d70-211">Du kan behöva köra `ssh pi@<ip address of pi>` ansluta Pi via SSH.</span><span class="sxs-lookup"><span data-stu-id="80d70-211">You might need to run `ssh pi@<ip address of pi>` to connect Pi via SSH.</span></span>
   > [!NOTE] 
   <span data-ttu-id="80d70-212">Standardanvändarnamnet är `pi` , och lösenordet är `raspberry`.</span><span class="sxs-lookup"><span data-stu-id="80d70-212">The default username is `pi` , and the password is `raspberry`.</span></span>

1. <span data-ttu-id="80d70-213">Installera Node.js och NPM till din Pi.</span><span class="sxs-lookup"><span data-stu-id="80d70-213">Install Node.js and NPM to your Pi.</span></span>
   
   <span data-ttu-id="80d70-214">Först bör du kontrollera din Node.js-version med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="80d70-214">First you should check your Node.js version with the following command.</span></span> 
   
   ```bash
   node -v
   ```

   <span data-ttu-id="80d70-215">Om versionen är lägre än 4.x eller om det finns inga Node.js på din Pi, kör du följande kommando för att installera eller uppdatera Node.js.</span><span class="sxs-lookup"><span data-stu-id="80d70-215">If the version is lower than 4.x or there is no Node.js on your Pi, then run the following command to install or update Node.js.</span></span>

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

1. <span data-ttu-id="80d70-216">Klona exempelprogrammet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="80d70-216">Clone the sample application by running the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

1. <span data-ttu-id="80d70-217">Installera alla paket med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="80d70-217">Install all packages by the following command.</span></span> <span data-ttu-id="80d70-218">Det innehåller Azure IoT-enhet SDK, BME280 Sensor bibliotek och kabeldragning Pi-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="80d70-218">It includes Azure IoT device SDK, BME280 Sensor library and Wiring Pi library.</span></span>

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   <span data-ttu-id="80d70-219">Det kan ta flera minuter att slutföra den här processen beroende på nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="80d70-219">It might take several minutes to finish this installation process depending on your network connection.</span></span>

### <a name="configure-the-sample-application"></a><span data-ttu-id="80d70-220">Konfigurera exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="80d70-220">Configure the sample application</span></span>

1. <span data-ttu-id="80d70-221">Öppna konfigurationsfilen genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="80d70-221">Open the config file by running the following commands:</span></span>

   ```bash
   nano config.json
   ```

   ![Config-fil](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   <span data-ttu-id="80d70-223">Det finns två objekt i den här filen kan du configurate.</span><span class="sxs-lookup"><span data-stu-id="80d70-223">There are two items in this file you can configurate.</span></span> <span data-ttu-id="80d70-224">Den första är `interval`, som definierar tidsintervall (i millisekunder) mellan två meddelanden som skickas till molnet.</span><span class="sxs-lookup"><span data-stu-id="80d70-224">The first one is `interval`, which defines the time interval (in milliseconds) between two messages that send to cloud.</span></span> <span data-ttu-id="80d70-225">Det andra `simulatedData`, vilket är ett booleskt värde för om du vill använda simulerade sensordata eller inte.</span><span class="sxs-lookup"><span data-stu-id="80d70-225">The second one `simulatedData`,which is a Boolean value for whether to use simulated sensor data or not.</span></span>

   <span data-ttu-id="80d70-226">Om du **saknar sensorn**, ange den `simulatedData` värde till `true` för att skapa och använda simulerade sensordata exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="80d70-226">If you **don't have the sensor**, set the `simulatedData` value to `true` to make the sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="80d70-227">Spara och avsluta genom att trycka på CTRL-O > Ange > kontrollen X.</span><span class="sxs-lookup"><span data-stu-id="80d70-227">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="run-the-sample-application"></a><span data-ttu-id="80d70-228">Kör exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="80d70-228">Run the sample application</span></span>

<span data-ttu-id="80d70-229">Kör exempelprogrammet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="80d70-229">Run the sample application by running the following command:</span></span>

   ```bash
   sudo node index.js '<YOUR AZURE IOT HUB DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   <span data-ttu-id="80d70-230">Kontrollera att du kopiera / klistra in anslutningssträngen enheten till de enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="80d70-230">Make sure you copy-paste the device connection string into the single quotes.</span></span>


<span data-ttu-id="80d70-231">Du bör se följande utdata som visar dessa sensordata och meddelanden som skickas till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80d70-231">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub.</span></span>

![Utdata - sensordata som skickats från hallon Pi till din IoT-hubb](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a><span data-ttu-id="80d70-233">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="80d70-233">Next steps</span></span>

<span data-ttu-id="80d70-234">Du har kört ett exempelprogram för att samla in sensordata och skicka den till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="80d70-234">You’ve run a sample application to collect sensor data and send it to your IoT hub.</span></span> <span data-ttu-id="80d70-235">Meddelanden som din hallon Pi har skickats till din IoT-hubb eller skicka meddelanden till din hallon Pi i ett kommandoradsgränssnitt finns i [hantera moln enhet meddelanden med iothub explorer kursen](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span><span class="sxs-lookup"><span data-stu-id="80d70-235">To see the messages that your Raspberry Pi has sent to your IoT hub or send messages to your Raspberry Pi in a command line interface, see the [Manage cloud device messaging with iothub-explorer tutorial](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
