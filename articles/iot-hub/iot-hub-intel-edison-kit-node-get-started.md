---
title: "Intel modern till molnet (Node.js) – ansluta Intel modern till Azure IoT Hub | Microsoft Docs"
description: "Lär dig mer om att konfigurera och ansluta Intel modern till Azure IoT-hubb för Intel modern att skicka data till Azure-molnplattform i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure iot intel modern, intel modern iot-hubb, intel modern skicka data till molnet, intel modern till molnet
ms.assetid: a7c9cf2d-c102-41b0-aa45-41285c6877eb
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/15/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5a31efba704045196b5563f7bc467c773bea7805
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="connect-intel-edison-to-azure-iot-hub-nodejs"></a><span data-ttu-id="66e04-104">Ansluta Intel modern till Azure IoT-hubb (Node.js)</span><span class="sxs-lookup"><span data-stu-id="66e04-104">Connect Intel Edison to Azure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="66e04-105">I den här kursen kan du börja lära dig grunderna i att arbeta med Intel modern.</span><span class="sxs-lookup"><span data-stu-id="66e04-105">In this tutorial, you begin by learning the basics of working with Intel Edison.</span></span> <span data-ttu-id="66e04-106">Du lär dig sedan sömlöst ansluter enheterna till molnet med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="66e04-106">You then learn how to seamlessly connect your devices to the cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span>

<span data-ttu-id="66e04-107">Har du en kit ännu?</span><span class="sxs-lookup"><span data-stu-id="66e04-107">Don't have a kit yet?</span></span> <span data-ttu-id="66e04-108">Starta [här](https://azure.microsoft.com/develop/iot/starter-kits)</span><span class="sxs-lookup"><span data-stu-id="66e04-108">Start [here](https://azure.microsoft.com/develop/iot/starter-kits)</span></span>

## <a name="what-you-do"></a><span data-ttu-id="66e04-109">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="66e04-109">What you do</span></span>

* <span data-ttu-id="66e04-110">Konfigurera Intel modern och och Groove moduler.</span><span class="sxs-lookup"><span data-stu-id="66e04-110">Setup Intel Edison and and Grove modules.</span></span>
* <span data-ttu-id="66e04-111">Skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="66e04-111">Create an IoT hub.</span></span>
* <span data-ttu-id="66e04-112">Registrera en enhet för modern i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="66e04-112">Register a device for Edison in your IoT hub.</span></span>
* <span data-ttu-id="66e04-113">Kör ett exempelprogram på modern sensordata ska skickas till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="66e04-113">Run a sample application on Edison to send sensor data to your IoT hub.</span></span>

<span data-ttu-id="66e04-114">Intel modern att ansluta till en IoT-hubb som du skapar.</span><span class="sxs-lookup"><span data-stu-id="66e04-114">Connect Intel Edison to an IoT hub that you create.</span></span> <span data-ttu-id="66e04-115">Sedan kör du ett exempelprogram på modern samla in data för temperatur- och fuktighetskonsekvens från en Groove-temperatursensor.</span><span class="sxs-lookup"><span data-stu-id="66e04-115">Then you run a sample application on Edison to collect temperature and humidity data from a Grove temperature sensor.</span></span> <span data-ttu-id="66e04-116">Slutligen kan du skicka dessa sensordata för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="66e04-116">Finally, you send the sensor data to your IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="66e04-117">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="66e04-117">What you learn</span></span>

* <span data-ttu-id="66e04-118">Hur du skapar en Azure IoT-hubb och hämta din nya anslutningssträngen för enheten.</span><span class="sxs-lookup"><span data-stu-id="66e04-118">How to create an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="66e04-119">Så här ansluter du modern med en Groove-temperatursensor.</span><span class="sxs-lookup"><span data-stu-id="66e04-119">How to connect Edison with a Grove temperature sensor.</span></span>
* <span data-ttu-id="66e04-120">Hur du samlar in sensordata genom att köra ett exempelprogram på modern.</span><span class="sxs-lookup"><span data-stu-id="66e04-120">How to collect sensor data by running a sample application on Edison.</span></span>
* <span data-ttu-id="66e04-121">Hur du skickar sensordata till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="66e04-121">How to send sensor data to your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="66e04-122">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="66e04-122">What you need</span></span>

