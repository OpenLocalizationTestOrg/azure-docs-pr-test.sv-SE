---
title: "aaaIntel modern toocloud (Node.js) – ansluta Intel modern tooAzure IoT-hubb | Microsoft Docs"
description: "Lär dig hur toosetup och ansluta Intel modern tooAzure IoT-hubb för Intel modern toosend data toohello Azure-molnplattform i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure iot intel modern, intel modern iot-hubb, intel modern skicka data toocloud, intel modern toocloud
ms.assetid: a7c9cf2d-c102-41b0-aa45-41285c6877eb
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/15/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfc3387efc532b4b83f0626a9cf61d12c2952af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-intel-edison-tooazure-iot-hub-nodejs"></a><span data-ttu-id="4367f-104">Ansluta Intel modern tooAzure IoT-hubb (Node.js)</span><span class="sxs-lookup"><span data-stu-id="4367f-104">Connect Intel Edison tooAzure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="4367f-105">I den här kursen kan du börja genom att lära dig hello grunderna i att arbeta med Intel modern.</span><span class="sxs-lookup"><span data-stu-id="4367f-105">In this tutorial, you begin by learning hello basics of working with Intel Edison.</span></span> <span data-ttu-id="4367f-106">Du lär dig hur tooseamlessly ansluta enheter toohello molnet med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="4367f-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span>

<span data-ttu-id="4367f-107">Har du en kit ännu?</span><span class="sxs-lookup"><span data-stu-id="4367f-107">Don't have a kit yet?</span></span> <span data-ttu-id="4367f-108">Starta [här](https://azure.microsoft.com/develop/iot/starter-kits)</span><span class="sxs-lookup"><span data-stu-id="4367f-108">Start [here](https://azure.microsoft.com/develop/iot/starter-kits)</span></span>

## <a name="what-you-do"></a><span data-ttu-id="4367f-109">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="4367f-109">What you do</span></span>

* <span data-ttu-id="4367f-110">Konfigurera Intel modern och och Groove moduler.</span><span class="sxs-lookup"><span data-stu-id="4367f-110">Setup Intel Edison and and Grove modules.</span></span>
* <span data-ttu-id="4367f-111">Skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4367f-111">Create an IoT hub.</span></span>
* <span data-ttu-id="4367f-112">Registrera en enhet för modern i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4367f-112">Register a device for Edison in your IoT hub.</span></span>
* <span data-ttu-id="4367f-113">Kör ett exempelprogram på modern toosend sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4367f-113">Run a sample application on Edison toosend sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="4367f-114">Ansluta Intel modern tooan IoT-hubb som du skapar.</span><span class="sxs-lookup"><span data-stu-id="4367f-114">Connect Intel Edison tooan IoT hub that you create.</span></span> <span data-ttu-id="4367f-115">Sedan kör du ett exempelprogram på modern toocollect temperatur- och fuktighetskonsekvens data från en Groove-temperatursensor.</span><span class="sxs-lookup"><span data-stu-id="4367f-115">Then you run a sample application on Edison toocollect temperature and humidity data from a Grove temperature sensor.</span></span> <span data-ttu-id="4367f-116">Slutligen kan skicka du hello sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4367f-116">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="4367f-117">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="4367f-117">What you learn</span></span>

* <span data-ttu-id="4367f-118">Hur toocreate en Azure IoT-hubb och få nya enheten anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="4367f-118">How toocreate an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="4367f-119">Hur tooconnect modern med en Groove-temperatursensor.</span><span class="sxs-lookup"><span data-stu-id="4367f-119">How tooconnect Edison with a Grove temperature sensor.</span></span>
* <span data-ttu-id="4367f-120">Hur toocollect sensordata genom att köra ett exempelprogram på modern.</span><span class="sxs-lookup"><span data-stu-id="4367f-120">How toocollect sensor data by running a sample application on Edison.</span></span>
* <span data-ttu-id="4367f-121">Hur toosend sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4367f-121">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="4367f-122">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="4367f-122">What you need</span></span>

