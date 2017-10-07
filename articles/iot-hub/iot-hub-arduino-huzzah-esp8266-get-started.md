---
title: aaaESP8266 toocloud - ansluta ludd HUZZAH ESP8266 tooAzure IoT-hubb | Microsoft Docs
description: "Lär dig hur toosetup och ansluta Adafruit ludd HUZZAH ESP8266 tooAzure IoT-hubb för den toosend dataplattform toohello Azure-molnet i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: c505aacf-89a8-40ed-a853-493b75bec524
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: xshi
ms.openlocfilehash: 44fd47232488948d21c7aa71bdd865397e41e63e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-adafruit-feather-huzzah-esp8266-tooazure-iot-hub-in-hello-cloud"></a><span data-ttu-id="d1fc0-103">Ansluta Adafruit ludd HUZZAH ESP8266 tooAzure IoT-hubb i hello moln</span><span class="sxs-lookup"><span data-stu-id="d1fc0-103">Connect Adafruit Feather HUZZAH ESP8266 tooAzure IoT Hub in hello cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Anslutningen mellan DHT22 och ludd HUZZAH ESP8266 IoT-hubb](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a><span data-ttu-id="d1fc0-105">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="d1fc0-105">What you do</span></span>


<span data-ttu-id="d1fc0-106">Ansluta Adafruit ludd HUZZAH ESP8266 tooan IoT-hubb som du skapar.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-106">Connect Adafruit Feather HUZZAH ESP8266 tooan IoT hub that you create.</span></span> <span data-ttu-id="d1fc0-107">Sedan kör du ett exempelprogram på ESP8266 toocollect hello temperatur- och fuktighetskonsekvens data från en DHT22 sensor.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-107">Then you run a sample application on ESP8266 toocollect hello temperature and humidity data from a DHT22 sensor.</span></span> <span data-ttu-id="d1fc0-108">Slutligen kan skicka du hello sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-108">Finally, you send hello sensor data tooyour IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="d1fc0-109">Om du använder andra ESP8266 anslagstavlor fortfarande följer du dessa steg tooconnect den tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-109">If you're using other ESP8266 boards, you can still follow these steps tooconnect it tooyour IoT hub.</span></span> <span data-ttu-id="d1fc0-110">Beroende på hello ESP8266 ändringar som du använder, kanske du måste tooreconfigure hello `LED_PIN`.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-110">Depending on hello ESP8266 board you're using, you might need tooreconfigure hello `LED_PIN`.</span></span> <span data-ttu-id="d1fc0-111">Till exempel, om du använder ESP8266 från AI-Thinker kan du ändra det från `0` för`2`.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-111">For example, if you're using ESP8266 from AI-Thinker, you might change it from `0` too`2`.</span></span> <span data-ttu-id="d1fc0-112">Har du en kit ännu?</span><span class="sxs-lookup"><span data-stu-id="d1fc0-112">Don't have a kit yet?</span></span> <span data-ttu-id="d1fc0-113">Hämta det från hello [Azure-webbplatsen](http://azure.com/iotstarterkits).</span><span class="sxs-lookup"><span data-stu-id="d1fc0-113">Get it from hello [Azure website](http://azure.com/iotstarterkits).</span></span>




## <a name="what-you-learn"></a><span data-ttu-id="d1fc0-114">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="d1fc0-114">What you learn</span></span>

* <span data-ttu-id="d1fc0-115">Hur toocreate en IoT-hubb och registrera en enhet för Ludd HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="d1fc0-115">How toocreate an IoT hub and register a device for Feather HUZZAH ESP8266</span></span>
* <span data-ttu-id="d1fc0-116">Hur tooconnect ludd HUZZAH ESP8266 med hello sensor och datorn</span><span class="sxs-lookup"><span data-stu-id="d1fc0-116">How tooconnect Feather HUZZAH ESP8266 with hello sensor and your computer</span></span>
* <span data-ttu-id="d1fc0-117">Hur toocollect sensordata genom att köra ett exempelprogram på ludd HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="d1fc0-117">How toocollect sensor data by running a sample application on Feather HUZZAH ESP8266</span></span>
* <span data-ttu-id="d1fc0-118">Hur toosend hello sensor data tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="d1fc0-118">How toosend hello sensor data tooyour IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="d1fc0-119">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="d1fc0-119">What you need</span></span>