![Vad du behöver](media/iot-hub-intel-edison-kit-node-get-started/0_kit.png)

* <span data-ttu-id="66e04-124">Intel modern-kort</span><span class="sxs-lookup"><span data-stu-id="66e04-124">The Intel Edison board</span></span>
* <span data-ttu-id="66e04-125">Arduino expansion-kort</span><span class="sxs-lookup"><span data-stu-id="66e04-125">Arduino expansion board</span></span>
* <span data-ttu-id="66e04-126">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="66e04-126">An active Azure subscription.</span></span> <span data-ttu-id="66e04-127">Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="66e04-127">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="66e04-128">En Mac- eller en dator som kör Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="66e04-128">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="66e04-129">En Internetanslutning.</span><span class="sxs-lookup"><span data-stu-id="66e04-129">An Internet connection.</span></span>
* <span data-ttu-id="66e04-130">En Micro B till typ A USB-kabel</span><span class="sxs-lookup"><span data-stu-id="66e04-130">A Micro B to Type A USB cable</span></span>
* <span data-ttu-id="66e04-131">En strömförsörjning direct aktuella (DC).</span><span class="sxs-lookup"><span data-stu-id="66e04-131">A direct current (DC) power supply.</span></span> <span data-ttu-id="66e04-132">Din strömförsörjning bör klassificeras enligt följande:</span><span class="sxs-lookup"><span data-stu-id="66e04-132">Your power supply should be rated as follows:</span></span>
  - <span data-ttu-id="66e04-133">7 15V DC</span><span class="sxs-lookup"><span data-stu-id="66e04-133">7-15V DC</span></span>
  - <span data-ttu-id="66e04-134">Minst 1500mA</span><span class="sxs-lookup"><span data-stu-id="66e04-134">At least 1500mA</span></span>
  - <span data-ttu-id="66e04-135">Center/inre PIN-koden måste vara positiva Polen för strömförsörjningen</span><span class="sxs-lookup"><span data-stu-id="66e04-135">The center/inner pin should be the positive pole of the power supply</span></span>

<span data-ttu-id="66e04-136">Följande objekt är valfria:</span><span class="sxs-lookup"><span data-stu-id="66e04-136">The following items are optional:</span></span>

* <span data-ttu-id="66e04-137">Groove grundläggande Shield V2</span><span class="sxs-lookup"><span data-stu-id="66e04-137">Grove Base Shield V2</span></span>
* <span data-ttu-id="66e04-138">Groove - temperatursensor</span><span class="sxs-lookup"><span data-stu-id="66e04-138">Grove - Temperature Sensor</span></span>
* <span data-ttu-id="66e04-139">Groove-kabel</span><span class="sxs-lookup"><span data-stu-id="66e04-139">Grove Cable</span></span>
* <span data-ttu-id="66e04-140">En GIF-staplar eller skruvar som ingår i paketering, inklusive två skruvar för att fästa modulen som ska expansionskort och fyra uppsättningar av skruvar och form mellanrum.</span><span class="sxs-lookup"><span data-stu-id="66e04-140">Any spacer bars or screws included in the packaging, including two screws to fasten the module to the expansion board and four sets of screws and plastic spacers.</span></span>

> [!NOTE] 
<span data-ttu-id="66e04-141">Objekten är valfritt eftersom kod exempel support simulerade sensordata.</span><span class="sxs-lookup"><span data-stu-id="66e04-141">These items are optional because the code sample support simulated sensor data.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a><span data-ttu-id="66e04-142">Installationsprogrammet Intel modern</span><span class="sxs-lookup"><span data-stu-id="66e04-142">Setup Intel Edison</span></span>

### <a name="assemble-your-board"></a><span data-ttu-id="66e04-143">Assemblera kortets</span><span class="sxs-lookup"><span data-stu-id="66e04-143">Assemble your board</span></span>