![Vad du behöver](media/iot-hub-intel-edison-kit-node-get-started/0_kit.png)

* <span data-ttu-id="4367f-124">hello Intel modern-kort</span><span class="sxs-lookup"><span data-stu-id="4367f-124">hello Intel Edison board</span></span>
* <span data-ttu-id="4367f-125">Arduino expansion-kort</span><span class="sxs-lookup"><span data-stu-id="4367f-125">Arduino expansion board</span></span>
* <span data-ttu-id="4367f-126">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4367f-126">An active Azure subscription.</span></span> <span data-ttu-id="4367f-127">Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="4367f-127">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="4367f-128">En Mac- eller en dator som kör Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="4367f-128">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="4367f-129">En Internetanslutning.</span><span class="sxs-lookup"><span data-stu-id="4367f-129">An Internet connection.</span></span>
* <span data-ttu-id="4367f-130">Micro B-tooType en USB-kabel</span><span class="sxs-lookup"><span data-stu-id="4367f-130">A Micro B tooType A USB cable</span></span>
* <span data-ttu-id="4367f-131">En strömförsörjning direct aktuella (DC).</span><span class="sxs-lookup"><span data-stu-id="4367f-131">A direct current (DC) power supply.</span></span> <span data-ttu-id="4367f-132">Din strömförsörjning bör klassificeras enligt följande:</span><span class="sxs-lookup"><span data-stu-id="4367f-132">Your power supply should be rated as follows:</span></span>
  - <span data-ttu-id="4367f-133">7 15V DC</span><span class="sxs-lookup"><span data-stu-id="4367f-133">7-15V DC</span></span>
  - <span data-ttu-id="4367f-134">Minst 1500mA</span><span class="sxs-lookup"><span data-stu-id="4367f-134">At least 1500mA</span></span>
  - <span data-ttu-id="4367f-135">hello center/inre PIN-kod måste vara positiva hello Polen av hello-strömförsörjning</span><span class="sxs-lookup"><span data-stu-id="4367f-135">hello center/inner pin should be hello positive pole of hello power supply</span></span>

<span data-ttu-id="4367f-136">hello följande objekt är valfria:</span><span class="sxs-lookup"><span data-stu-id="4367f-136">hello following items are optional:</span></span>

* <span data-ttu-id="4367f-137">Groove grundläggande Shield V2</span><span class="sxs-lookup"><span data-stu-id="4367f-137">Grove Base Shield V2</span></span>
* <span data-ttu-id="4367f-138">Groove - temperatursensor</span><span class="sxs-lookup"><span data-stu-id="4367f-138">Grove - Temperature Sensor</span></span>
* <span data-ttu-id="4367f-139">Groove-kabel</span><span class="sxs-lookup"><span data-stu-id="4367f-139">Grove Cable</span></span>
* <span data-ttu-id="4367f-140">En GIF-staplar eller skruvar som ingår i hello paketering, inklusive två skruvar toofasten hello modulen toohello expansionskort och fyra uppsättningar av skruvar och form mellanrum.</span><span class="sxs-lookup"><span data-stu-id="4367f-140">Any spacer bars or screws included in hello packaging, including two screws toofasten hello module toohello expansion board and four sets of screws and plastic spacers.</span></span>

> [!NOTE] 
<span data-ttu-id="4367f-141">Objekten är valfritt eftersom hello kod exempel support simulerade sensordata.</span><span class="sxs-lookup"><span data-stu-id="4367f-141">These items are optional because hello code sample support simulated sensor data.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a><span data-ttu-id="4367f-142">Installationsprogrammet Intel modern</span><span class="sxs-lookup"><span data-stu-id="4367f-142">Setup Intel Edison</span></span>

### <a name="assemble-your-board"></a><span data-ttu-id="4367f-143">Assemblera kortets</span><span class="sxs-lookup"><span data-stu-id="4367f-143">Assemble your board</span></span>

<span data-ttu-id="4367f-144">Det här avsnittet innehåller steg tooattach din Intel® modern modulen tooyour expansionskort.</span><span class="sxs-lookup"><span data-stu-id="4367f-144">This section contains steps tooattach your Intel® Edison module tooyour expansion board.</span></span>