![delar som behövs för hello självstudiekursen](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="d1fc0-121">toocomplete den här åtgärden måste följande delar från ludd för HUZZAH ESP8266 startpaket hello:</span><span class="sxs-lookup"><span data-stu-id="d1fc0-121">toocomplete this operation, you need hello following parts from your Feather HUZZAH ESP8266 Starter Kit:</span></span>

* <span data-ttu-id="d1fc0-122">hello ludd HUZZAH ESP8266-kort</span><span class="sxs-lookup"><span data-stu-id="d1fc0-122">hello Feather HUZZAH ESP8266 board</span></span>
* <span data-ttu-id="d1fc0-123">Micro USB-tooType en USB-kabel</span><span class="sxs-lookup"><span data-stu-id="d1fc0-123">A Micro USB tooType A USB cable</span></span>

<span data-ttu-id="d1fc0-124">Du måste också följande saker för din utvecklingsmiljö hello:</span><span class="sxs-lookup"><span data-stu-id="d1fc0-124">You also need hello following things for your development environment:</span></span>

* <span data-ttu-id="d1fc0-125">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-125">An active Azure subscription.</span></span> <span data-ttu-id="d1fc0-126">Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-126">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="d1fc0-127">Eller Mac-dator som kör Windows eller Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-127">Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="d1fc0-128">Ludd HUZZAH ESP8266 tooconnect till trådlöst nätverk.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-128">Wireless network for Feather HUZZAH ESP8266 tooconnect to.</span></span>
* <span data-ttu-id="d1fc0-129">Internet-anslutning toodownload hello-konfigurationsverktyget.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-129">Internet connection toodownload hello configuration tool.</span></span>
* <span data-ttu-id="d1fc0-130">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-130">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 or later.</span></span> <span data-ttu-id="d1fc0-131">Tidigare versioner fungerar inte med hello AzureIoT bibliotek.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-131">Earlier versions don't work with hello AzureIoT library.</span></span>

<span data-ttu-id="d1fc0-132">hello följande objekt är valfritt om du inte har en sensor.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-132">hello following items are optional in case you don’t have a sensor.</span></span> <span data-ttu-id="d1fc0-133">Du kan också ha hello möjlighet att använda simulerade sensordata.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-133">You also have hello option of using simulated sensor data.</span></span>

* <span data-ttu-id="d1fc0-134">En Adafruit DHT22 temperatur- och fuktighetskonsekvens sensor</span><span class="sxs-lookup"><span data-stu-id="d1fc0-134">An Adafruit DHT22 temperature and humidity sensor</span></span>
* <span data-ttu-id="d1fc0-135">En breadboard</span><span class="sxs-lookup"><span data-stu-id="d1fc0-135">A breadboard</span></span>
* <span data-ttu-id="d1fc0-136">M/M omkopplare kablar</span><span class="sxs-lookup"><span data-stu-id="d1fc0-136">M/M jumper wires</span></span>


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-hello-sensor-and-your-computer"></a><span data-ttu-id="d1fc0-137">Anslut ludd HUZZAH ESP8266 med hello sensor och din dator</span><span class="sxs-lookup"><span data-stu-id="d1fc0-137">Connect Feather HUZZAH ESP8266 with hello sensor and your computer</span></span>
<span data-ttu-id="d1fc0-138">I det här avsnittet kan du ansluta hello sensorer tooyour kort.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-138">In this section, you connect hello sensors tooyour board.</span></span> <span data-ttu-id="d1fc0-139">Sedan ansluter du enheten tooyour datorn för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-139">Then you plug in your device tooyour computer for further use.</span></span>
### <a name="connect-a-dht22-temperature-and-humidity-sensor-toofeather-huzzah-esp8266"></a><span data-ttu-id="d1fc0-140">Ansluta en DHT22 temperatur- och fuktighetskonsekvens sensor tooFeather HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="d1fc0-140">Connect a DHT22 temperature and humidity sensor tooFeather HUZZAH ESP8266</span></span>

<span data-ttu-id="d1fc0-141">Använd hello breadboard och omkopplare kablar toomake hello-anslutning på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-141">Use hello breadboard and jumper wires toomake hello connection as follows.</span></span> <span data-ttu-id="d1fc0-142">Om du inte har en sensor hoppa över det här avsnittet, eftersom du kan använda simulerade sensordata i stället.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-142">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![Referens för anslutningar](media/iot-hub-arduino-huzzah-esp8266-get-started/15_connections_on_breadboard.png)


<span data-ttu-id="d1fc0-144">För sensor PIN-koder, använder du följande kablar hello:</span><span class="sxs-lookup"><span data-stu-id="d1fc0-144">For sensor pins, use hello following wiring:</span></span>


| <span data-ttu-id="d1fc0-145">Starta (Sensor)</span><span class="sxs-lookup"><span data-stu-id="d1fc0-145">Start (Sensor)</span></span>           | <span data-ttu-id="d1fc0-146">END (moderkort)</span><span class="sxs-lookup"><span data-stu-id="d1fc0-146">End (Board)</span></span>           | <span data-ttu-id="d1fc0-147">Kabel färg</span><span class="sxs-lookup"><span data-stu-id="d1fc0-147">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="d1fc0-148">VDD (PIN-kod 31F)</span><span class="sxs-lookup"><span data-stu-id="d1fc0-148">VDD (Pin 31F)</span></span>            | <span data-ttu-id="d1fc0-149">3v (fästa 58H)</span><span class="sxs-lookup"><span data-stu-id="d1fc0-149">3V (Pin 58H)</span></span>           | <span data-ttu-id="d1fc0-150">Röd kabel</span><span class="sxs-lookup"><span data-stu-id="d1fc0-150">Red cable</span></span>     |
| <span data-ttu-id="d1fc0-151">DATA (PIN-kod 32F)</span><span class="sxs-lookup"><span data-stu-id="d1fc0-151">DATA (Pin 32F)</span></span>           | <span data-ttu-id="d1fc0-152">GPIO 2 (PIN-kod 46A)</span><span class="sxs-lookup"><span data-stu-id="d1fc0-152">GPIO 2 (Pin 46A)</span></span>       | <span data-ttu-id="d1fc0-153">Blå kabeln</span><span class="sxs-lookup"><span data-stu-id="d1fc0-153">Blue cable</span></span>    |
| <span data-ttu-id="d1fc0-154">JORD (PIN-kod 34F)</span><span class="sxs-lookup"><span data-stu-id="d1fc0-154">GND (Pin 34F)</span></span>            | <span data-ttu-id="d1fc0-155">JORD (PIN-kod 56I)</span><span class="sxs-lookup"><span data-stu-id="d1fc0-155">GND (PIn 56I)</span></span>          | <span data-ttu-id="d1fc0-156">Svart kabel</span><span class="sxs-lookup"><span data-stu-id="d1fc0-156">Black cable</span></span>   |

<span data-ttu-id="d1fc0-157">Mer information finns i [Adafruit DHT22 sensor installationsprogrammet](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) och [Adafruit ludd HUZZAH Esp8266 Pinouts](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).</span><span class="sxs-lookup"><span data-stu-id="d1fc0-157">For more information, see [Adafruit DHT22 sensor setup](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) and [Adafruit Feather HUZZAH Esp8266 Pinouts](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).</span></span>



<span data-ttu-id="d1fc0-158">Nu bör din ludd Huzzah ESP8266 anslutas med en fungerar sensor.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-158">Now your Feather Huzzah ESP8266 should be connected with a working sensor.</span></span>

![Anslut DHT22 med ludd Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-tooyour-computer"></a><span data-ttu-id="d1fc0-160">Ansluta ludd HUZZAH ESP8266 tooyour dator</span><span class="sxs-lookup"><span data-stu-id="d1fc0-160">Connect Feather HUZZAH ESP8266 tooyour computer</span></span>

<span data-ttu-id="d1fc0-161">Använda hello Micro USB tooType en USB-kabel tooconnect ludd HUZZAH ESP8266 tooyour dator visas nästa.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-161">As shown next, use hello Micro USB tooType A USB cable tooconnect Feather HUZZAH ESP8266 tooyour computer.</span></span>

![Ansluta ludd Huzzah tooyour dator](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a><span data-ttu-id="d1fc0-163">Lägg till behörigheter för seriell port (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="d1fc0-163">Add serial port permissions (Ubuntu only)</span></span>


<span data-ttu-id="d1fc0-164">Om du använder Ubuntu, kontrollera att du har hello behörigheter toooperate på hello USB-port för Ludd HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-164">If you use Ubuntu, make sure you have hello permissions toooperate on hello USB port of Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="d1fc0-165">tooadd serieport behörigheter så här:</span><span class="sxs-lookup"><span data-stu-id="d1fc0-165">tooadd serial port permissions, follow these steps:</span></span>


1. <span data-ttu-id="d1fc0-166">Kör följande kommandon i en terminal hello:</span><span class="sxs-lookup"><span data-stu-id="d1fc0-166">Run hello following commands at a terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="d1fc0-167">Du får ett av hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="d1fc0-167">You get one of hello following outputs:</span></span>

   * <span data-ttu-id="d1fc0-168">crw-rw---1 rot uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="d1fc0-168">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="d1fc0-169">crw-rw---1 rot antingen xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="d1fc0-169">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="d1fc0-170">Hello utdata och notera att `uucp` eller `dialout` är hello ägare gruppnamn hello USB-port.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-170">In hello output, notice that `uucp` or `dialout` is hello group owner name of hello USB port.</span></span>

1. <span data-ttu-id="d1fc0-171">Lägg till hello toohello användargrupp genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="d1fc0-171">Add hello user toohello group by running hello following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="d1fc0-172">`<group-owner-name>`är hello ägare gruppnamn du fick i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-172">`<group-owner-name>` is hello group owner name you obtained in hello previous step.</span></span> <span data-ttu-id="d1fc0-173">`<username>`är din Ubuntu användarnamn.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-173">`<username>` is your Ubuntu user name.</span></span>

1. <span data-ttu-id="d1fc0-174">Logga ut från Ubuntu och loggar sedan in igen för hello ändra tooappear.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-174">Sign out of Ubuntu, and then sign in again for hello change tooappear.</span></span>

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a><span data-ttu-id="d1fc0-175">Samla in sensordata och skicka den tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="d1fc0-175">Collect sensor data and send it tooyour IoT hub</span></span>

<span data-ttu-id="d1fc0-176">I det här avsnittet, distribuera och köra ett exempelprogram på ludd HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-176">In this section, you deploy and run a sample application on Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="d1fc0-177">hello exempelprogrammet blinkar hello Indikator på ludd HUZZAH ESP8266 och skickar hello temperatur och fuktighet data som samlas in från hello DHT22 sensor tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-177">hello sample application blinks hello LED on Feather HUZZAH ESP8266, and sends hello temperature and humidity data collected from hello DHT22 sensor tooyour IoT hub.</span></span>

### <a name="get-hello-sample-application-from-github"></a><span data-ttu-id="d1fc0-178">Hämta exempelprogrammet hello från GitHub</span><span class="sxs-lookup"><span data-stu-id="d1fc0-178">Get hello sample application from GitHub</span></span>

<span data-ttu-id="d1fc0-179">hello exempelprogrammet finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-179">hello sample application is hosted on GitHub.</span></span> <span data-ttu-id="d1fc0-180">Klona hello exempel lagringsplats som innehåller hello exempelprogrammet från GitHub.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-180">Clone hello sample repository that contains hello sample application from GitHub.</span></span> <span data-ttu-id="d1fc0-181">tooclone hello exempel lagringsplatsen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="d1fc0-181">tooclone hello sample repository, follow these steps:</span></span>

1. <span data-ttu-id="d1fc0-182">Öppna en kommandotolk eller ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-182">Open a command prompt or a terminal window.</span></span>
1. <span data-ttu-id="d1fc0-183">Gå tooa mappen där du vill att hello exempel programmet toobe lagras.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-183">Go tooa folder where you want hello sample application toobe stored.</span></span>
1. <span data-ttu-id="d1fc0-184">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="d1fc0-184">Run hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

<span data-ttu-id="d1fc0-185">Installera hello paketet för Ludd HUZZAH ESP8266 i hello Arduino IDE:</span><span class="sxs-lookup"><span data-stu-id="d1fc0-185">Install hello package for Feather HUZZAH ESP8266 in hello Arduino IDE:</span></span>

1. <span data-ttu-id="d1fc0-186">Öppna hello mappen där hello exempelprogrammet lagras.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-186">Open hello folder where hello sample application is stored.</span></span>
1. <span data-ttu-id="d1fc0-187">Öppna hello app.ino filen i mappen för hello-app i hello Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-187">Open hello app.ino file in hello app folder in hello Arduino IDE.</span></span>

   ![Öppna hello exempelprogrammet i Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/10_arduino-ide-open-sample-app.png)

1. <span data-ttu-id="d1fc0-189">Klicka på hello Arduino IDE **filen** > **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-189">In hello Arduino IDE, click **File** > **Preferences**.</span></span>
1. <span data-ttu-id="d1fc0-190">I hello **inställningar** dialogrutan klickar du på hello ikonen nästa toohello **ytterligare anslagstavlor Manager URL: er** rutan.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-190">In hello **Preferences** dialog box, click hello icon next toohello **Additional Boards Manager URLs** box.</span></span>
1. <span data-ttu-id="d1fc0-191">Ange hello följande URL i hello popup-fönster, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-191">In hello pop-up window, enter hello following URL, and then click **OK**.</span></span>

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Peka tooa webbadressen i Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/11_arduino-ide-package-url.png)

1. <span data-ttu-id="d1fc0-193">I hello **inställningar** dialogrutan klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-193">In hello **Preference** dialog box, click **OK**.</span></span>
1. <span data-ttu-id="d1fc0-194">Klicka på **verktyg** > **Hanteringsstyrenhetens** > **anslagstavlor Manager**, och sök sedan efter esp8266.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-194">Click **Tools** > **Board** > **Boards Manager**, and then search for esp8266.</span></span>

   <span data-ttu-id="d1fc0-195">Anslagstavlor Manager anger att ESP8266 med en version av 2.2.0 eller senare är installerad.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-195">Boards Manager indicates that ESP8266 with a version of 2.2.0 or later is installed.</span></span>

   ![Hej esp8266 paketet har installerats](media/iot-hub-arduino-huzzah-esp8266-get-started/12_arduino-ide-esp8266-installed.png)

1. <span data-ttu-id="d1fc0-197">Klicka på **verktyg** > **Hanteringsstyrenhetens** > **Adafruit HUZZAH ESP8266**.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-197">Click **Tools** > **Board** > **Adafruit HUZZAH ESP8266**.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="d1fc0-198">Installera nödvändiga bibliotek</span><span class="sxs-lookup"><span data-stu-id="d1fc0-198">Install necessary libraries</span></span>

1. <span data-ttu-id="d1fc0-199">I hello Arduino IDE, klickar du på **skiss** > **innehåller biblioteket** > **Hantera bibliotek**.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-199">In hello Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>
1. <span data-ttu-id="d1fc0-200">Sök efter hello efter namn på biblioteket i taget.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-200">Search for hello following library names one by one.</span></span> <span data-ttu-id="d1fc0-201">Klicka på för alla bibliotek som du hittar **installera**.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-201">For each  library that you find, click **Install**.</span></span>
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a><span data-ttu-id="d1fc0-202">Har en verklig DHT22 sensor?</span><span class="sxs-lookup"><span data-stu-id="d1fc0-202">Don’t have a real DHT22 sensor?</span></span>

<span data-ttu-id="d1fc0-203">hello exempelprogrammet simulera temperatur- och fuktighetskonsekvens data om du inte har en verklig DHT22 sensor.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-203">hello sample application can simulate temperature and humidity data in case you don’t have a real DHT22 sensor.</span></span> <span data-ttu-id="d1fc0-204">tooset in hello programmet toouse simulerade exempeldata, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="d1fc0-204">tooset up hello sample application toouse simulated data, follow these steps:</span></span>

1. <span data-ttu-id="d1fc0-205">Öppna hello `config.h` filen i hello `app` mapp.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-205">Open hello `config.h` file in hello `app` folder.</span></span>
1. <span data-ttu-id="d1fc0-206">Leta upp följande kodrad hello och ändrar hello-värdet från `false` för`true`:</span><span class="sxs-lookup"><span data-stu-id="d1fc0-206">Locate hello following line of code and change hello value from `false` too`true`:</span></span>
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurera hello exempeldata programmet toouse simulerade](media/iot-hub-arduino-huzzah-esp8266-get-started/13_arduino-ide-configure-app-use-simulated-data.png)

1. <span data-ttu-id="d1fc0-208">Spara hello-filen med `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-208">Save hello file with `Control-s`.</span></span>

### <a name="deploy-hello-sample-application-toofeather-huzzah-esp8266"></a><span data-ttu-id="d1fc0-209">Distribuera hello exempel programmet tooFeather HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="d1fc0-209">Deploy hello sample application tooFeather HUZZAH ESP8266</span></span>

1. <span data-ttu-id="d1fc0-210">Klicka på hello Arduino IDE **verktyget** > **Port**, och klicka sedan på hello seriell port för Ludd HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-210">In hello Arduino IDE, click **Tool** > **Port**, and then click hello serial port for Feather HUZZAH ESP8266.</span></span>
1. <span data-ttu-id="d1fc0-211">Klicka på **skiss** > **överför** toobuild och distribuera hello exempel programmet tooFeather HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-211">Click **Sketch** > **Upload** toobuild and deploy hello sample application tooFeather HUZZAH ESP8266.</span></span>

### <a name="enter-your-credentials"></a><span data-ttu-id="d1fc0-212">Ange autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="d1fc0-212">Enter your credentials</span></span>

<span data-ttu-id="d1fc0-213">När hello överföringen är klar följer du dessa steg tooenter dina autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="d1fc0-213">After hello upload completes successfully, follow these steps tooenter your credentials:</span></span>

1. <span data-ttu-id="d1fc0-214">Klicka på hello Arduino IDE **verktyg** > **seriella övervakaren**.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-214">In hello Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>
1. <span data-ttu-id="d1fc0-215">Observera hello två listrutorna i hello nedre högra hörnet i hello seriella övervakningsfönstret.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-215">In hello serial monitor window, notice hello two drop-down lists in hello lower-right corner.</span></span>
1. <span data-ttu-id="d1fc0-216">Välj **ingen rad avslutas** för hello vänstra nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-216">Select **No line ending** for hello left drop-down list.</span></span>
1. <span data-ttu-id="d1fc0-217">Välj **115200 baud** för hello höger listrutan.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-217">Select **115200 baud** for hello right drop-down list.</span></span>
1. <span data-ttu-id="d1fc0-218">Ange hello efter information om du uppmanas tooprovide i hello-textrutan finns hello överst i hello seriella övervakningsfönstret dem, och klicka sedan på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-218">In hello input box located at hello top of hello serial monitor window, enter hello following information if you are asked tooprovide them, and then click **Send**.</span></span>
   * <span data-ttu-id="d1fc0-219">Wi-Fi SSID</span><span class="sxs-lookup"><span data-stu-id="d1fc0-219">Wi-Fi SSID</span></span>
   * <span data-ttu-id="d1fc0-220">Wi-Fi-lösenord</span><span class="sxs-lookup"><span data-stu-id="d1fc0-220">Wi-Fi password</span></span>
   * <span data-ttu-id="d1fc0-221">Anslutningssträngen för enhet</span><span class="sxs-lookup"><span data-stu-id="d1fc0-221">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="d1fc0-222">information om hello autentiseringsuppgifter lagras i hello EEPROM av ludd HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-222">hello credential information is stored in hello EEPROM of Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="d1fc0-223">Om du klickar på hello Återställ-knappen i hello ludd HUZZAH ESP8266 board ombeds hello exempelprogrammet tooerase hello information.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-223">If you click hello reset button on hello Feather HUZZAH ESP8266 board, hello sample application asks if you want tooerase hello information.</span></span> <span data-ttu-id="d1fc0-224">Ange `Y` toohave hello information tas bort.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-224">Enter `Y` toohave hello information erased.</span></span> <span data-ttu-id="d1fc0-225">Du uppmanas tooprovide hello information en gång.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-225">You are asked tooprovide hello information a second time.</span></span>

### <a name="verify-hello-sample-application-is-running-successfully"></a><span data-ttu-id="d1fc0-226">Kontrollera hello exempelprogrammet körts</span><span class="sxs-lookup"><span data-stu-id="d1fc0-226">Verify hello sample application is running successfully</span></span>

<span data-ttu-id="d1fc0-227">Om du ser hello följande utdata från hello seriella övervakningsfönstret och hello blinka Indikator på ludd HUZZAH ESP8266, hello exempelprogrammet körts.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-227">If you see hello following output from hello serial monitor window and hello blinking LED on Feather HUZZAH ESP8266, hello sample application is running successfully.</span></span>

![Slutversionen i Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="d1fc0-229">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d1fc0-229">Next steps</span></span>

<span data-ttu-id="d1fc0-230">Du har anslutna ludd HUZZAH ESP8266 tooyour IoT-hubb, och skickas hello avbildas sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d1fc0-230">You have successfully connected a Feather HUZZAH ESP8266 tooyour IoT hub, and sent hello captured sensor data tooyour IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