<span data-ttu-id="66e04-144">Det här avsnittet innehåller steg om du vill bifoga Intel® modern-modulen expansionskort.</span><span class="sxs-lookup"><span data-stu-id="66e04-144">This section contains steps to attach your Intel® Edison module to your expansion board.</span></span>

1. <span data-ttu-id="66e04-145">Placera modulen Intel® modern inom vit kontur på expansionskort, Rada upp tomrum i modulen med skruvar på expansionskort.</span><span class="sxs-lookup"><span data-stu-id="66e04-145">Place the Intel® Edison module within the white outline on your expansion board, lining up the holes on the module with the screws on the expansion board.</span></span>

2. <span data-ttu-id="66e04-146">Tryck ned i modulen nedanför orden `What will you make?` tills du anser att en snapin.</span><span class="sxs-lookup"><span data-stu-id="66e04-146">Press down on the module just below the words `What will you make?` until you feel a snap.</span></span>

   ![Assemblera board 2](media/iot-hub-intel-edison-kit-node-get-started/1_assemble_board2.jpg)

3. <span data-ttu-id="66e04-148">Använd två hexadecimala nuts (ingår i paketet) för att skydda modulen expansion planen.</span><span class="sxs-lookup"><span data-stu-id="66e04-148">Use the two hex nuts (included in the package) to secure the module to the expansion board.</span></span>

   ![Assemblera board 3](media/iot-hub-intel-edison-kit-node-get-started/2_assemble_board3.jpg)

4. <span data-ttu-id="66e04-150">Infoga en skruv i någon av fyra hörnet hål på expansionskort.</span><span class="sxs-lookup"><span data-stu-id="66e04-150">Insert a screw in one of the four corner holes on the expansion board.</span></span> <span data-ttu-id="66e04-151">Genom att vrida och öka en vit form mellanrum till skruven.</span><span class="sxs-lookup"><span data-stu-id="66e04-151">Twist and tighten one of the white plastic spacers onto the screw.</span></span>

   ![Assemblera board 4](media/iot-hub-intel-edison-kit-node-get-started/3_assemble_board4.jpg)

5. <span data-ttu-id="66e04-153">Upprepa för de andra tre hörnet mellanrum.</span><span class="sxs-lookup"><span data-stu-id="66e04-153">Repeat for the other three corner spacers.</span></span>

   ![Assemblera board 5](media/iot-hub-intel-edison-kit-node-get-started/4_assemble_board5.jpg)

<span data-ttu-id="66e04-155">Nu är kortets monterad.</span><span class="sxs-lookup"><span data-stu-id="66e04-155">Now your board is assembled.</span></span>

   ![monterade board](media/iot-hub-intel-edison-kit-node-get-started/5_assembled_board.jpg)

### <a name="connect-the-grove-base-shield-and-the-temperature-sensor"></a><span data-ttu-id="66e04-157">Ansluta Groove Base Shield och -temperatursensor</span><span class="sxs-lookup"><span data-stu-id="66e04-157">Connect the Grove Base Shield and the temperature sensor</span></span>

1. <span data-ttu-id="66e04-158">Placera Groove Base Shield in dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="66e04-158">Place the Grove Base Shield on to your board.</span></span> <span data-ttu-id="66e04-159">Kontrollera att alla stift nära är anslutna till din kort.</span><span class="sxs-lookup"><span data-stu-id="66e04-159">Make sure all pins are tightly plugged into your board.</span></span>
   
   ![Groove grundläggande Shield](media/iot-hub-intel-edison-kit-node-get-started/6_grove_base_sheild.jpg)

2. <span data-ttu-id="66e04-161">Använd Groove kabel för att ansluta Groove-temperatursensor till Groove Base Shield **A0** port.</span><span class="sxs-lookup"><span data-stu-id="66e04-161">Use Grove Cable to connect Grove temperature sensor onto the Grove Base Shield **A0** port.</span></span>

   ![Anslut till-temperatursensor](media/iot-hub-intel-edison-kit-node-get-started/7_temperature_sensor.jpg)

   ![Modern och sensor-anslutning](media/iot-hub-intel-edison-kit-node-get-started/16_edion_sensor.png)

