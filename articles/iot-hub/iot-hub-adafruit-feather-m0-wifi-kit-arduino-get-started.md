---
title: 'M0 toocloud: ansluta ludd M0 WiFi tooAzure IoT-hubb | Microsoft Docs'
description: "Lär dig hur tooset upp och ansluta Adafruit ludd M0 WiFi tooAzure IoT-hubb toosend data toohello Azure cloud-plattformen i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 51befcdb-332b-416f-a6a1-8aabdb67f283
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/16/2017
ms.author: xshi
ms.openlocfilehash: 6aabeb961a50ba5d3934f77eb1ccda4af1bf64c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-adafruit-feather-m0-wifi-tooazure-iot-hub-in-hello-cloud"></a><span data-ttu-id="1a2d4-103">Ansluta Adafruit ludd M0 WiFi tooAzure IoT-hubb i hello moln</span><span class="sxs-lookup"><span data-stu-id="1a2d4-103">Connect Adafruit Feather M0 WiFi tooAzure IoT Hub in hello cloud</span></span>
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Anslutning mellan en BME280 och ludd M0 WiFi IoT-hubb](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

<span data-ttu-id="1a2d4-105">I den här självstudiekursen börjar du med learning hello grunderna i att arbeta med Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-105">In this tutorial, you begin by learning hello basics of working with your Arduino board.</span></span> <span data-ttu-id="1a2d4-106">Du lär dig hur tooseamlessly ansluta enheter toohello molnet med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="1a2d4-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="1a2d4-107">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="1a2d4-107">What you do</span></span>

<span data-ttu-id="1a2d4-108">Ansluta Adafruit ludd M0 WiFi tooan IoT-hubb som du skapar.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-108">Connect Adafruit Feather M0 WiFi tooan IoT hub that you create.</span></span> <span data-ttu-id="1a2d4-109">Sedan kör du ett exempelprogram på M0 WiFi toocollect hello temperatur- och fuktighetskonsekvens data från en BME280.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-109">Then you run a sample application on M0 WiFi toocollect hello temperature and humidity data from a BME280.</span></span> <span data-ttu-id="1a2d4-110">Slutligen kan skicka du hello sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-110">Finally, you send hello sensor data tooyour IoT hub.</span></span>


## <a name="what-you-learn"></a><span data-ttu-id="1a2d4-111">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="1a2d4-111">What you learn</span></span>

* <span data-ttu-id="1a2d4-112">Hur toocreate en IoT-hubb och registrera en enhet för Ludd M0 WiFi</span><span class="sxs-lookup"><span data-stu-id="1a2d4-112">How toocreate an IoT hub and register a device for Feather M0 WiFi</span></span>
* <span data-ttu-id="1a2d4-113">Hur tooconnect ludd M0 WiFi med hello sensor och datorn</span><span class="sxs-lookup"><span data-stu-id="1a2d4-113">How tooconnect Feather M0 WiFi with hello sensor and your computer</span></span>
* <span data-ttu-id="1a2d4-114">Hur toocollect sensordata genom att köra ett exempelprogram på ludd M0 WiFi</span><span class="sxs-lookup"><span data-stu-id="1a2d4-114">How toocollect sensor data by running a sample application on Feather M0 WiFi</span></span>
* <span data-ttu-id="1a2d4-115">Hur toosend hello sensor data tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="1a2d4-115">How toosend hello sensor data tooyour IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="1a2d4-116">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="1a2d4-116">What you need</span></span>

![delar som behövs för hello självstudiekursen](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="1a2d4-118">toocomplete den här åtgärden måste hello följande delar från din startpaket för Ludd M0 Wi-Fi:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-118">toocomplete this operation, you need hello following parts from your Feather M0 WiFi Starter Kit:</span></span>

* <span data-ttu-id="1a2d4-119">hello ludd M0 WiFi-kort</span><span class="sxs-lookup"><span data-stu-id="1a2d4-119">hello Feather M0 WiFi board</span></span>
* <span data-ttu-id="1a2d4-120">Micro USB-tooType en USB-kabel</span><span class="sxs-lookup"><span data-stu-id="1a2d4-120">A Micro USB tooType A USB cable</span></span>

<span data-ttu-id="1a2d4-121">Du måste också följande saker för din utvecklingsmiljö hello:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-121">You also need hello following things for your development environment:</span></span>

* <span data-ttu-id="1a2d4-122">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-122">An active Azure subscription.</span></span> <span data-ttu-id="1a2d4-123">Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-123">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="1a2d4-124">En Mac eller en dator som kör Windows eller Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-124">A Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="1a2d4-125">Ett trådlöst nätverk för Ludd M0 WiFi tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-125">A wireless network for Feather M0 WiFi tooconnect to.</span></span>
* <span data-ttu-id="1a2d4-126">En Internet-anslutning toodownload hello-konfigurationsverktyget.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-126">An Internet connection toodownload hello configuration tool.</span></span>
* <span data-ttu-id="1a2d4-127">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-127">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 or later.</span></span> <span data-ttu-id="1a2d4-128">Tidigare versioner fungerar inte med hello Azure IoT Hub-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-128">Earlier versions don't work with hello Azure IoT Hub library.</span></span>

<span data-ttu-id="1a2d4-129">Om du inte har en sensor är hello följande objekt valfria.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-129">If you don’t have a sensor, hello following items are optional.</span></span> <span data-ttu-id="1a2d4-130">Du kan också ha hello möjlighet att använda simulerade sensordata:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-130">You also have hello option of using simulated sensor data:</span></span>

* <span data-ttu-id="1a2d4-131">En BME280 temperatur- och fuktighetskonsekvens sensor</span><span class="sxs-lookup"><span data-stu-id="1a2d4-131">A BME280 temperature and humidity sensor</span></span>
* <span data-ttu-id="1a2d4-132">En breadboard</span><span class="sxs-lookup"><span data-stu-id="1a2d4-132">A breadboard</span></span>
* <span data-ttu-id="1a2d4-133">M/M omkopplare kablar</span><span class="sxs-lookup"><span data-stu-id="1a2d4-133">M/M jumper wires</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-hello-sensor-and-your-computer"></a><span data-ttu-id="1a2d4-134">Anslut ludd M0 WiFi med hello sensor och din dator</span><span class="sxs-lookup"><span data-stu-id="1a2d4-134">Connect Feather M0 WiFi with hello sensor and your computer</span></span>
<span data-ttu-id="1a2d4-135">I det här avsnittet kan du ansluta hello sensorer tooyour kort.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-135">In this section, you connect hello sensors tooyour board.</span></span> <span data-ttu-id="1a2d4-136">Sedan ansluter du enheten tooyour datorn för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-136">Then you plug in your device tooyour computer for further use.</span></span>

### <a name="connect-a-dht22-temperature-and-humidity-sensor-toofeather-m0-wifi"></a><span data-ttu-id="1a2d4-137">Ansluta en DHT22 temperatur- och fuktighetskonsekvens sensor tooFeather M0 WiFi</span><span class="sxs-lookup"><span data-stu-id="1a2d4-137">Connect a DHT22 temperature and humidity sensor tooFeather M0 WiFi</span></span>

<span data-ttu-id="1a2d4-138">Använd hello breadboard och omkopplare kablar toomake hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-138">Use hello breadboard and jumper wires toomake hello connection.</span></span> <span data-ttu-id="1a2d4-139">Om du inte har en sensor hoppa över det här avsnittet, eftersom du kan använda simulerade sensordata i stället.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-139">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![Referens för anslutningar](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


<span data-ttu-id="1a2d4-141">För sensor PIN-koder, använder du följande kablar hello:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-141">For sensor pins, use hello following wiring:</span></span>


| <span data-ttu-id="1a2d4-142">Start (sensor)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-142">Start (sensor)</span></span>           | <span data-ttu-id="1a2d4-143">END (moderkort)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-143">End (board)</span></span>            | <span data-ttu-id="1a2d4-144">Kabel färg</span><span class="sxs-lookup"><span data-stu-id="1a2d4-144">Cable color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="1a2d4-145">VDD (PIN-kod 27A)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-145">VDD (Pin 27A)</span></span>            | <span data-ttu-id="1a2d4-146">3v (3A PIN-kod)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-146">3V (Pin 3A)</span></span>            | <span data-ttu-id="1a2d4-147">Röd kabel</span><span class="sxs-lookup"><span data-stu-id="1a2d4-147">Red cable</span></span>     |
| <span data-ttu-id="1a2d4-148">JORD (PIN-kod 29 a)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-148">GND (Pin 29A)</span></span>            | <span data-ttu-id="1a2d4-149">JORD (6A PIN-kod)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-149">GND (Pin 6A)</span></span>           | <span data-ttu-id="1a2d4-150">Svart kabel</span><span class="sxs-lookup"><span data-stu-id="1a2d4-150">Black cable</span></span>   |
| <span data-ttu-id="1a2d4-151">SCK (PIN-kod 30A)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-151">SCK (Pin 30A)</span></span>            | <span data-ttu-id="1a2d4-152">SCK (PIN-kod 12A)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-152">SCK (Pin 12A)</span></span>          | <span data-ttu-id="1a2d4-153">Gult kabel</span><span class="sxs-lookup"><span data-stu-id="1a2d4-153">Yellow cable</span></span>  |
| <span data-ttu-id="1a2d4-154">SDO (PIN-kod 31A)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-154">SDO (Pin 31A)</span></span>            | <span data-ttu-id="1a2d4-155">MI (PIN-kod 14A)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-155">MI (Pin 14A)</span></span>           | <span data-ttu-id="1a2d4-156">Vita kabeln</span><span class="sxs-lookup"><span data-stu-id="1a2d4-156">White cable</span></span>   |
| <span data-ttu-id="1a2d4-157">SDI (PIN-kod 32 a)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-157">SDI (Pin 32A)</span></span>            | <span data-ttu-id="1a2d4-158">M0 (PIN-kod 13 a)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-158">M0 (Pin 13A)</span></span>           | <span data-ttu-id="1a2d4-159">Blå kabeln</span><span class="sxs-lookup"><span data-stu-id="1a2d4-159">Blue cable</span></span>    |
| <span data-ttu-id="1a2d4-160">CS (PIN-kod 33A)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-160">CS (Pin 33A)</span></span>             | <span data-ttu-id="1a2d4-161">GPIO 5 (PIN-kod 15J)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-161">GPIO 5 (Pin 15J)</span></span>       | <span data-ttu-id="1a2d4-162">Orange kabel</span><span class="sxs-lookup"><span data-stu-id="1a2d4-162">Orange cable</span></span>  |

<span data-ttu-id="1a2d4-163">Mer information finns i [Adafruit BME280 fuktighet barometertrycket + temperatur Sensor nedbrytning](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) och [Adafruit ludd M0 WiFi pinouts](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).</span><span class="sxs-lookup"><span data-stu-id="1a2d4-163">For more information, see [Adafruit BME280 Humidity + Barometric Pressure + Temperature Sensor Breakout](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) and [Adafruit Feather M0 WiFi pinouts](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).</span></span>



<span data-ttu-id="1a2d4-164">Ludd M0 Wi-Fi bör nu vara ansluten med en fungerar sensor.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-164">Now your Feather M0 WiFi should be connected with a working sensor.</span></span>

![Anslut DHT22 med ludd Huzzah](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-tooyour-computer"></a><span data-ttu-id="1a2d4-166">Ansluta ludd M0 WiFi tooyour dator</span><span class="sxs-lookup"><span data-stu-id="1a2d4-166">Connect Feather M0 WiFi tooyour computer</span></span>

<span data-ttu-id="1a2d4-167">Använd hello Micro USB tooType en USB-kabel tooconnect ludd M0 WiFi tooyour dator, som visas:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-167">Use hello Micro USB tooType A USB cable tooconnect Feather M0 WiFi tooyour computer, as shown:</span></span>

![Ansluta ludd Huzzah tooyour dator](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a><span data-ttu-id="1a2d4-169">Lägg till behörigheter för seriell port (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="1a2d4-169">Add serial port permissions (Ubuntu only)</span></span>

<span data-ttu-id="1a2d4-170">Om du använder Ubuntu, kontrollera att du har hello behörigheter toooperate på hello USB-port för Ludd M0 WiFi.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-170">If you use Ubuntu, make sure you have hello permissions toooperate on hello USB port of Feather M0 WiFi.</span></span> <span data-ttu-id="1a2d4-171">tooadd serieport behörigheter så här:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-171">tooadd serial port permissions, follow these steps:</span></span>


1. <span data-ttu-id="1a2d4-172">Kör hello följande kommandon i en terminal:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-172">At a terminal, run hello following commands:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="1a2d4-173">Du får ett av hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-173">You get one of hello following outputs:</span></span>

   * <span data-ttu-id="1a2d4-174">crw-rw---1 rot uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="1a2d4-174">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="1a2d4-175">crw-rw---1 rot antingen xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="1a2d4-175">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="1a2d4-176">Hello utdata och notera att `uucp` eller `dialout` är hello ägare gruppnamn hello USB-port.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-176">In hello output, notice that `uucp` or `dialout` is hello group owner name of hello USB port.</span></span>

2. <span data-ttu-id="1a2d4-177">tooadd hello toohello användargruppen, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-177">tooadd hello user toohello group, run hello following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="1a2d4-178">I föregående steg hello du fick hello gruppnamn ägare `<group-owner-name>`.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-178">In hello previous step, you obtained hello group owner name `<group-owner-name>`.</span></span> <span data-ttu-id="1a2d4-179">Användarnamnet Ubuntu är `<username>`.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-179">Your Ubuntu user name is `<username>`.</span></span>

3. <span data-ttu-id="1a2d4-180">Logga ut från Ubuntu för hello ändra tooappear och loggar sedan in igen.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-180">For hello change tooappear, sign out of Ubuntu and then sign in again.</span></span>

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a><span data-ttu-id="1a2d4-181">Samla in sensordata och skicka den tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="1a2d4-181">Collect sensor data and send it tooyour IoT hub</span></span>

<span data-ttu-id="1a2d4-182">I det här avsnittet, distribuera och köra ett exempelprogram på ludd M0 WiFi.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-182">In this section, you deploy and run a sample application on Feather M0 WiFi.</span></span> <span data-ttu-id="1a2d4-183">hello exempelprogrammet gör hello Indikator blinka på ludd M0 WiFi.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-183">hello sample application makes hello LED blink on Feather M0 WiFi.</span></span> <span data-ttu-id="1a2d4-184">Skickar sedan hello temperatur och fuktighet data som samlas in från hello BME280 sensor tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-184">It then sends hello temperature and humidity data collected from hello BME280 sensor tooyour IoT hub.</span></span>

### <a name="get-hello-sample-application-from-github-and-prepare-hello-arduino-ide"></a><span data-ttu-id="1a2d4-185">Hämta exempelprogrammet hello från GitHub och förbereda hello Arduino IDE</span><span class="sxs-lookup"><span data-stu-id="1a2d4-185">Get hello sample application from GitHub and prepare hello Arduino IDE</span></span>

<span data-ttu-id="1a2d4-186">hello exempelprogrammet finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-186">hello sample application is hosted on GitHub.</span></span> <span data-ttu-id="1a2d4-187">Klona hello exempel lagringsplats som innehåller hello exempelprogrammet från GitHub.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-187">Clone hello sample repository that contains hello sample application from GitHub.</span></span> <span data-ttu-id="1a2d4-188">tooclone hello exempel lagringsplatsen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-188">tooclone hello sample repository, follow these steps:</span></span>

1. <span data-ttu-id="1a2d4-189">Öppna en kommandotolk eller ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-189">Open a command prompt or a terminal window.</span></span>

2. <span data-ttu-id="1a2d4-190">Gå tooa mappen där du vill att hello exempel programmet toobe lagras.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-190">Go tooa folder where you want hello sample application toobe stored.</span></span>
3. <span data-ttu-id="1a2d4-191">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-191">Run hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-hello-package-for-feather-m0-wifi-in-hello-arduino-ide"></a><span data-ttu-id="1a2d4-192">Installera hello paketet för Ludd M0 WiFi i hello Arduino IDE</span><span class="sxs-lookup"><span data-stu-id="1a2d4-192">Install hello package for Feather M0 WiFi in hello Arduino IDE</span></span>

1. <span data-ttu-id="1a2d4-193">Öppna hello mappen där hello exempelprogrammet lagras.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-193">Open hello folder where hello sample application is stored.</span></span>

2. <span data-ttu-id="1a2d4-194">Öppna hello app.ino filen i mappen för hello-app i hello Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-194">Open hello app.ino file in hello app folder in hello Arduino IDE.</span></span>

   ![Öppna hello exempelprogrammet i Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


1. <span data-ttu-id="1a2d4-196">Klicka på **filen** > **inställningar** (Windows-/ Linux) eller **Arduino** > **inställningar** (Mac) och kopiera och Klistra in hello länk nedan i hello **ytterligare anslagstavlor Manager URL: er** alternativet i hello Arduino IDE-inställningar.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-196">Click **File** > **Preferences** (Windows/Linux) or **Arduino** > **Preferences** (Mac) and copy and paste hello link below into hello **Additional Boards Manager URLs** option in hello Arduino IDE preferences.</span></span>
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json, https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

1. <span data-ttu-id="1a2d4-197">Klicka på **verktyg** > **Hanteringsstyrenhetens** > **anslagstavlor Manager**, och sedan installera hello `Arduino SAMD Boards` version `1.6.2` eller senare.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-197">Click **Tools** > **Board** > **Boards Manager**, and then install hello `Arduino SAMD Boards` version `1.6.2` or later.</span></span> 

1. <span data-ttu-id="1a2d4-198">I Hej samma fönster, installera `Adafruit SAMD Boards` paketet tooadd hello board definitioner.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-198">Then in hello same window, install `Adafruit SAMD Boards` package tooadd hello board file definitions.</span></span>

   ![Hej esp8266 paketet har installerats](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

4. <span data-ttu-id="1a2d4-200">Klicka på **verktyg** > **Hanteringsstyrenhetens** > **Adafruit M0 WiFi**.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-200">Click **Tools** > **Board** > **Adafruit M0 WiFi**.</span></span>

5. <span data-ttu-id="1a2d4-201">Installera drivrutiner (för Windows).</span><span class="sxs-lookup"><span data-stu-id="1a2d4-201">Install drivers (for Windows only).</span></span> <span data-ttu-id="1a2d4-202">När du ansluter ludd M0 WiFi kan behöva du tooinstall en drivrutin.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-202">When you plug in Feather M0 WiFi, you might need tooinstall a driver.</span></span> <span data-ttu-id="1a2d4-203">Klicka på [hello hämtningslänken på webbsidan hello](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) toodownload hello drivrutinen installer.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-203">Click [hello download link on hello webpage](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) toodownload hello driver installer.</span></span> <span data-ttu-id="1a2d4-204">Följ hello steg tooinstall hello drivrutiner som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-204">Follow hello steps tooinstall hello drivers you want.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="1a2d4-205">Installera nödvändiga bibliotek</span><span class="sxs-lookup"><span data-stu-id="1a2d4-205">Install necessary libraries</span></span>

1. <span data-ttu-id="1a2d4-206">I hello Arduino IDE, klickar du på **skiss** > **innehåller biblioteket** > **Hantera bibliotek**.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-206">In hello Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>

2. <span data-ttu-id="1a2d4-207">Sök efter hello efter namn på biblioteket i taget.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-207">Search for hello following library names one by one.</span></span> <span data-ttu-id="1a2d4-208">Klicka på för alla bibliotek som du hittar **installera**:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-208">For each library that you find, click **Install**:</span></span>

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. <span data-ttu-id="1a2d4-209">Installera manuellt `Adafruit_WINC1500`.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-209">Manually install `Adafruit_WINC1500`.</span></span> <span data-ttu-id="1a2d4-210">Gå för[webbplatsen](https://github.com/adafruit/Adafruit_WINC1500) och på **kloning eller hämta** > **hämta ZIP**.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-210">Go too[this website](https://github.com/adafruit/Adafruit_WINC1500) and click **Clone or download** > **Download ZIP**.</span></span> <span data-ttu-id="1a2d4-211">Sedan gå för i din Arduino IDE**skiss** > **innehåller biblioteket** > **lägger du till .zip biblioteket** och Lägg till hello zip-filen.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-211">Then in your Arduino IDE, go too**Sketch** > **Include Library** > **Add .zip Library** and add hello zip file.</span></span>

### <a name="use-hello-sample-application-if-you-dont-have-a-real-bme280-sensor"></a><span data-ttu-id="1a2d4-212">Använd hello exempelprogrammet om du inte har en verklig BME280 sensor</span><span class="sxs-lookup"><span data-stu-id="1a2d4-212">Use hello sample application if you don’t have a real BME280 sensor</span></span>

<span data-ttu-id="1a2d4-213">Om du inte har en verklig BME280 sensor simulera hello exempelprogrammet temperatur- och fuktighetskonsekvens data.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-213">If you don’t have a real BME280 sensor, hello sample application can simulate temperature and humidity data.</span></span> <span data-ttu-id="1a2d4-214">tooset in hello programmet toouse simulerade exempeldata, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-214">tooset up hello sample application toouse simulated data, follow these steps:</span></span>

1. <span data-ttu-id="1a2d4-215">Öppna hello `config.h` filen i hello `app` mapp.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-215">Open hello `config.h` file in hello `app` folder.</span></span>

2. <span data-ttu-id="1a2d4-216">Leta upp följande kodrad hello och ändrar hello-värdet från `false` för`true`:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-216">Locate hello following line of code and change hello value from `false` too`true`:</span></span>

   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurera hello exempeldata programmet toouse simulerade](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. <span data-ttu-id="1a2d4-218">Spara hello-filen med `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-218">Save hello file with `Control-s`.</span></span>

### <a name="deploy-hello-sample-application-toofeather-m0-wifi"></a><span data-ttu-id="1a2d4-219">Distribuera hello exempel programmet tooFeather M0 WiFi</span><span class="sxs-lookup"><span data-stu-id="1a2d4-219">Deploy hello sample application tooFeather M0 WiFi</span></span>

1. <span data-ttu-id="1a2d4-220">Klicka på hello Arduino IDE **verktyget** > **Port**, och klicka sedan på hello seriell port för Ludd M0 WiFi.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-220">In hello Arduino IDE, click **Tool** > **Port**, and then click hello serial port for Feather M0 WiFi.</span></span>

2. <span data-ttu-id="1a2d4-221">Klicka på **skiss** > **överför** toobuild och distribuera hello exempel programmet tooFeather M0 WiFi.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-221">Click **Sketch** > **Upload** toobuild and deploy hello sample application tooFeather M0 WiFi.</span></span>

### <a name="enter-your-credentials"></a><span data-ttu-id="1a2d4-222">Ange autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="1a2d4-222">Enter your credentials</span></span>

<span data-ttu-id="1a2d4-223">När hello överföringen är klar följer du dessa steg tooenter dina autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-223">After hello upload completes successfully, follow these steps tooenter your credentials:</span></span>

1. <span data-ttu-id="1a2d4-224">Klicka på hello Arduino IDE **verktyg** > **seriella övervakaren**.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-224">In hello Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>

2. <span data-ttu-id="1a2d4-225">Markera i hello nedre högra hörnet av hello seriella övervakaren **ingen rad avslutas** i listrutan för hello hello vänster.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-225">In hello lower-right corner of hello serial monitor window, select **No line ending** in hello drop-down list on hello left.</span></span>
3. <span data-ttu-id="1a2d4-226">Välj **115200 baud** i hello listrutan på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-226">Select **115200 baud** in hello drop-down list on hello right.</span></span>
4. <span data-ttu-id="1a2d4-227">Ange hello följande information om du är i hello-textrutan hello överst och tooprovide och klicka på **skicka**:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-227">In hello input box at hello top, enter hello following information if you're asked tooprovide it, and click **Send**:</span></span>

   * <span data-ttu-id="1a2d4-228">Wi-Fi SSID</span><span class="sxs-lookup"><span data-stu-id="1a2d4-228">Wi-Fi SSID</span></span>
   * <span data-ttu-id="1a2d4-229">Wi-Fi-lösenord</span><span class="sxs-lookup"><span data-stu-id="1a2d4-229">Wi-Fi password</span></span>
   * <span data-ttu-id="1a2d4-230">Anslutningssträngen för enhet</span><span class="sxs-lookup"><span data-stu-id="1a2d4-230">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="1a2d4-231">information om hello autentiseringsuppgifter lagras i hello EEPROM av ludd M0 WiFi.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-231">hello credential information is stored in hello EEPROM of Feather M0 WiFi.</span></span> <span data-ttu-id="1a2d4-232">Om du klickar på hello Återställ-knappen i hello ludd M0 WiFi-kort frågar hello exempelprogrammet om du vill ha tooerase hello information.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-232">If you click hello reset button on hello Feather M0 WiFi board, hello sample application asks if you want tooerase hello information.</span></span> <span data-ttu-id="1a2d4-233">Ange `Y` tooerase hello information.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-233">Enter `Y` tooerase hello information.</span></span> <span data-ttu-id="1a2d4-234">Du uppmanas tooprovide hello information en gång.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-234">You're asked tooprovide hello information a second time.</span></span>

### <a name="verify-that-hello-sample-application-is-running-successfully"></a><span data-ttu-id="1a2d4-235">Kontrollera att hello exempelprogrammet körts</span><span class="sxs-lookup"><span data-stu-id="1a2d4-235">Verify that hello sample application is running successfully</span></span>

<span data-ttu-id="1a2d4-236">Om du ser hello följande utdata från hello seriella övervakningsfönstret och hello blinka Indikator på ludd M0 WiFi, hello exempelprogrammet körts:</span><span class="sxs-lookup"><span data-stu-id="1a2d4-236">If you see hello following output from hello serial monitor window and hello blinking LED on Feather M0 WiFi, hello sample application is running successfully:</span></span>

![Slutversionen i Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="1a2d4-238">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a2d4-238">Next steps</span></span>

<span data-ttu-id="1a2d4-239">Du har anslutit ludd M0 WiFi tooyour IoT-hubb och skickas hello avbildas sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="1a2d4-239">You have successfully connected Feather M0 WiFi tooyour IoT hub and sent hello captured sensor data tooyour IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

