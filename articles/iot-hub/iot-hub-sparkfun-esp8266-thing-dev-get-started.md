---
title: aaaESP8266 toocloud - ansluta Sparkfun ESP8266 sak Dev tooAzure IoT-hubb | Microsoft Docs
description: "Lär dig hur toosetup och ansluta Sparkfun ESP8266 sak Dev tooAzure IoT-hubb för den toosend dataplattform toohello Azure-molnet i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 587fe292-9602-45b4-95ee-f39bba10e716
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 19b249df23b6df516634853521c6d532f51014da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-sparkfun-esp8266-thing-dev-tooazure-iot-hub-in-hello-cloud"></a><span data-ttu-id="043f7-103">Ansluta Sparkfun ESP8266 sak Dev tooAzure IoT-hubb i hello moln</span><span class="sxs-lookup"><span data-stu-id="043f7-103">Connect Sparkfun ESP8266 Thing Dev tooAzure IoT Hub in hello cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![anslutningen mellan DHT22 och sak Dev IoT-hubb](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a><span data-ttu-id="043f7-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="043f7-105">What you will do</span></span>

<span data-ttu-id="043f7-106">Ansluta Sparkfun ESP8266 sak Dev tooan IoT-hubb skapar du.</span><span class="sxs-lookup"><span data-stu-id="043f7-106">Connect Sparkfun ESP8266 Thing Dev tooan IoT hub you will create.</span></span> <span data-ttu-id="043f7-107">Kör sedan ett exempelprogram på ESP8266 toocollect temperatur- och fuktighetskonsekvens data från en DHT22 sensor.</span><span class="sxs-lookup"><span data-stu-id="043f7-107">Then run a sample application on ESP8266 toocollect temperature and humidity data from a DHT22 sensor.</span></span> <span data-ttu-id="043f7-108">Slutligen skicka hello sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="043f7-108">Finally, send hello sensor data tooyour IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="043f7-109">Om du använder andra ESP8266: er kan du fortfarande kan följa dessa steg tooconnect den tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="043f7-109">If you are using other ESP8266 boards, you can still follow these steps tooconnect it tooyour IoT hub.</span></span> <span data-ttu-id="043f7-110">Hej ESP8266 ändringar som du använder, måste du använda tooreconfigure hello `LED_PIN`.</span><span class="sxs-lookup"><span data-stu-id="043f7-110">Depending on hello ESP8266 board you are using, you may need tooreconfigure hello `LED_PIN`.</span></span> <span data-ttu-id="043f7-111">Till exempel om du använder ESP8266 från AI-Thinker, du kan ändra den från `0` för`2`.</span><span class="sxs-lookup"><span data-stu-id="043f7-111">For example, if you are using ESP8266 from AI-Thinker, you may change it from `0` too`2`.</span></span> <span data-ttu-id="043f7-112">Har ett kit än?: Klicka på [här](http://azure.com/iotstarterkits)</span><span class="sxs-lookup"><span data-stu-id="043f7-112">Don't have a kit yet?: Click [here](http://azure.com/iotstarterkits)</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="043f7-113">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="043f7-113">What you will learn</span></span>

* <span data-ttu-id="043f7-114">Hur toocreate en IoT-hubb och registrera en enhet för sak Utvecklartestning</span><span class="sxs-lookup"><span data-stu-id="043f7-114">How toocreate an IoT hub and register a device for Thing Dev.</span></span>
* <span data-ttu-id="043f7-115">Hur tooconnect sak Dev med hello sensor och din dator.</span><span class="sxs-lookup"><span data-stu-id="043f7-115">How tooconnect Thing Dev with hello sensor and your computer.</span></span>
* <span data-ttu-id="043f7-116">Hur toocollect sensordata genom att köra ett exempelprogram på sak Utvecklartestning</span><span class="sxs-lookup"><span data-stu-id="043f7-116">How toocollect sensor data by running a sample application on Thing Dev.</span></span>
* <span data-ttu-id="043f7-117">Hur hello toosend sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="043f7-117">How toosend hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="043f7-118">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="043f7-118">What you will need</span></span>

![delar som behövs för hello självstudiekursen](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="043f7-120">toocomplete den här åtgärden måste följande delar från din sak Dev startpaket hello:</span><span class="sxs-lookup"><span data-stu-id="043f7-120">toocomplete this operation, you need hello following parts from your Thing Dev Starter Kit:</span></span>

* <span data-ttu-id="043f7-121">Hej Sparkfun ESP8266 sak Dev-kort.</span><span class="sxs-lookup"><span data-stu-id="043f7-121">hello Sparkfun ESP8266 Thing Dev board.</span></span>
* <span data-ttu-id="043f7-122">Micro USB-tooType en USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="043f7-122">A Micro USB tooType A USB cable.</span></span>

<span data-ttu-id="043f7-123">Du måste också hello följande för din utvecklingsmiljö:</span><span class="sxs-lookup"><span data-stu-id="043f7-123">You also need hello following for your development environment:</span></span>

* <span data-ttu-id="043f7-124">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="043f7-124">An active Azure subscription.</span></span> <span data-ttu-id="043f7-125">Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="043f7-125">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="043f7-126">Eller Mac-dator som kör Windows eller Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="043f7-126">Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="043f7-127">Sparkfun ESP8266 sak Dev tooconnect till trådlöst nätverk.</span><span class="sxs-lookup"><span data-stu-id="043f7-127">Wireless network for Sparkfun ESP8266 Thing Dev tooconnect to.</span></span>
* <span data-ttu-id="043f7-128">Internet-anslutning toodownload hello-konfigurationsverktyget.</span><span class="sxs-lookup"><span data-stu-id="043f7-128">Internet connection toodownload hello configuration tool.</span></span>
* <span data-ttu-id="043f7-129">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 (eller senare), tidigare versioner fungerar inte med hello AzureIoT bibliotek.</span><span class="sxs-lookup"><span data-stu-id="043f7-129">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 (or newer), earlier versions will not work with hello AzureIoT library.</span></span>

<span data-ttu-id="043f7-130">hello följande objekt är valfritt om du inte har en sensor.</span><span class="sxs-lookup"><span data-stu-id="043f7-130">hello following items are optional in case you don’t have a sensor.</span></span> <span data-ttu-id="043f7-131">Du kan också ha hello möjlighet att använda simulerade sensordata.</span><span class="sxs-lookup"><span data-stu-id="043f7-131">You also have hello option of using simulated sensor data.</span></span>

* <span data-ttu-id="043f7-132">En Adafruit DHT22 temperatur- och fuktighetskonsekvens sensor.</span><span class="sxs-lookup"><span data-stu-id="043f7-132">An Adafruit DHT22 temperature and humidity sensor.</span></span>
* <span data-ttu-id="043f7-133">En breadboard.</span><span class="sxs-lookup"><span data-stu-id="043f7-133">A breadboard.</span></span>
* <span data-ttu-id="043f7-134">M/M omkopplare kablar.</span><span class="sxs-lookup"><span data-stu-id="043f7-134">M/M jumper wires.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-hello-sensor-and-your-computer"></a><span data-ttu-id="043f7-135">Anslut ESP8266 sak Dev med hello sensor och din dator</span><span class="sxs-lookup"><span data-stu-id="043f7-135">Connect ESP8266 Thing Dev with hello sensor and your computer</span></span>

### <a name="connect-a-dht22-temperature-and-humidity-sensor-tooesp8266-thing-dev"></a><span data-ttu-id="043f7-136">Ansluta en DHT22 temperatur- och fuktighetskonsekvens sensor tooESP8266 sak Dev</span><span class="sxs-lookup"><span data-stu-id="043f7-136">Connect a DHT22 temperature and humidity sensor tooESP8266 Thing Dev</span></span>

<span data-ttu-id="043f7-137">Använd hello breadboard och omkopplare kablar toomake hello-anslutning på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="043f7-137">Use hello breadboard and jumper wires toomake hello connection as follows.</span></span> <span data-ttu-id="043f7-138">Om du inte har en sensor hoppa över det här avsnittet, eftersom du kan använda simulerade sensordata i stället.</span><span class="sxs-lookup"><span data-stu-id="043f7-138">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![Referens för anslutningar](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

<span data-ttu-id="043f7-140">För sensor PIN-koder, kommer vi att använda följande kablar hello:</span><span class="sxs-lookup"><span data-stu-id="043f7-140">For sensor pins, we will use hello following wiring:</span></span>

| <span data-ttu-id="043f7-141">Starta (Sensor)</span><span class="sxs-lookup"><span data-stu-id="043f7-141">Start (Sensor)</span></span>           | <span data-ttu-id="043f7-142">END (moderkort)</span><span class="sxs-lookup"><span data-stu-id="043f7-142">End (Board)</span></span>           | <span data-ttu-id="043f7-143">Kabel färg</span><span class="sxs-lookup"><span data-stu-id="043f7-143">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="043f7-144">VDD (PIN-kod 27F)</span><span class="sxs-lookup"><span data-stu-id="043f7-144">VDD (Pin 27F)</span></span>            | <span data-ttu-id="043f7-145">3v (8A PIN-kod)</span><span class="sxs-lookup"><span data-stu-id="043f7-145">3V (Pin 8A)</span></span>           | <span data-ttu-id="043f7-146">Röd kabel</span><span class="sxs-lookup"><span data-stu-id="043f7-146">Red cable</span></span>     |
| <span data-ttu-id="043f7-147">DATA (PIN-kod 28 f)</span><span class="sxs-lookup"><span data-stu-id="043f7-147">DATA (Pin 28F)</span></span>           | <span data-ttu-id="043f7-148">GPIO 2 (9A PIN-kod)</span><span class="sxs-lookup"><span data-stu-id="043f7-148">GPIO 2 (Pin 9A)</span></span>       | <span data-ttu-id="043f7-149">Vita kabeln</span><span class="sxs-lookup"><span data-stu-id="043f7-149">White cable</span></span>    |
| <span data-ttu-id="043f7-150">JORD (PIN-kod 30 f)</span><span class="sxs-lookup"><span data-stu-id="043f7-150">GND (Pin 30F)</span></span>            | <span data-ttu-id="043f7-151">JORD (7J PIN-kod)</span><span class="sxs-lookup"><span data-stu-id="043f7-151">GND (Pin 7J)</span></span>          | <span data-ttu-id="043f7-152">Svart kabel</span><span class="sxs-lookup"><span data-stu-id="043f7-152">Black cable</span></span>   |


- <span data-ttu-id="043f7-153">Mer information finns: [DHT22 sensor installationsprogrammet](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) och [Sparkfun ESP8266 sak Dev-specifikationen](https://www.sparkfun.com/products/13711)</span><span class="sxs-lookup"><span data-stu-id="043f7-153">For more information, see: [DHT22 sensor setup](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) and [Sparkfun ESP8266 Thing Dev specification](https://www.sparkfun.com/products/13711)</span></span>

<span data-ttu-id="043f7-154">Nu bör din Sparkfun ESP8266 sak Dev anslutas med en fungerar sensor.</span><span class="sxs-lookup"><span data-stu-id="043f7-154">Now your Sparkfun ESP8266 Thing Dev should be connected with a working sensor.</span></span>

![Anslut dht22 med ESP8266 sak Dev](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-tooyour-computer"></a><span data-ttu-id="043f7-156">Ansluta Sparkfun ESP8266 sak Dev tooyour dator</span><span class="sxs-lookup"><span data-stu-id="043f7-156">Connect Sparkfun ESP8266 Thing Dev tooyour computer</span></span>

<span data-ttu-id="043f7-157">Använda hello Micro USB tooType en USB-kabel tooconnect Sparkfun ESP8266 sak Dev tooyour dator på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="043f7-157">Use hello Micro USB tooType A USB cable tooconnect Sparkfun ESP8266 Thing Dev tooyour computer as follows.</span></span>

![ansluta ludd huzzah tooyour dator](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a><span data-ttu-id="043f7-159">Lägg till behörigheter för seriell port – Ubuntu endast</span><span class="sxs-lookup"><span data-stu-id="043f7-159">Add serial port permissions – Ubuntu only</span></span>

<span data-ttu-id="043f7-160">Om du använder Ubuntu Kontrollera normal användare har hello behörigheter toooperate på hello USB-port för Sparkfun ESP8266 sak Utvecklartestning</span><span class="sxs-lookup"><span data-stu-id="043f7-160">If you use Ubuntu, make sure a normal user has hello permissions toooperate on hello USB port of Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="043f7-161">tooadd serieport behörigheter för en normal användare så här:</span><span class="sxs-lookup"><span data-stu-id="043f7-161">tooadd serial port permissions for a normal user, follow these steps:</span></span>

1. <span data-ttu-id="043f7-162">Kör följande kommandon i en terminal hello:</span><span class="sxs-lookup"><span data-stu-id="043f7-162">Run hello following commands at a terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="043f7-163">Du får ett av hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="043f7-163">You get one of hello following outputs:</span></span>

   * <span data-ttu-id="043f7-164">crw-rw---1 rot uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="043f7-164">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="043f7-165">crw-rw---1 rot antingen xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="043f7-165">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="043f7-166">Hello utdata och notera `uucp` eller `dialout` som hello ägare gruppnamnet för hello USB-port.</span><span class="sxs-lookup"><span data-stu-id="043f7-166">In hello output, notice `uucp` or `dialout` that is hello group owner name of hello USB port.</span></span>

1. <span data-ttu-id="043f7-167">Lägg till hello toohello användargrupp genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="043f7-167">Add hello user toohello group by running hello following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="043f7-168">`<group-owner-name>`är hello ägare gruppnamn du fick i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="043f7-168">`<group-owner-name>` is hello group owner name you obtained in hello previous step.</span></span> <span data-ttu-id="043f7-169">`<username>`är din Ubuntu användarnamn.</span><span class="sxs-lookup"><span data-stu-id="043f7-169">`<username>` is your Ubuntu user name.</span></span>

1. <span data-ttu-id="043f7-170">Logga ut Ubuntu och logga in det igen för hello ändra tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="043f7-170">Log out Ubuntu and log in it again for hello change tootake effect.</span></span>

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a><span data-ttu-id="043f7-171">Samla in sensordata och skicka den tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="043f7-171">Collect sensor data and send it tooyour IoT hub</span></span>

<span data-ttu-id="043f7-172">I det här avsnittet kan du distribuera och köra ett exempelprogram på Sparkfun ESP8266 sak Utvecklartestning</span><span class="sxs-lookup"><span data-stu-id="043f7-172">In this section, you deploy and run a sample application on Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="043f7-173">hello exempelprogrammet blinkar hello Indikator på Sparkfun ESP8266 sak Dev och skickar hello temperatur och fuktighet data som samlas in från hello DHT22 sensor tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="043f7-173">hello sample application blinks hello LED on Sparkfun ESP8266 Thing Dev and sends hello temperature and humidity data collected from hello DHT22 sensor tooyour IoT hub.</span></span>

### <a name="get-hello-sample-application-from-github"></a><span data-ttu-id="043f7-174">Hämta exempelprogrammet hello från GitHub</span><span class="sxs-lookup"><span data-stu-id="043f7-174">Get hello sample application from GitHub</span></span>

<span data-ttu-id="043f7-175">hello exempelprogrammet finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="043f7-175">hello sample application is hosted on GitHub.</span></span> <span data-ttu-id="043f7-176">Klona hello exempel lagringsplats som innehåller hello exempelprogrammet från GitHub.</span><span class="sxs-lookup"><span data-stu-id="043f7-176">Clone hello sample repository that contains hello sample application from GitHub.</span></span> <span data-ttu-id="043f7-177">tooclone hello exempel lagringsplatsen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="043f7-177">tooclone hello sample repository, follow these steps:</span></span>

1. <span data-ttu-id="043f7-178">Öppna en kommandotolk eller ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="043f7-178">Open a command prompt or a terminal window.</span></span>
1. <span data-ttu-id="043f7-179">Gå tooa mappen där du vill att hello exempel programmet toobe lagras.</span><span class="sxs-lookup"><span data-stu-id="043f7-179">Go tooa folder where you want hello sample application toobe stored.</span></span>
1. <span data-ttu-id="043f7-180">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="043f7-180">Run hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

<span data-ttu-id="043f7-181">Installera hello paketet för Sparkfun ESP8266 sak Dev i Arduino IDE:</span><span class="sxs-lookup"><span data-stu-id="043f7-181">Install hello package for Sparkfun ESP8266 Thing Dev in Arduino IDE:</span></span>

1. <span data-ttu-id="043f7-182">Öppna hello mappen där hello exempelprogrammet lagras.</span><span class="sxs-lookup"><span data-stu-id="043f7-182">Open hello folder where hello sample application is stored.</span></span>
1. <span data-ttu-id="043f7-183">Öppna hello app.ino filen i hello app mappen i Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="043f7-183">Open hello app.ino file in hello app folder in Arduino IDE.</span></span>

   ![Öppna hello exempelprogrammet i arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. <span data-ttu-id="043f7-185">Klicka på hello Arduino IDE **filen** > **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="043f7-185">In hello Arduino IDE, click **File** > **Preferences**.</span></span>
1. <span data-ttu-id="043f7-186">I hello **inställningar** dialogrutan klickar du på hello ikonen nästa toohello **ytterligare anslagstavlor Manager URL: er** textruta.</span><span class="sxs-lookup"><span data-stu-id="043f7-186">In hello **Preferences** dialog box, click hello icon next toohello **Additional Boards Manager URLs** text box.</span></span>
1. <span data-ttu-id="043f7-187">Ange hello följande URL i hello popup-fönster, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="043f7-187">In hello pop-up window, enter hello following URL, and then click **OK**.</span></span>

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Peka tooa webbadressen i arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. <span data-ttu-id="043f7-189">I hello **inställningar** dialogrutan klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="043f7-189">In hello **Preference** dialog box, click **OK**.</span></span>
1. <span data-ttu-id="043f7-190">Klicka på **verktyg** > **Hanteringsstyrenhetens** > **anslagstavlor Manager**, och sök sedan efter esp8266.</span><span class="sxs-lookup"><span data-stu-id="043f7-190">Click **Tools** > **Board** > **Boards Manager**, and then search for esp8266.</span></span>
   <span data-ttu-id="043f7-191">ESP8266 med en version av 2.2.0 eller senare ska installeras.</span><span class="sxs-lookup"><span data-stu-id="043f7-191">ESP8266 with a version of 2.2.0 or later should be installed.</span></span>

   ![Hej esp8266 paketet har installerats](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. <span data-ttu-id="043f7-193">Klicka på **verktyg** > **Hanteringsstyrenhetens** > **Sparkfun ESP8266 sak Dev**.</span><span class="sxs-lookup"><span data-stu-id="043f7-193">Click **Tools** > **Board** > **Sparkfun ESP8266 Thing Dev**.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="043f7-194">Installera nödvändiga bibliotek</span><span class="sxs-lookup"><span data-stu-id="043f7-194">Install necessary libraries</span></span>

1. <span data-ttu-id="043f7-195">I hello Arduino IDE, klickar du på **skiss** > **innehåller biblioteket** > **Hantera bibliotek**.</span><span class="sxs-lookup"><span data-stu-id="043f7-195">In hello Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>
1. <span data-ttu-id="043f7-196">Sök efter hello efter namn på biblioteket i taget.</span><span class="sxs-lookup"><span data-stu-id="043f7-196">Search for hello following library names one by one.</span></span> <span data-ttu-id="043f7-197">För var och en av hello-biblioteket som du hittar klickar **installera**.</span><span class="sxs-lookup"><span data-stu-id="043f7-197">For each of hello library you find, click **Install**.</span></span>
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a><span data-ttu-id="043f7-198">Har en verklig DHT22 sensor?</span><span class="sxs-lookup"><span data-stu-id="043f7-198">Don’t have a real DHT22 sensor?</span></span>

<span data-ttu-id="043f7-199">hello exempelprogrammet simulera temperatur- och fuktighetskonsekvens data om du inte har en verklig DHT22 sensor.</span><span class="sxs-lookup"><span data-stu-id="043f7-199">hello sample application can simulate temperature and humidity data in case you don’t have a real DHT22 sensor.</span></span> <span data-ttu-id="043f7-200">tooenable hello programmet toouse simulerade exempeldata, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="043f7-200">tooenable hello sample application toouse simulated data, follow these steps:</span></span>

1. <span data-ttu-id="043f7-201">Öppna hello `config.h` filen i hello `app` mapp.</span><span class="sxs-lookup"><span data-stu-id="043f7-201">Open hello `config.h` file in hello `app` folder.</span></span>
1. <span data-ttu-id="043f7-202">Leta upp följande kodrad hello och ändrar hello-värdet från `false` för`true`:</span><span class="sxs-lookup"><span data-stu-id="043f7-202">Locate hello following line of code and change hello value from `false` too`true`:</span></span>
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurera hello exempeldata programmet toouse simulerade](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. <span data-ttu-id="043f7-204">Spara med `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="043f7-204">Save with `Control-s`.</span></span>

### <a name="deploy-hello-sample-application-toosparkfun-esp8266-thing-dev"></a><span data-ttu-id="043f7-205">Distribuera hello exempel programmet tooSparkfun ESP8266 sak Dev</span><span class="sxs-lookup"><span data-stu-id="043f7-205">Deploy hello sample application tooSparkfun ESP8266 Thing Dev</span></span>

1. <span data-ttu-id="043f7-206">Klicka på hello Arduino IDE **verktyget** > **Port**, och klicka sedan på hello seriell port för Sparkfun ESP8266 sak Utvecklartestning</span><span class="sxs-lookup"><span data-stu-id="043f7-206">In hello Arduino IDE, click **Tool** > **Port**, and then click hello serial port for Sparkfun ESP8266 Thing Dev.</span></span>
1. <span data-ttu-id="043f7-207">Klicka på **skiss** > **överför** toobuild och distribuera hello exempel programmet tooSparkfun ESP8266 sak Utvecklartestning</span><span class="sxs-lookup"><span data-stu-id="043f7-207">Click **Sketch** > **Upload** toobuild and deploy hello sample application tooSparkfun ESP8266 Thing Dev.</span></span>

> [!Note]
> <span data-ttu-id="043f7-208">Om du använder macOS kunde du antagligen se hello efter meddelanden under överföring.</span><span class="sxs-lookup"><span data-stu-id="043f7-208">If you are using macOS you could probably see hello following messages during uploading.</span></span> <span data-ttu-id="043f7-209">`warning: espcomm_sync failed`,`error: espcomm_open failed`.</span><span class="sxs-lookup"><span data-stu-id="043f7-209">`warning: espcomm_sync failed`,`error: espcomm_open failed`.</span></span> <span data-ttu-id="043f7-210">Öppna fönstret ternimal och slutför under åtgärder toosolve problemet.</span><span class="sxs-lookup"><span data-stu-id="043f7-210">Please open your ternimal window and finish below actions toosolve this issue.</span></span>
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a><span data-ttu-id="043f7-211">Ange autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="043f7-211">Enter your credentials</span></span>

<span data-ttu-id="043f7-212">När hello överföringen är klar så hello steg tooenter dina autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="043f7-212">After hello upload completes successfully, follow hello steps tooenter your credentials:</span></span>

1. <span data-ttu-id="043f7-213">Klicka på hello Arduino IDE **verktyg** > **seriella övervakaren**.</span><span class="sxs-lookup"><span data-stu-id="043f7-213">In hello Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>
1. <span data-ttu-id="043f7-214">Observera hello två listrutorna på hello nedre högra hörnet i hello seriella övervakningsfönstret.</span><span class="sxs-lookup"><span data-stu-id="043f7-214">In hello serial monitor window, notice hello two drop-down lists on hello bottom right corner.</span></span>
1. <span data-ttu-id="043f7-215">Välj **ingen rad avslutas** för hello vänstra nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="043f7-215">Select **No line ending** for hello left drop-down list.</span></span>
1. <span data-ttu-id="043f7-216">Välj **115200 baud** för hello höger listrutan.</span><span class="sxs-lookup"><span data-stu-id="043f7-216">Select **115200 baud** for hello right drop-down list.</span></span>
1. <span data-ttu-id="043f7-217">Ange hello efter information om du uppmanas tooprovide i hello-textrutan finns hello överst i hello seriella övervakningsfönstret dem, och klicka sedan på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="043f7-217">In hello input box located at hello top of hello serial monitor window, enter hello following information if you are asked tooprovide them, and then click **Send**.</span></span>
   * <span data-ttu-id="043f7-218">Wi-Fi SSID</span><span class="sxs-lookup"><span data-stu-id="043f7-218">Wi-Fi SSID</span></span>
   * <span data-ttu-id="043f7-219">Wi-Fi-lösenord</span><span class="sxs-lookup"><span data-stu-id="043f7-219">Wi-Fi password</span></span>
   * <span data-ttu-id="043f7-220">Anslutningssträngen för enhet</span><span class="sxs-lookup"><span data-stu-id="043f7-220">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="043f7-221">information om hello autentiseringsuppgifter lagras i hello EEPROM av Sparkfun ESP8266 sak Utvecklartestning</span><span class="sxs-lookup"><span data-stu-id="043f7-221">hello credential information is stored in hello EEPROM of Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="043f7-222">Om du klickar på hello Återställ-knappen i hello Sparkfun ESP8266 sak Dev board frågar hello exempelprogrammet om du vill ha tooerase hello information.</span><span class="sxs-lookup"><span data-stu-id="043f7-222">If you click hello reset button on hello Sparkfun ESP8266 Thing Dev board, hello sample application asks you if you want tooerase hello information.</span></span> <span data-ttu-id="043f7-223">Ange `Y` toohave hello information tas bort och du ombeds tooprovide hello informationen igen.</span><span class="sxs-lookup"><span data-stu-id="043f7-223">Enter `Y` toohave hello information erased and you are asked tooprovide hello information again.</span></span>

### <a name="verify-hello-sample-application-is-running-successfully"></a><span data-ttu-id="043f7-224">Kontrollera hello exempelprogrammet körts</span><span class="sxs-lookup"><span data-stu-id="043f7-224">Verify hello sample application is running successfully</span></span>

<span data-ttu-id="043f7-225">Om du ser hello följande utdata från hello seriella övervakningsfönstret och hello blinka Indikator på Sparkfun ESP8266 sak Dev hello exempelprogrammet körts.</span><span class="sxs-lookup"><span data-stu-id="043f7-225">If you see hello following output from hello serial monitor window and hello blinking LED on Sparkfun ESP8266 Thing Dev, hello sample application is running successfully.</span></span>

![slutversionen i arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="043f7-227">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="043f7-227">Next steps</span></span>

<span data-ttu-id="043f7-228">Du har anslutit en Sparkfun ESP8266 sak Dev tooyour IoT-hubb och skickas hello avbildas sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="043f7-228">You have successfully connected a Sparkfun ESP8266 Thing Dev tooyour IoT hub and sent hello captured sensor data tooyour IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