<span data-ttu-id="66e04-164">Din sensor är nu klar.</span><span class="sxs-lookup"><span data-stu-id="66e04-164">Now your sensor is ready.</span></span>

### <a name="power-up-edison"></a><span data-ttu-id="66e04-165">Använd modern</span><span class="sxs-lookup"><span data-stu-id="66e04-165">Power up Edison</span></span>

1. <span data-ttu-id="66e04-166">Anslut strömförsörjningen.</span><span class="sxs-lookup"><span data-stu-id="66e04-166">Plug in the power supply.</span></span>

   ![Plugin-strömförsörjning](media/iot-hub-intel-edison-kit-node-get-started/8_plug_power.jpg)

2. <span data-ttu-id="66e04-168">En grön Indikator (heter DS1 på expansion-kort Arduino *) ska lysa upp och hålla upplysta.</span><span class="sxs-lookup"><span data-stu-id="66e04-168">A green LED(labeled DS1 on the Arduino* expansion board) should light up and stay lit.</span></span>

3. <span data-ttu-id="66e04-169">Vänta en minut för board att avsluta startar.</span><span class="sxs-lookup"><span data-stu-id="66e04-169">Wait one minute for the board to finish booting up.</span></span>

   > [!NOTE]
   > <span data-ttu-id="66e04-170">Om du inte har en DC-strömförsörjning kan du fortfarande power board via en USB-port.</span><span class="sxs-lookup"><span data-stu-id="66e04-170">If you do not have a DC power supply, you can still power the board through a USB port.</span></span> <span data-ttu-id="66e04-171">Se `Connect Edison to your computer` information.</span><span class="sxs-lookup"><span data-stu-id="66e04-171">See `Connect Edison to your computer` section for details.</span></span> <span data-ttu-id="66e04-172">Startar din board på detta sätt kan resultera i oväntade funktionssätt från dina ändringar, särskilt när med hjälp av Wi-Fi eller drivande motorer.</span><span class="sxs-lookup"><span data-stu-id="66e04-172">Powering your board in this fashion may result in unpredictable behavior from your board, especially when using Wi-Fi or driving motors.</span></span>

### <a name="connect-edison-to-your-computer"></a><span data-ttu-id="66e04-173">Ansluta modern till din dator</span><span class="sxs-lookup"><span data-stu-id="66e04-173">Connect Edison to your computer</span></span>

