---
title: 'M0 till molnet: ansluta ludd M0 WiFi till Azure IoT Hub | Microsoft Docs'
description: "Lär dig hur du ställer in och ansluta Adafruit ludd M0 WiFi till Azure IoT-hubb för att skicka data till Azure-molnplattform i den här självstudiekursen."
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
ms.openlocfilehash: 0dcf6b46a4c6c743c713d24ce7844e801b278dcf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="connect-adafruit-feather-m0-wifi-to-azure-iot-hub-in-the-cloud"></a><span data-ttu-id="013a7-103">Ansluta Adafruit ludd M0 WiFi till Azure IoT-hubb i molnet</span><span class="sxs-lookup"><span data-stu-id="013a7-103">Connect Adafruit Feather M0 WiFi to Azure IoT Hub in the cloud</span></span>
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Anslutning mellan en BME280 och ludd M0 WiFi IoT-hubb](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

<span data-ttu-id="013a7-105">I den här kursen kan du börja genom att lära dig grunderna i att arbeta med Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="013a7-105">In this tutorial, you begin by learning the basics of working with your Arduino board.</span></span> <span data-ttu-id="013a7-106">Du lär dig sedan sömlöst ansluter enheterna till molnet med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="013a7-106">You then learn how to seamlessly connect your devices to the cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="013a7-107">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="013a7-107">What you do</span></span>

<span data-ttu-id="013a7-108">Ansluta Adafruit ludd M0 WiFi till en IoT-hubb som du skapar.</span><span class="sxs-lookup"><span data-stu-id="013a7-108">Connect Adafruit Feather M0 WiFi to an IoT hub that you create.</span></span> <span data-ttu-id="013a7-109">Sedan kör du ett exempelprogram på M0 WiFi samla in data för temperatur- och fuktighetskonsekvens från en BME280.</span><span class="sxs-lookup"><span data-stu-id="013a7-109">Then you run a sample application on M0 WiFi to collect the temperature and humidity data from a BME280.</span></span> <span data-ttu-id="013a7-110">Slutligen kan du skicka dessa sensordata för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="013a7-110">Finally, you send the sensor data to your IoT hub.</span></span>


## <a name="what-you-learn"></a><span data-ttu-id="013a7-111">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="013a7-111">What you learn</span></span>

* <span data-ttu-id="013a7-112">Hur du skapar en IoT-hubb och registrera en enhet för Ludd M0 WiFi</span><span class="sxs-lookup"><span data-stu-id="013a7-112">How to create an IoT hub and register a device for Feather M0 WiFi</span></span>
* <span data-ttu-id="013a7-113">Så här ansluter du ludd M0 WiFi med sensorn och datorn</span><span class="sxs-lookup"><span data-stu-id="013a7-113">How to connect Feather M0 WiFi with the sensor and your computer</span></span>
* <span data-ttu-id="013a7-114">Hur du samlar in sensordata genom att köra ett exempelprogram på ludd M0 WiFi</span><span class="sxs-lookup"><span data-stu-id="013a7-114">How to collect sensor data by running a sample application on Feather M0 WiFi</span></span>
* <span data-ttu-id="013a7-115">Hur du skickar dessa sensordata till din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="013a7-115">How to send the sensor data to your IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="013a7-116">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="013a7-116">What you need</span></span>

![Delar som behövs för kursen](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="013a7-118">För att slutföra den här åtgärden behöver du följande delar från din startpaket för Ludd M0 Wi-Fi:</span><span class="sxs-lookup"><span data-stu-id="013a7-118">To complete this operation, you need the following parts from your Feather M0 WiFi Starter Kit:</span></span>

* <span data-ttu-id="013a7-119">Ludd M0 WiFi-kort</span><span class="sxs-lookup"><span data-stu-id="013a7-119">The Feather M0 WiFi board</span></span>
* <span data-ttu-id="013a7-120">En Micro USB till typ A USB-kabel</span><span class="sxs-lookup"><span data-stu-id="013a7-120">A Micro USB to Type A USB cable</span></span>

<span data-ttu-id="013a7-121">Du måste också följande saker för din utvecklingsmiljö:</span><span class="sxs-lookup"><span data-stu-id="013a7-121">You also need the following things for your development environment:</span></span>

* <span data-ttu-id="013a7-122">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="013a7-122">An active Azure subscription.</span></span> <span data-ttu-id="013a7-123">Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="013a7-123">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="013a7-124">En Mac eller en dator som kör Windows eller Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="013a7-124">A Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="013a7-125">Ett trådlöst nätverk för Ludd M0 WiFi att ansluta till.</span><span class="sxs-lookup"><span data-stu-id="013a7-125">A wireless network for Feather M0 WiFi to connect to.</span></span>
* <span data-ttu-id="013a7-126">En Internet-anslutning att hämta verktyget configuration.</span><span class="sxs-lookup"><span data-stu-id="013a7-126">An Internet connection to download the configuration tool.</span></span>
* <span data-ttu-id="013a7-127">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="013a7-127">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 or later.</span></span> <span data-ttu-id="013a7-128">Tidigare versioner fungerar inte med Azure IoT Hub-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="013a7-128">Earlier versions don't work with the Azure IoT Hub library.</span></span>

<span data-ttu-id="013a7-129">Om du inte har en sensor är följande objekt valfria.</span><span class="sxs-lookup"><span data-stu-id="013a7-129">If you don’t have a sensor, the following items are optional.</span></span> <span data-ttu-id="013a7-130">Du har också möjlighet att använda simulerade sensordata:</span><span class="sxs-lookup"><span data-stu-id="013a7-130">You also have the option of using simulated sensor data:</span></span>

* <span data-ttu-id="013a7-131">En BME280 temperatur- och fuktighetskonsekvens sensor</span><span class="sxs-lookup"><span data-stu-id="013a7-131">A BME280 temperature and humidity sensor</span></span>
* <span data-ttu-id="013a7-132">En breadboard</span><span class="sxs-lookup"><span data-stu-id="013a7-132">A breadboard</span></span>
* <span data-ttu-id="013a7-133">M/M omkopplare kablar</span><span class="sxs-lookup"><span data-stu-id="013a7-133">M/M jumper wires</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-the-sensor-and-your-computer"></a><span data-ttu-id="013a7-134">Anslut ludd M0 WiFi med sensorn och din dator</span><span class="sxs-lookup"><span data-stu-id="013a7-134">Connect Feather M0 WiFi with the sensor and your computer</span></span>
<span data-ttu-id="013a7-135">I det här avsnittet kan ansluta du sensorerna till kortets.</span><span class="sxs-lookup"><span data-stu-id="013a7-135">In this section, you connect the sensors to your board.</span></span> <span data-ttu-id="013a7-136">Sedan ansluta du enheten till din dator för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="013a7-136">Then you plug in your device to your computer for further use.</span></span>

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-m0-wifi"></a><span data-ttu-id="013a7-137">Ansluta en DHT22 temperatur- och fuktighetskonsekvens sensor till ludd M0 WiFi</span><span class="sxs-lookup"><span data-stu-id="013a7-137">Connect a DHT22 temperature and humidity sensor to Feather M0 WiFi</span></span>

<span data-ttu-id="013a7-138">Använd breadboard och omkopplare kablar för att ansluta.</span><span class="sxs-lookup"><span data-stu-id="013a7-138">Use the breadboard and jumper wires to make the connection.</span></span> <span data-ttu-id="013a7-139">Om du inte har en sensor hoppa över det här avsnittet, eftersom du kan använda simulerade sensordata i stället.</span><span class="sxs-lookup"><span data-stu-id="013a7-139">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![Referens för anslutningar](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


<span data-ttu-id="013a7-141">Använd följande-kablar för sensor PIN:</span><span class="sxs-lookup"><span data-stu-id="013a7-141">For sensor pins, use the following wiring:</span></span>


| <span data-ttu-id="013a7-142">Start (sensor)</span><span class="sxs-lookup"><span data-stu-id="013a7-142">Start (sensor)</span></span>           | <span data-ttu-id="013a7-143">END (moderkort)</span><span class="sxs-lookup"><span data-stu-id="013a7-143">End (board)</span></span>            | <span data-ttu-id="013a7-144">Kabel färg</span><span class="sxs-lookup"><span data-stu-id="013a7-144">Cable color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="013a7-145">VDD (PIN-kod 27A)</span><span class="sxs-lookup"><span data-stu-id="013a7-145">VDD (Pin 27A)</span></span>            | <span data-ttu-id="013a7-146">3v (3A PIN-kod)</span><span class="sxs-lookup"><span data-stu-id="013a7-146">3V (Pin 3A)</span></span>            | <span data-ttu-id="013a7-147">Röd kabel</span><span class="sxs-lookup"><span data-stu-id="013a7-147">Red cable</span></span>     |
| <span data-ttu-id="013a7-148">JORD (PIN-kod 29 a)</span><span class="sxs-lookup"><span data-stu-id="013a7-148">GND (Pin 29A)</span></span>            | <span data-ttu-id="013a7-149">JORD (6A PIN-kod)</span><span class="sxs-lookup"><span data-stu-id="013a7-149">GND (Pin 6A)</span></span>           | <span data-ttu-id="013a7-150">Svart kabel</span><span class="sxs-lookup"><span data-stu-id="013a7-150">Black cable</span></span>   |
| <span data-ttu-id="013a7-151">SCK (PIN-kod 30A)</span><span class="sxs-lookup"><span data-stu-id="013a7-151">SCK (Pin 30A)</span></span>            | <span data-ttu-id="013a7-152">SCK (PIN-kod 12A)</span><span class="sxs-lookup"><span data-stu-id="013a7-152">SCK (Pin 12A)</span></span>          | <span data-ttu-id="013a7-153">Gult kabel</span><span class="sxs-lookup"><span data-stu-id="013a7-153">Yellow cable</span></span>  |
| <span data-ttu-id="013a7-154">SDO (PIN-kod 31A)</span><span class="sxs-lookup"><span data-stu-id="013a7-154">SDO (Pin 31A)</span></span>            | <span data-ttu-id="013a7-155">MI (PIN-kod 14A)</span><span class="sxs-lookup"><span data-stu-id="013a7-155">MI (Pin 14A)</span></span>           | <span data-ttu-id="013a7-156">Vita kabeln</span><span class="sxs-lookup"><span data-stu-id="013a7-156">White cable</span></span>   |
| <span data-ttu-id="013a7-157">SDI (PIN-kod 32 a)</span><span class="sxs-lookup"><span data-stu-id="013a7-157">SDI (Pin 32A)</span></span>            | <span data-ttu-id="013a7-158">M0 (PIN-kod 13 a)</span><span class="sxs-lookup"><span data-stu-id="013a7-158">M0 (Pin 13A)</span></span>           | <span data-ttu-id="013a7-159">Blå kabeln</span><span class="sxs-lookup"><span data-stu-id="013a7-159">Blue cable</span></span>    |
| <span data-ttu-id="013a7-160">CS (PIN-kod 33A)</span><span class="sxs-lookup"><span data-stu-id="013a7-160">CS (Pin 33A)</span></span>             | <span data-ttu-id="013a7-161">GPIO 5 (PIN-kod 15J)</span><span class="sxs-lookup"><span data-stu-id="013a7-161">GPIO 5 (Pin 15J)</span></span>       | <span data-ttu-id="013a7-162">Orange kabel</span><span class="sxs-lookup"><span data-stu-id="013a7-162">Orange cable</span></span>  |

<span data-ttu-id="013a7-163">Mer information finns i [Adafruit BME280 fuktighet barometertrycket + temperatur Sensor nedbrytning](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) och [Adafruit ludd M0 WiFi pinouts](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).</span><span class="sxs-lookup"><span data-stu-id="013a7-163">For more information, see [Adafruit BME280 Humidity + Barometric Pressure + Temperature Sensor Breakout](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) and [Adafruit Feather M0 WiFi pinouts](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).</span></span>



<span data-ttu-id="013a7-164">Ludd M0 Wi-Fi bör nu vara ansluten med en fungerar sensor.</span><span class="sxs-lookup"><span data-stu-id="013a7-164">Now your Feather M0 WiFi should be connected with a working sensor.</span></span>

![Anslut DHT22 med ludd Huzzah](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-to-your-computer"></a><span data-ttu-id="013a7-166">Ansluta ludd M0 WiFi till din dator</span><span class="sxs-lookup"><span data-stu-id="013a7-166">Connect Feather M0 WiFi to your computer</span></span>

<span data-ttu-id="013a7-167">Använda Micro USB till typ A USB-kabel för att ansluta ludd M0 WiFi till datorn, som visas:</span><span class="sxs-lookup"><span data-stu-id="013a7-167">Use the Micro USB to Type A USB cable to connect Feather M0 WiFi to your computer, as shown:</span></span>

![Ansluta ludd Huzzah till din dator](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a><span data-ttu-id="013a7-169">Lägg till behörigheter för seriell port (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="013a7-169">Add serial port permissions (Ubuntu only)</span></span>

<span data-ttu-id="013a7-170">Om du använder Ubuntu, kontrollera att du har behörighet att fungera på den USB-port för Ludd M0 WiFi.</span><span class="sxs-lookup"><span data-stu-id="013a7-170">If you use Ubuntu, make sure you have the permissions to operate on the USB port of Feather M0 WiFi.</span></span> <span data-ttu-id="013a7-171">Följ dessa steg för att lägga till behörigheter för seriell port:</span><span class="sxs-lookup"><span data-stu-id="013a7-171">To add serial port permissions, follow these steps:</span></span>


1. <span data-ttu-id="013a7-172">Kör följande kommandon i en terminal:</span><span class="sxs-lookup"><span data-stu-id="013a7-172">At a terminal, run the following commands:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="013a7-173">Du får ett av följande utdata:</span><span class="sxs-lookup"><span data-stu-id="013a7-173">You get one of the following outputs:</span></span>

   * <span data-ttu-id="013a7-174">crw-rw---1 rot uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="013a7-174">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="013a7-175">crw-rw---1 rot antingen xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="013a7-175">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="013a7-176">Observera att i utdata, `uucp` eller `dialout` är gruppnamn för ägare av USB-port.</span><span class="sxs-lookup"><span data-stu-id="013a7-176">In the output, notice that `uucp` or `dialout` is the group owner name of the USB port.</span></span>

2. <span data-ttu-id="013a7-177">Lägg till användaren i gruppen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="013a7-177">To add the user to the group, run the following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="013a7-178">I föregående steg, som du fick ägare gruppnamnet `<group-owner-name>`.</span><span class="sxs-lookup"><span data-stu-id="013a7-178">In the previous step, you obtained the group owner name `<group-owner-name>`.</span></span> <span data-ttu-id="013a7-179">Användarnamnet Ubuntu är `<username>`.</span><span class="sxs-lookup"><span data-stu-id="013a7-179">Your Ubuntu user name is `<username>`.</span></span>

3. <span data-ttu-id="013a7-180">Logga ut från Ubuntu och loggar sedan in igen för att ändringen ska visas.</span><span class="sxs-lookup"><span data-stu-id="013a7-180">For the change to appear, sign out of Ubuntu and then sign in again.</span></span>

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a><span data-ttu-id="013a7-181">Samla in sensordata och skicka den till din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="013a7-181">Collect sensor data and send it to your IoT hub</span></span>

<span data-ttu-id="013a7-182">I det här avsnittet, distribuera och köra ett exempelprogram på ludd M0 WiFi.</span><span class="sxs-lookup"><span data-stu-id="013a7-182">In this section, you deploy and run a sample application on Feather M0 WiFi.</span></span> <span data-ttu-id="013a7-183">Exempelprogrammet gör Indikator blinka på ludd M0 WiFi.</span><span class="sxs-lookup"><span data-stu-id="013a7-183">The sample application makes the LED blink on Feather M0 WiFi.</span></span> <span data-ttu-id="013a7-184">Därefter skickas temperatur- och fuktighetskonsekvens data som samlas in från BME280 sensor till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="013a7-184">It then sends the temperature and humidity data collected from the BME280 sensor to your IoT hub.</span></span>

### <a name="get-the-sample-application-from-github-and-prepare-the-arduino-ide"></a><span data-ttu-id="013a7-185">Hämta exempelprogrammet från GitHub och förbereda Arduino IDE</span><span class="sxs-lookup"><span data-stu-id="013a7-185">Get the sample application from GitHub and prepare the Arduino IDE</span></span>

<span data-ttu-id="013a7-186">Exempelprogrammet finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="013a7-186">The sample application is hosted on GitHub.</span></span> <span data-ttu-id="013a7-187">Klona lagringsplatsen prov som innehåller exempelprogrammet från GitHub.</span><span class="sxs-lookup"><span data-stu-id="013a7-187">Clone the sample repository that contains the sample application from GitHub.</span></span> <span data-ttu-id="013a7-188">Följ dessa steg om du vill klona databasen exempel:</span><span class="sxs-lookup"><span data-stu-id="013a7-188">To clone the sample repository, follow these steps:</span></span>

1. <span data-ttu-id="013a7-189">Öppna en kommandotolk eller ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="013a7-189">Open a command prompt or a terminal window.</span></span>

2. <span data-ttu-id="013a7-190">Gå till en mapp där du vill att exempelprogrammet ska lagras.</span><span class="sxs-lookup"><span data-stu-id="013a7-190">Go to a folder where you want the sample application to be stored.</span></span>
3. <span data-ttu-id="013a7-191">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="013a7-191">Run the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-the-package-for-feather-m0-wifi-in-the-arduino-ide"></a><span data-ttu-id="013a7-192">Installera paketet för Ludd M0 WiFi i Arduino IDE</span><span class="sxs-lookup"><span data-stu-id="013a7-192">Install the package for Feather M0 WiFi in the Arduino IDE</span></span>

1. <span data-ttu-id="013a7-193">Öppna mappen där exempelprogrammet lagras.</span><span class="sxs-lookup"><span data-stu-id="013a7-193">Open the folder where the sample application is stored.</span></span>

2. <span data-ttu-id="013a7-194">Öppna filen app.ino i mappen app i Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="013a7-194">Open the app.ino file in the app folder in the Arduino IDE.</span></span>

   ![Öppna exempelprogrammet i Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


1. <span data-ttu-id="013a7-196">Klicka på **filen** > **inställningar** (Windows-/ Linux) eller **Arduino** > **inställningar** (Mac) och kopiera och Klistra in en länk nedan i den **ytterligare anslagstavlor Manager URL: er** alternativet i Arduino IDE-inställningar.</span><span class="sxs-lookup"><span data-stu-id="013a7-196">Click **File** > **Preferences** (Windows/Linux) or **Arduino** > **Preferences** (Mac) and copy and paste the link below into the **Additional Boards Manager URLs** option in the Arduino IDE preferences.</span></span>
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json, https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

1. <span data-ttu-id="013a7-197">Klicka på **verktyg** > **Hanteringsstyrenhetens** > **anslagstavlor Manager**, och sedan installera den `Arduino SAMD Boards` version `1.6.2` eller senare.</span><span class="sxs-lookup"><span data-stu-id="013a7-197">Click **Tools** > **Board** > **Boards Manager**, and then install the `Arduino SAMD Boards` version `1.6.2` or later.</span></span> 

1. <span data-ttu-id="013a7-198">Installera sedan i fönstret samma `Adafruit SAMD Boards` paket för att lägga till kort definitioner.</span><span class="sxs-lookup"><span data-stu-id="013a7-198">Then in the same window, install `Adafruit SAMD Boards` package to add the board file definitions.</span></span>

   ![Esp8266 paketet har installerats](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

4. <span data-ttu-id="013a7-200">Klicka på **verktyg** > **Hanteringsstyrenhetens** > **Adafruit M0 WiFi**.</span><span class="sxs-lookup"><span data-stu-id="013a7-200">Click **Tools** > **Board** > **Adafruit M0 WiFi**.</span></span>

5. <span data-ttu-id="013a7-201">Installera drivrutiner (för Windows).</span><span class="sxs-lookup"><span data-stu-id="013a7-201">Install drivers (for Windows only).</span></span> <span data-ttu-id="013a7-202">När du ansluter ludd M0 WiFi, kan du behöva installera en drivrutin.</span><span class="sxs-lookup"><span data-stu-id="013a7-202">When you plug in Feather M0 WiFi, you might need to install a driver.</span></span> <span data-ttu-id="013a7-203">Klicka på [länken på webbsidan](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) att hämta installationsprogrammet för drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="013a7-203">Click [the download link on the webpage](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) to download the driver installer.</span></span> <span data-ttu-id="013a7-204">Följ stegen för att installera drivrutiner som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="013a7-204">Follow the steps to install the drivers you want.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="013a7-205">Installera nödvändiga bibliotek</span><span class="sxs-lookup"><span data-stu-id="013a7-205">Install necessary libraries</span></span>

1. <span data-ttu-id="013a7-206">I Arduino IDE, klickar du på **skiss** > **innehåller biblioteket** > **Hantera bibliotek**.</span><span class="sxs-lookup"><span data-stu-id="013a7-206">In the Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>

2. <span data-ttu-id="013a7-207">Sök efter följande biblioteket namn i taget.</span><span class="sxs-lookup"><span data-stu-id="013a7-207">Search for the following library names one by one.</span></span> <span data-ttu-id="013a7-208">Klicka på för alla bibliotek som du hittar **installera**:</span><span class="sxs-lookup"><span data-stu-id="013a7-208">For each library that you find, click **Install**:</span></span>

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. <span data-ttu-id="013a7-209">Installera manuellt `Adafruit_WINC1500`.</span><span class="sxs-lookup"><span data-stu-id="013a7-209">Manually install `Adafruit_WINC1500`.</span></span> <span data-ttu-id="013a7-210">Gå till [webbplatsen](https://github.com/adafruit/Adafruit_WINC1500) och på **kloning eller hämta** > **hämta ZIP**.</span><span class="sxs-lookup"><span data-stu-id="013a7-210">Go to [this website](https://github.com/adafruit/Adafruit_WINC1500) and click **Clone or download** > **Download ZIP**.</span></span> <span data-ttu-id="013a7-211">Gå till i din Arduino IDE **skiss** > **innehåller biblioteket** > **lägger du till .zip biblioteket** och Lägg till zip-filen.</span><span class="sxs-lookup"><span data-stu-id="013a7-211">Then in your Arduino IDE, go to **Sketch** > **Include Library** > **Add .zip Library** and add the zip file.</span></span>

### <a name="use-the-sample-application-if-you-dont-have-a-real-bme280-sensor"></a><span data-ttu-id="013a7-212">Använd exempelprogrammet om du inte har en verklig BME280 sensor</span><span class="sxs-lookup"><span data-stu-id="013a7-212">Use the sample application if you don’t have a real BME280 sensor</span></span>

<span data-ttu-id="013a7-213">Om du inte har en verklig BME280 sensor simulera exempelprogrammet temperatur- och fuktighetskonsekvens data.</span><span class="sxs-lookup"><span data-stu-id="013a7-213">If you don’t have a real BME280 sensor, the sample application can simulate temperature and humidity data.</span></span> <span data-ttu-id="013a7-214">Följ dessa steg om du vill konfigurera exempelprogrammet använda simulerade data:</span><span class="sxs-lookup"><span data-stu-id="013a7-214">To set up the sample application to use simulated data, follow these steps:</span></span>

1. <span data-ttu-id="013a7-215">Öppna den `config.h` filen i den `app` mapp.</span><span class="sxs-lookup"><span data-stu-id="013a7-215">Open the `config.h` file in the `app` folder.</span></span>

2. <span data-ttu-id="013a7-216">Leta upp följande kodrad och ändrar du värdet från `false` till `true`:</span><span class="sxs-lookup"><span data-stu-id="013a7-216">Locate the following line of code and change the value from `false` to `true`:</span></span>

   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurera exempelprogrammet använda simulerade data](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. <span data-ttu-id="013a7-218">Spara filen med `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="013a7-218">Save the file with `Control-s`.</span></span>

### <a name="deploy-the-sample-application-to-feather-m0-wifi"></a><span data-ttu-id="013a7-219">Distribuera exempelprogrammet till ludd M0 WiFi</span><span class="sxs-lookup"><span data-stu-id="013a7-219">Deploy the sample application to Feather M0 WiFi</span></span>

1. <span data-ttu-id="013a7-220">Klicka i IDE Arduino **verktyget** > **Port**, och klicka sedan på den seriella porten för Ludd M0 WiFi.</span><span class="sxs-lookup"><span data-stu-id="013a7-220">In the Arduino IDE, click **Tool** > **Port**, and then click the serial port for Feather M0 WiFi.</span></span>

2. <span data-ttu-id="013a7-221">Klicka på **skiss** > **överför** att skapa och distribuera exempelprogrammet till ludd M0 WiFi.</span><span class="sxs-lookup"><span data-stu-id="013a7-221">Click **Sketch** > **Upload** to build and deploy the sample application to Feather M0 WiFi.</span></span>

### <a name="enter-your-credentials"></a><span data-ttu-id="013a7-222">Ange autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="013a7-222">Enter your credentials</span></span>

<span data-ttu-id="013a7-223">När överföringen är klar följer du stegen nedan för att ange dina autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="013a7-223">After the upload completes successfully, follow these steps to enter your credentials:</span></span>

1. <span data-ttu-id="013a7-224">Klicka i IDE Arduino **verktyg** > **seriella övervakaren**.</span><span class="sxs-lookup"><span data-stu-id="013a7-224">In the Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>

2. <span data-ttu-id="013a7-225">I det nedre högra hörnet i fönstret seriella väljer **ingen rad avslutas** i den nedrullningsbara listan till vänster.</span><span class="sxs-lookup"><span data-stu-id="013a7-225">In the lower-right corner of the serial monitor window, select **No line ending** in the drop-down list on the left.</span></span>
3. <span data-ttu-id="013a7-226">Välj **115200 baud** i den nedrullningsbara listan till höger.</span><span class="sxs-lookup"><span data-stu-id="013a7-226">Select **115200 baud** in the drop-down list on the right.</span></span>
4. <span data-ttu-id="013a7-227">Ange följande information i textrutan längst upp om du uppmanas att ange den och klicka på **skicka**:</span><span class="sxs-lookup"><span data-stu-id="013a7-227">In the input box at the top, enter the following information if you're asked to provide it, and click **Send**:</span></span>

   * <span data-ttu-id="013a7-228">Wi-Fi SSID</span><span class="sxs-lookup"><span data-stu-id="013a7-228">Wi-Fi SSID</span></span>
   * <span data-ttu-id="013a7-229">Wi-Fi-lösenord</span><span class="sxs-lookup"><span data-stu-id="013a7-229">Wi-Fi password</span></span>
   * <span data-ttu-id="013a7-230">Anslutningssträngen för enhet</span><span class="sxs-lookup"><span data-stu-id="013a7-230">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="013a7-231">Information om autentiseringsuppgifter lagras i EEPROM av ludd M0 WiFi.</span><span class="sxs-lookup"><span data-stu-id="013a7-231">The credential information is stored in the EEPROM of Feather M0 WiFi.</span></span> <span data-ttu-id="013a7-232">Om du klickar på återställningsknappen på ludd M0 WiFi-kort frågar exempelprogrammet om du vill radera informationen.</span><span class="sxs-lookup"><span data-stu-id="013a7-232">If you click the reset button on the Feather M0 WiFi board, the sample application asks if you want to erase the information.</span></span> <span data-ttu-id="013a7-233">Ange `Y` att radera informationen.</span><span class="sxs-lookup"><span data-stu-id="013a7-233">Enter `Y` to erase the information.</span></span> <span data-ttu-id="013a7-234">Du uppmanas att ange informationen som en andra gång.</span><span class="sxs-lookup"><span data-stu-id="013a7-234">You're asked to provide the information a second time.</span></span>

### <a name="verify-that-the-sample-application-is-running-successfully"></a><span data-ttu-id="013a7-235">Kontrollera att exempelprogrammet körts</span><span class="sxs-lookup"><span data-stu-id="013a7-235">Verify that the sample application is running successfully</span></span>

<span data-ttu-id="013a7-236">Om du ser i följande utdata från den seriella övervakningsfönstret och blinkande Indikator på ludd M0 WiFi exempelprogrammet körts:</span><span class="sxs-lookup"><span data-stu-id="013a7-236">If you see the following output from the serial monitor window and the blinking LED on Feather M0 WiFi, the sample application is running successfully:</span></span>

![Slutversionen i Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="013a7-238">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="013a7-238">Next steps</span></span>

<span data-ttu-id="013a7-239">Du har anslutna ludd M0 WiFi till din IoT-hubb och skickas avbildade sensordata till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="013a7-239">You have successfully connected Feather M0 WiFi to your IoT hub and sent the captured sensor data to your IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