1. <span data-ttu-id="4367f-145">Placera hello Intel® modern modul i hello vit disposition på expansionskort, Rada upp hello hål på hello modulen med hello skruvar på hello expansion-kort.</span><span class="sxs-lookup"><span data-stu-id="4367f-145">Place hello Intel® Edison module within hello white outline on your expansion board, lining up hello holes on hello module with hello screws on hello expansion board.</span></span>

2. <span data-ttu-id="4367f-146">Tryck ned på hello modulen nedanför hello ord `What will you make?` tills du anser att en snapin.</span><span class="sxs-lookup"><span data-stu-id="4367f-146">Press down on hello module just below hello words `What will you make?` until you feel a snap.</span></span>

   ![Assemblera board 2](media/iot-hub-intel-edison-kit-node-get-started/1_assemble_board2.jpg)

3. <span data-ttu-id="4367f-148">Använd hello två hexadecimala nuts (ingår i hello-paket) toosecure hello modulen toohello expansionskort.</span><span class="sxs-lookup"><span data-stu-id="4367f-148">Use hello two hex nuts (included in hello package) toosecure hello module toohello expansion board.</span></span>

   ![Assemblera board 3](media/iot-hub-intel-edison-kit-node-get-started/2_assemble_board3.jpg)

4. <span data-ttu-id="4367f-150">Infoga en skruv i de hello fyra hörnet hål på hello expansion-kort.</span><span class="sxs-lookup"><span data-stu-id="4367f-150">Insert a screw in one of hello four corner holes on hello expansion board.</span></span> <span data-ttu-id="4367f-151">Genom att vrida och öka en vit hello form mellanrum till hello skruv.</span><span class="sxs-lookup"><span data-stu-id="4367f-151">Twist and tighten one of hello white plastic spacers onto hello screw.</span></span>

   ![Assemblera board 4](media/iot-hub-intel-edison-kit-node-get-started/3_assemble_board4.jpg)

5. <span data-ttu-id="4367f-153">Upprepa för hello andra tre hörnet mellanrum.</span><span class="sxs-lookup"><span data-stu-id="4367f-153">Repeat for hello other three corner spacers.</span></span>

   ![Assemblera board 5](media/iot-hub-intel-edison-kit-node-get-started/4_assemble_board5.jpg)

<span data-ttu-id="4367f-155">Nu är kortets monterad.</span><span class="sxs-lookup"><span data-stu-id="4367f-155">Now your board is assembled.</span></span>

   ![monterade board](media/iot-hub-intel-edison-kit-node-get-started/5_assembled_board.jpg)

### <a name="connect-hello-grove-base-shield-and-hello-temperature-sensor"></a><span data-ttu-id="4367f-157">Ansluta hello Groove Base Shield och hello-temperatursensor</span><span class="sxs-lookup"><span data-stu-id="4367f-157">Connect hello Grove Base Shield and hello temperature sensor</span></span>

1. <span data-ttu-id="4367f-158">Placera hello Groove Base Shield på tooyour-kort.</span><span class="sxs-lookup"><span data-stu-id="4367f-158">Place hello Grove Base Shield on tooyour board.</span></span> <span data-ttu-id="4367f-159">Kontrollera att alla stift nära är anslutna till din kort.</span><span class="sxs-lookup"><span data-stu-id="4367f-159">Make sure all pins are tightly plugged into your board.</span></span>
   
   ![Groove grundläggande Shield](media/iot-hub-intel-edison-kit-node-get-started/6_grove_base_sheild.jpg)

2. <span data-ttu-id="4367f-161">Använd Groove kabel tooconnect Groove-temperatursensor till hello Groove Base Shield **A0** port.</span><span class="sxs-lookup"><span data-stu-id="4367f-161">Use Grove Cable tooconnect Grove temperature sensor onto hello Grove Base Shield **A0** port.</span></span>

   ![Ansluta tootemperature sensor](media/iot-hub-intel-edison-kit-node-get-started/7_temperature_sensor.jpg)

   ![Modern och sensor-anslutning](media/iot-hub-intel-edison-kit-node-get-started/16_edion_sensor.png)