1. <span data-ttu-id="66e04-174">Växla ned microswitch mot de två micro USB-portarna så att modern enhet-läge.</span><span class="sxs-lookup"><span data-stu-id="66e04-174">Toggle down the microswitch towards the two micro USB ports, so that Edison is in device mode.</span></span> <span data-ttu-id="66e04-175">Skillnader mellan enheten och värden läge referera [här](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span><span class="sxs-lookup"><span data-stu-id="66e04-175">For differences between device mode and host mode, please reference [here](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span></span>

   ![Växla ned microswitch](media/iot-hub-intel-edison-kit-node-get-started/9_toggle_down_microswitch.jpg)

2. <span data-ttu-id="66e04-177">Anslut micro USB-kabel till översta micro USB-port.</span><span class="sxs-lookup"><span data-stu-id="66e04-177">Plug the micro USB cable into the top micro USB port.</span></span>

   ![Övre micro USB-port](media/iot-hub-intel-edison-kit-node-get-started/10_top_usbport.jpg)

3. <span data-ttu-id="66e04-179">Anslut den andra änden av USB-kabel till din dator.</span><span class="sxs-lookup"><span data-stu-id="66e04-179">Plug the other end of USB cable into your computer.</span></span>

   ![Datorn USB](media/iot-hub-intel-edison-kit-node-get-started/11_computer_usb.jpg)

4. <span data-ttu-id="66e04-181">Vet du kortets initieras fullständigt när datorn monterar en ny enhet (ungefär som att lägga till ett SD-kort i datorn).</span><span class="sxs-lookup"><span data-stu-id="66e04-181">You will know that your board is fully initialized when your computer mounts a new drive (much like inserting a SD card into your computer).</span></span>

## <a name="download-and-run-the-configuration-tool"></a><span data-ttu-id="66e04-182">Hämta och kör verktyget configuration</span><span class="sxs-lookup"><span data-stu-id="66e04-182">Download and run the configuration tool</span></span>
<span data-ttu-id="66e04-183">Hämta den senaste konfiguration för från [länken](https://software.intel.com/en-us/iot/hardware/edison/downloads) visas under den `Installers` rubrik.</span><span class="sxs-lookup"><span data-stu-id="66e04-183">Get the latest configuration tool from [this link](https://software.intel.com/en-us/iot/hardware/edison/downloads) listed under the `Installers` heading.</span></span> <span data-ttu-id="66e04-184">Kör verktyget och följ dess på skärmen instruktioner, klicka på Nästa om det behövs</span><span class="sxs-lookup"><span data-stu-id="66e04-184">Execute the tool and follow its on-screen instructions, clicking Next where needed</span></span>

### <a name="flash-firmware"></a><span data-ttu-id="66e04-185">Flash inbyggd programvara</span><span class="sxs-lookup"><span data-stu-id="66e04-185">Flash firmware</span></span>
1. <span data-ttu-id="66e04-186">På den `Set up options` klickar du på `Flash Firmware`.</span><span class="sxs-lookup"><span data-stu-id="66e04-186">On the `Set up options` page, click `Flash Firmware`.</span></span>
2. <span data-ttu-id="66e04-187">Välj avbildningen som flash till kortets genom att göra något av följande:</span><span class="sxs-lookup"><span data-stu-id="66e04-187">Select the image to flash onto your board by doing one of the following:</span></span>
   - <span data-ttu-id="66e04-188">Du kan hämta och flash din board med den senaste firmware-avbildningen från Intel, Välj `Download the latest image version xxxx`.</span><span class="sxs-lookup"><span data-stu-id="66e04-188">To download and flash your board with the latest firmware image available from Intel, select `Download the latest image version xxxx`.</span></span>
   - <span data-ttu-id="66e04-189">För att flash dina ändringar med en avbildning som redan har sparats på datorn, Välj `Select the local image`.</span><span class="sxs-lookup"><span data-stu-id="66e04-189">To flash your board with an image you already have saved on your computer, select `Select the local image`.</span></span> <span data-ttu-id="66e04-190">Bläddra till och välj den avbildning du vill flash din planen.</span><span class="sxs-lookup"><span data-stu-id="66e04-190">Browse to and select the image you want to flash to your board.</span></span>
3. <span data-ttu-id="66e04-191">Verktyget installationsprogrammet försöker flash kortets.</span><span class="sxs-lookup"><span data-stu-id="66e04-191">The setup tool will attempt to flash your board.</span></span> <span data-ttu-id="66e04-192">Hela blinkande processen kan ta upp till 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="66e04-192">The entire flashing process may take up to 10 minutes.</span></span>

### <a name="set-password"></a><span data-ttu-id="66e04-193">Ange ett lösenord</span><span class="sxs-lookup"><span data-stu-id="66e04-193">Set password</span></span>
1. <span data-ttu-id="66e04-194">På den `Set up options` klickar du på `Enable Security`.</span><span class="sxs-lookup"><span data-stu-id="66e04-194">On the `Set up options` page, click `Enable Security`.</span></span>
2. <span data-ttu-id="66e04-195">Du kan ange ett eget namn för Intel® modern-kort.</span><span class="sxs-lookup"><span data-stu-id="66e04-195">You can set a custom name for your Intel® Edison board.</span></span> <span data-ttu-id="66e04-196">Det här är valfritt.</span><span class="sxs-lookup"><span data-stu-id="66e04-196">This is optional.</span></span>
3. <span data-ttu-id="66e04-197">Ange ett lösenord för dina ändringar och klicka sedan på `Set password`.</span><span class="sxs-lookup"><span data-stu-id="66e04-197">Type a password for your board, then click `Set password`.</span></span>
4. <span data-ttu-id="66e04-198">Anteckna lösenordet som används senare.</span><span class="sxs-lookup"><span data-stu-id="66e04-198">Mark down the password, which is used later.</span></span>

### <a name="connect-wi-fi"></a><span data-ttu-id="66e04-199">Ansluta Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="66e04-199">Connect Wi-Fi</span></span>
1. <span data-ttu-id="66e04-200">På den `Set up options` klickar du på `Connect Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="66e04-200">On the `Set up options` page, click `Connect Wi-Fi`.</span></span> <span data-ttu-id="66e04-201">Vänta i upp till en minut som datorn söker efter tillgängliga Wi-Fi-nätverk.</span><span class="sxs-lookup"><span data-stu-id="66e04-201">Wait up to one minute as your computer scans for available Wi-Fi networks.</span></span>
2. <span data-ttu-id="66e04-202">Från den `Detected Networks` listrutan väljer du nätverket.</span><span class="sxs-lookup"><span data-stu-id="66e04-202">From the `Detected Networks` drop-down list, select your network.</span></span>
3. <span data-ttu-id="66e04-203">Från den `Security` listrutan väljer du nätverkstyp säkerhet.</span><span class="sxs-lookup"><span data-stu-id="66e04-203">From the `Security` drop-down list, select the network's security type.</span></span>
4. <span data-ttu-id="66e04-204">Ange din information inloggningsnamn och lösenord och klicka sedan på `Configure Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="66e04-204">Provide your login and password information, then click `Configure Wi-Fi`.</span></span>
5. <span data-ttu-id="66e04-205">Anteckna den IP-adressen som används senare.</span><span class="sxs-lookup"><span data-stu-id="66e04-205">Mark down the IP address, which is used later.</span></span>

> [!NOTE]
> <span data-ttu-id="66e04-206">Kontrollera att modern är ansluten till samma nätverk som datorn.</span><span class="sxs-lookup"><span data-stu-id="66e04-206">Make sure that Edison is connected to the same network as your computer.</span></span> <span data-ttu-id="66e04-207">Datorn ansluter till din modern med hjälp av IP-adress.</span><span class="sxs-lookup"><span data-stu-id="66e04-207">Your computer connects to your Edison by using the IP address.</span></span>

   ![Anslut till-temperatursensor](media/iot-hub-intel-edison-kit-node-get-started/12_configuration_tool.png)

<span data-ttu-id="66e04-209">Grattis!</span><span class="sxs-lookup"><span data-stu-id="66e04-209">Congratulations!</span></span> <span data-ttu-id="66e04-210">Du har konfigurerat modern.</span><span class="sxs-lookup"><span data-stu-id="66e04-210">You've successfully configured Edison.</span></span>

## <a name="run-a-sample-application-on-intel-edison"></a><span data-ttu-id="66e04-211">Kör ett exempelprogram på Intel modern</span><span class="sxs-lookup"><span data-stu-id="66e04-211">Run a sample application on Intel Edison</span></span>

### <a name="prepare-the-azure-iot-device-sdk"></a><span data-ttu-id="66e04-212">Förbered enheten för Azure IoT SDK</span><span class="sxs-lookup"><span data-stu-id="66e04-212">Prepare the Azure IoT Device SDK</span></span>

1. <span data-ttu-id="66e04-213">Använd en av följande SSH-klienter från värddatorn för att ansluta till Intel-modern.</span><span class="sxs-lookup"><span data-stu-id="66e04-213">Use one of the following SSH clients from your host computer to connect to your Intel Edison.</span></span> <span data-ttu-id="66e04-214">IP-adressen är från konfigurationsverktyget och lösenordet är det du har angett i verktyget.</span><span class="sxs-lookup"><span data-stu-id="66e04-214">The IP address is from the configuration tool and the password is the one you've set in that tool.</span></span>
    - <span data-ttu-id="66e04-215">[PuTTY](http://www.putty.org/) för Windows.</span><span class="sxs-lookup"><span data-stu-id="66e04-215">[PuTTY](http://www.putty.org/) for Windows.</span></span>
    - <span data-ttu-id="66e04-216">Den inbyggda SSH-klienten på Ubuntu eller macOS.</span><span class="sxs-lookup"><span data-stu-id="66e04-216">The built-in SSH client on Ubuntu or macOS.</span></span>

2. <span data-ttu-id="66e04-217">Klona exempelappen klient till din enhet.</span><span class="sxs-lookup"><span data-stu-id="66e04-217">Clone the sample client app to your device.</span></span> 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-intel-edison-client-app
   ```

3. <span data-ttu-id="66e04-218">Gå till mappen lagringsplatsen för att köra följande kommando för att installera alla paket kan det ta serval minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="66e04-218">Then navigate to the repo folder to run the following command to install all packages, it may take serval minutes to complete.</span></span>
   
   ```bash
   cd iot-hub-node-intel-edison-client-app
   npm install
   ```


### <a name="configure-and-run-the-sample-application"></a><span data-ttu-id="66e04-219">Konfigurera och köra exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="66e04-219">Configure and run the sample application</span></span>

1. <span data-ttu-id="66e04-220">Öppna konfigurationsfilen genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="66e04-220">Open the config file by running the following commands:</span></span>

   ```bash
   nano config.json
   ```

   ![Config-fil](media/iot-hub-intel-edison-kit-node-get-started/13_configure_file.png)

   <span data-ttu-id="66e04-222">Det finns två makron i den här filen kan du configurate.</span><span class="sxs-lookup"><span data-stu-id="66e04-222">There are two macros in this file you can configurate.</span></span> <span data-ttu-id="66e04-223">Den första är `INTERVAL`, som definierar tidsintervallet mellan två meddelanden som skickas till molnet.</span><span class="sxs-lookup"><span data-stu-id="66e04-223">The first one is `INTERVAL`, which defines the time interval between two messages that send to cloud.</span></span> <span data-ttu-id="66e04-224">Det andra `simulatedData`, vilket är ett booleskt värde för om du vill använda simulerade sensordata eller inte.</span><span class="sxs-lookup"><span data-stu-id="66e04-224">The second one `simulatedData`,which is a Boolean value for whether to use simulated sensor data or not.</span></span>

   <span data-ttu-id="66e04-225">Om du **saknar sensorn**, ange den `simulatedData` värde till `true` för att skapa och använda simulerade sensordata exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="66e04-225">If you **don't have the sensor**, set the `simulatedData` value to `true` to make the sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="66e04-226">Spara och avsluta genom att trycka på CTRL-O > Ange > kontrollen X.</span><span class="sxs-lookup"><span data-stu-id="66e04-226">Save and exit by pressing Control-O > Enter > Control-X.</span></span>


1. <span data-ttu-id="66e04-227">Kör exempelprogrammet genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="66e04-227">Run the sample application by running the following command:</span></span>

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   <span data-ttu-id="66e04-228">Kontrollera att du kopiera / klistra in anslutningssträngen enheten till de enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="66e04-228">Make sure you copy-paste the device connection string into the single quotes.</span></span>

<span data-ttu-id="66e04-229">Du bör se följande utdata som visar dessa sensordata och meddelanden som skickas till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="66e04-229">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub.</span></span>

![Utdata - sensordata som skickats från Intel modern till din IoT-hubb](media/iot-hub-intel-edison-kit-node-get-started/15_message_sent.png)

## <a name="next-steps"></a><span data-ttu-id="66e04-231">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66e04-231">Next steps</span></span>

<span data-ttu-id="66e04-232">Du har kört ett exempelprogram för att samla in sensordata och skicka den till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="66e04-232">You’ve run a sample application to collect sensor data and send it to your IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