<span data-ttu-id="4367f-164">Din sensor är nu klar.</span><span class="sxs-lookup"><span data-stu-id="4367f-164">Now your sensor is ready.</span></span>

### <a name="power-up-edison"></a><span data-ttu-id="4367f-165">Använd modern</span><span class="sxs-lookup"><span data-stu-id="4367f-165">Power up Edison</span></span>

1. <span data-ttu-id="4367f-166">Anslut hello strömförsörjning.</span><span class="sxs-lookup"><span data-stu-id="4367f-166">Plug in hello power supply.</span></span>

   ![Plugin-strömförsörjning](media/iot-hub-intel-edison-kit-node-get-started/8_plug_power.jpg)

2. <span data-ttu-id="4367f-168">En grön Indikator (heter DS1 på hello Arduino * expansion board) ska lysa upp och hålla upplysta.</span><span class="sxs-lookup"><span data-stu-id="4367f-168">A green LED(labeled DS1 on hello Arduino* expansion board) should light up and stay lit.</span></span>

3. <span data-ttu-id="4367f-169">Vänta en minut för hello board toofinish startas.</span><span class="sxs-lookup"><span data-stu-id="4367f-169">Wait one minute for hello board toofinish booting up.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4367f-170">Om du inte har en DC-strömförsörjning kan du fortfarande power hello board via en USB-port.</span><span class="sxs-lookup"><span data-stu-id="4367f-170">If you do not have a DC power supply, you can still power hello board through a USB port.</span></span> <span data-ttu-id="4367f-171">Se `Connect Edison tooyour computer` information.</span><span class="sxs-lookup"><span data-stu-id="4367f-171">See `Connect Edison tooyour computer` section for details.</span></span> <span data-ttu-id="4367f-172">Startar din board på detta sätt kan resultera i oväntade funktionssätt från dina ändringar, särskilt när med hjälp av Wi-Fi eller drivande motorer.</span><span class="sxs-lookup"><span data-stu-id="4367f-172">Powering your board in this fashion may result in unpredictable behavior from your board, especially when using Wi-Fi or driving motors.</span></span>

### <a name="connect-edison-tooyour-computer"></a><span data-ttu-id="4367f-173">Ansluta modern tooyour dator</span><span class="sxs-lookup"><span data-stu-id="4367f-173">Connect Edison tooyour computer</span></span>

1. <span data-ttu-id="4367f-174">Växla ned hello microswitch mot hello två micro USB-portar, så att modern enhet-läge.</span><span class="sxs-lookup"><span data-stu-id="4367f-174">Toggle down hello microswitch towards hello two micro USB ports, so that Edison is in device mode.</span></span> <span data-ttu-id="4367f-175">Skillnader mellan enheten och värden läge referera [här](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span><span class="sxs-lookup"><span data-stu-id="4367f-175">For differences between device mode and host mode, please reference [here](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span></span>

   ![Växla ned hello microswitch](media/iot-hub-intel-edison-kit-node-get-started/9_toggle_down_microswitch.jpg)

2. <span data-ttu-id="4367f-177">Anslut hello micro USB-kabel till hello översta micro USB-port.</span><span class="sxs-lookup"><span data-stu-id="4367f-177">Plug hello micro USB cable into hello top micro USB port.</span></span>

   ![Övre micro USB-port](media/iot-hub-intel-edison-kit-node-get-started/10_top_usbport.jpg)

3. <span data-ttu-id="4367f-179">Plug hello andra änden av USB-kabel till datorn.</span><span class="sxs-lookup"><span data-stu-id="4367f-179">Plug hello other end of USB cable into your computer.</span></span>

   ![Datorn USB](media/iot-hub-intel-edison-kit-node-get-started/11_computer_usb.jpg)

4. <span data-ttu-id="4367f-181">Vet du kortets initieras fullständigt när datorn monterar en ny enhet (ungefär som att lägga till ett SD-kort i datorn).</span><span class="sxs-lookup"><span data-stu-id="4367f-181">You will know that your board is fully initialized when your computer mounts a new drive (much like inserting a SD card into your computer).</span></span>

## <a name="download-and-run-hello-configuration-tool"></a><span data-ttu-id="4367f-182">Hämta och kör konfigurationsverktyget för hello</span><span class="sxs-lookup"><span data-stu-id="4367f-182">Download and run hello configuration tool</span></span>
<span data-ttu-id="4367f-183">Hämta hello senaste konfigurationsverktyget från [länken](https://software.intel.com/en-us/iot/hardware/edison/downloads) visas under hello `Installers` rubrik.</span><span class="sxs-lookup"><span data-stu-id="4367f-183">Get hello latest configuration tool from [this link](https://software.intel.com/en-us/iot/hardware/edison/downloads) listed under hello `Installers` heading.</span></span> <span data-ttu-id="4367f-184">Kör verktyget hello och följ dess på skärmen instruktioner, klicka på Nästa om det behövs</span><span class="sxs-lookup"><span data-stu-id="4367f-184">Execute hello tool and follow its on-screen instructions, clicking Next where needed</span></span>

### <a name="flash-firmware"></a><span data-ttu-id="4367f-185">Flash inbyggd programvara</span><span class="sxs-lookup"><span data-stu-id="4367f-185">Flash firmware</span></span>
1. <span data-ttu-id="4367f-186">På hello `Set up options` klickar du på `Flash Firmware`.</span><span class="sxs-lookup"><span data-stu-id="4367f-186">On hello `Set up options` page, click `Flash Firmware`.</span></span>
2. <span data-ttu-id="4367f-187">Välj hello avbildningen tooflash till dina ändringar genom att göra något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="4367f-187">Select hello image tooflash onto your board by doing one of hello following:</span></span>
   - <span data-ttu-id="4367f-188">toodownload och flash din board med hello senaste firmware-avbildning från Intel, Välj `Download hello latest image version xxxx`.</span><span class="sxs-lookup"><span data-stu-id="4367f-188">toodownload and flash your board with hello latest firmware image available from Intel, select `Download hello latest image version xxxx`.</span></span>
   - <span data-ttu-id="4367f-189">tooflash dina ändringar med en avbildning som redan har sparats på din dator, Välj `Select hello local image`.</span><span class="sxs-lookup"><span data-stu-id="4367f-189">tooflash your board with an image you already have saved on your computer, select `Select hello local image`.</span></span> <span data-ttu-id="4367f-190">Bläddra tooand väljer hello bild tooflash tooyour-kort.</span><span class="sxs-lookup"><span data-stu-id="4367f-190">Browse tooand select hello image you want tooflash tooyour board.</span></span>
3. <span data-ttu-id="4367f-191">hello installationsverktyget försöker tooflash kortets.</span><span class="sxs-lookup"><span data-stu-id="4367f-191">hello setup tool will attempt tooflash your board.</span></span> <span data-ttu-id="4367f-192">hello hela blinkande processen kan ta too10 minuter.</span><span class="sxs-lookup"><span data-stu-id="4367f-192">hello entire flashing process may take up too10 minutes.</span></span>

### <a name="set-password"></a><span data-ttu-id="4367f-193">Ange ett lösenord</span><span class="sxs-lookup"><span data-stu-id="4367f-193">Set password</span></span>
1. <span data-ttu-id="4367f-194">På hello `Set up options` klickar du på `Enable Security`.</span><span class="sxs-lookup"><span data-stu-id="4367f-194">On hello `Set up options` page, click `Enable Security`.</span></span>
2. <span data-ttu-id="4367f-195">Du kan ange ett eget namn för Intel® modern-kort.</span><span class="sxs-lookup"><span data-stu-id="4367f-195">You can set a custom name for your Intel® Edison board.</span></span> <span data-ttu-id="4367f-196">Det här är valfritt.</span><span class="sxs-lookup"><span data-stu-id="4367f-196">This is optional.</span></span>
3. <span data-ttu-id="4367f-197">Ange ett lösenord för dina ändringar och klicka sedan på `Set password`.</span><span class="sxs-lookup"><span data-stu-id="4367f-197">Type a password for your board, then click `Set password`.</span></span>
4. <span data-ttu-id="4367f-198">Anteckna hello lösenord, som används senare.</span><span class="sxs-lookup"><span data-stu-id="4367f-198">Mark down hello password, which is used later.</span></span>

### <a name="connect-wi-fi"></a><span data-ttu-id="4367f-199">Ansluta Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="4367f-199">Connect Wi-Fi</span></span>
1. <span data-ttu-id="4367f-200">På hello `Set up options` klickar du på `Connect Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="4367f-200">On hello `Set up options` page, click `Connect Wi-Fi`.</span></span> <span data-ttu-id="4367f-201">Vänta upp tooone minut som din dator sökningar efter tillgängliga Wi-Fi-nätverk.</span><span class="sxs-lookup"><span data-stu-id="4367f-201">Wait up tooone minute as your computer scans for available Wi-Fi networks.</span></span>
2. <span data-ttu-id="4367f-202">Från hello `Detected Networks` listrutan väljer du nätverket.</span><span class="sxs-lookup"><span data-stu-id="4367f-202">From hello `Detected Networks` drop-down list, select your network.</span></span>
3. <span data-ttu-id="4367f-203">Från hello `Security` listrutan, Välj hello nätverket säkerhetstyp.</span><span class="sxs-lookup"><span data-stu-id="4367f-203">From hello `Security` drop-down list, select hello network's security type.</span></span>
4. <span data-ttu-id="4367f-204">Ange din information inloggningsnamn och lösenord och klicka sedan på `Configure Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="4367f-204">Provide your login and password information, then click `Configure Wi-Fi`.</span></span>
5. <span data-ttu-id="4367f-205">Anteckna hello IP-adress som används senare.</span><span class="sxs-lookup"><span data-stu-id="4367f-205">Mark down hello IP address, which is used later.</span></span>

> [!NOTE]
> <span data-ttu-id="4367f-206">Se till att modern är anslutna toohello samma nätverk som datorn.</span><span class="sxs-lookup"><span data-stu-id="4367f-206">Make sure that Edison is connected toohello same network as your computer.</span></span> <span data-ttu-id="4367f-207">Datorn ansluter tooyour modern med hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="4367f-207">Your computer connects tooyour Edison by using hello IP address.</span></span>

   ![Ansluta tootemperature sensor](media/iot-hub-intel-edison-kit-node-get-started/12_configuration_tool.png)

<span data-ttu-id="4367f-209">Grattis!</span><span class="sxs-lookup"><span data-stu-id="4367f-209">Congratulations!</span></span> <span data-ttu-id="4367f-210">Du har konfigurerat modern.</span><span class="sxs-lookup"><span data-stu-id="4367f-210">You've successfully configured Edison.</span></span>

## <a name="run-a-sample-application-on-intel-edison"></a><span data-ttu-id="4367f-211">Kör ett exempelprogram på Intel modern</span><span class="sxs-lookup"><span data-stu-id="4367f-211">Run a sample application on Intel Edison</span></span>

### <a name="prepare-hello-azure-iot-device-sdk"></a><span data-ttu-id="4367f-212">Förbereda hello Azure SDK för IoT-enhet</span><span class="sxs-lookup"><span data-stu-id="4367f-212">Prepare hello Azure IoT Device SDK</span></span>

1. <span data-ttu-id="4367f-213">Använd någon av följande SSH-klienter från din värd datorn tooconnect tooyour Intel modern hello.</span><span class="sxs-lookup"><span data-stu-id="4367f-213">Use one of hello following SSH clients from your host computer tooconnect tooyour Intel Edison.</span></span> <span data-ttu-id="4367f-214">hello IP-adressen är från hello konfigurationsverktyget och hello lösenordet är hello något du har angett i verktyget.</span><span class="sxs-lookup"><span data-stu-id="4367f-214">hello IP address is from hello configuration tool and hello password is hello one you've set in that tool.</span></span>
    - <span data-ttu-id="4367f-215">[PuTTY](http://www.putty.org/) för Windows.</span><span class="sxs-lookup"><span data-stu-id="4367f-215">[PuTTY](http://www.putty.org/) for Windows.</span></span>
    - <span data-ttu-id="4367f-216">hello inbyggda SSH-klienten på Ubuntu eller macOS.</span><span class="sxs-lookup"><span data-stu-id="4367f-216">hello built-in SSH client on Ubuntu or macOS.</span></span>

2. <span data-ttu-id="4367f-217">Klona hello exempel app tooyour klientenheten.</span><span class="sxs-lookup"><span data-stu-id="4367f-217">Clone hello sample client app tooyour device.</span></span> 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-intel-edison-client-app
   ```

3. <span data-ttu-id="4367f-218">Gå sedan följande kommando tooinstall toohello lagringsplatsen mappen toorun hello alla paket kan det ta serval minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="4367f-218">Then navigate toohello repo folder toorun hello following command tooinstall all packages, it may take serval minutes toocomplete.</span></span>
   
   ```bash
   cd iot-hub-node-intel-edison-client-app
   npm install
   ```


### <a name="configure-and-run-hello-sample-application"></a><span data-ttu-id="4367f-219">Konfigurera och köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="4367f-219">Configure and run hello sample application</span></span>

1. <span data-ttu-id="4367f-220">Öppna hello config-fil genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="4367f-220">Open hello config file by running hello following commands:</span></span>

   ```bash
   nano config.json
   ```

   ![Config-fil](media/iot-hub-intel-edison-kit-node-get-started/13_configure_file.png)

   <span data-ttu-id="4367f-222">Det finns två makron i den här filen kan du configurate.</span><span class="sxs-lookup"><span data-stu-id="4367f-222">There are two macros in this file you can configurate.</span></span> <span data-ttu-id="4367f-223">hello först en är `INTERVAL`, som definierar hello tidsintervallet mellan två meddelanden som skickar toocloud.</span><span class="sxs-lookup"><span data-stu-id="4367f-223">hello first one is `INTERVAL`, which defines hello time interval between two messages that send toocloud.</span></span> <span data-ttu-id="4367f-224">Hej andra `simulatedData`, vilket är ett booleskt värde för om toouse simulerade sensordata eller inte.</span><span class="sxs-lookup"><span data-stu-id="4367f-224">hello second one `simulatedData`,which is a Boolean value for whether toouse simulated sensor data or not.</span></span>

   <span data-ttu-id="4367f-225">Om du **saknar hello sensor**, Ställ in hello `simulatedData` värdet för`true` toomake hello exempelprogrammet skapa och använda simulerade sensordata.</span><span class="sxs-lookup"><span data-stu-id="4367f-225">If you **don't have hello sensor**, set hello `simulatedData` value too`true` toomake hello sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="4367f-226">Spara och avsluta genom att trycka på CTRL-O > Ange > kontrollen X.</span><span class="sxs-lookup"><span data-stu-id="4367f-226">Save and exit by pressing Control-O > Enter > Control-X.</span></span>


1. <span data-ttu-id="4367f-227">Kör exempelprogrammet hello genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="4367f-227">Run hello sample application by running hello following command:</span></span>

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   <span data-ttu-id="4367f-228">Kontrollera att du kopiera / klistra in anslutningssträngen för hello enhet till hello enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="4367f-228">Make sure you copy-paste hello device connection string into hello single quotes.</span></span>

<span data-ttu-id="4367f-229">Du bör se hello följande utdata som visar hello sensor data och hello meddelanden som skickas tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4367f-229">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub.</span></span>

![Utdata - sensordata som skickas från Intel modern tooyour IoT-hubb](media/iot-hub-intel-edison-kit-node-get-started/15_message_sent.png)

## <a name="next-steps"></a><span data-ttu-id="4367f-231">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4367f-231">Next steps</span></span>

<span data-ttu-id="4367f-232">Du har kört ett program toocollect sensor exempeldata och skicka den tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="4367f-232">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
