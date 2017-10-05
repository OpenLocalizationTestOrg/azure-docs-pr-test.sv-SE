---
title: 'Ansluta Intel modern (nod) till Azure IoT - lektionen 1: Konfigurera enhet | Microsoft Docs'
description: "Konfigurera Intel modern för första gången."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino ställer in, ansluta arduino till pc, installationsprogrammet arduino, arduino-kort"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 372c9b6d-e701-4ff6-8151-d262aa76aa55
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 87bf3a917af096e43a43a2143afa4bf43a72d7fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-intel-edison"></a><span data-ttu-id="f7441-104">Konfigurera Intel-modern</span><span class="sxs-lookup"><span data-stu-id="f7441-104">Configure your Intel Edison</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="f7441-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="f7441-105">What you will do</span></span>
<span data-ttu-id="f7441-106">Konfigurera Intel modern för första gången som samlar in ändringar, startar upp och installera verktyget för serverkonfiguration för ditt skrivbord OS till flash Moderns inbyggd programvara, ange sitt lösenord och ansluter till Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="f7441-106">Configure Intel Edison for first-time use by assembling the board, powering it up and installing configuration tool to your desktop OS to flash Edison's firmware, set its password and connect it to Wi-Fi.</span></span> <span data-ttu-id="f7441-107">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="f7441-107">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f7441-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="f7441-108">What you will learn</span></span>
<span data-ttu-id="f7441-109">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="f7441-109">In this article, you will learn:</span></span>

* <span data-ttu-id="f7441-110">Så här montering modern board och slår upp.</span><span class="sxs-lookup"><span data-stu-id="f7441-110">How to assemble Edison board and power it up.</span></span>
* <span data-ttu-id="f7441-111">Så här flash Moderns inbyggd programvara, ange lösenordet och ansluta Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="f7441-111">How to flash Edison's firmware, set password and connect Wi-Fi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f7441-112">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="f7441-112">What you need</span></span>
<span data-ttu-id="f7441-113">För att slutföra den här åtgärden behöver du följande delar från din Intel modern startpaket:</span><span class="sxs-lookup"><span data-stu-id="f7441-113">To complete this operation, you need the following parts from your Intel Edison Starter Kit:</span></span>

* <span data-ttu-id="f7441-114">Intel® modern modul</span><span class="sxs-lookup"><span data-stu-id="f7441-114">Intel® Edison module</span></span>
* <span data-ttu-id="f7441-115">Arduino expansion-kort</span><span class="sxs-lookup"><span data-stu-id="f7441-115">Arduino expansion board</span></span>
* <span data-ttu-id="f7441-116">En GIF-staplar eller skruvar som ingår i paketering, inklusive två skruvar för att fästa modulen som ska expansionskort och fyra uppsättningar av skruvar och form mellanrum.</span><span class="sxs-lookup"><span data-stu-id="f7441-116">Any spacer bars or screws included in the packaging, including two screws to fasten the module to the expansion board and four sets of screws and plastic spacers.</span></span>
* <span data-ttu-id="f7441-117">En Micro B till typ A USB-kabel</span><span class="sxs-lookup"><span data-stu-id="f7441-117">A Micro B to Type A USB cable</span></span>
* <span data-ttu-id="f7441-118">En strömförsörjning direct aktuella (DC).</span><span class="sxs-lookup"><span data-stu-id="f7441-118">A direct current (DC) power supply.</span></span> <span data-ttu-id="f7441-119">Din strömförsörjning bör klassificeras enligt följande:</span><span class="sxs-lookup"><span data-stu-id="f7441-119">Your power supply should be rated as follows:</span></span>
  - <span data-ttu-id="f7441-120">7 15V DC</span><span class="sxs-lookup"><span data-stu-id="f7441-120">7-15V DC</span></span>
  - <span data-ttu-id="f7441-121">Minst 1500mA</span><span class="sxs-lookup"><span data-stu-id="f7441-121">At least 1500mA</span></span>
  - <span data-ttu-id="f7441-122">Center/inre PIN-koden måste vara positiva Polen för strömförsörjningen</span><span class="sxs-lookup"><span data-stu-id="f7441-122">The center/inner pin should be the positive pole of the power supply</span></span>

  ![Saker i din startpaket](media/iot-hub-intel-edison-lessons/lesson1/kit.png)

<span data-ttu-id="f7441-124">Du behöver också:</span><span class="sxs-lookup"><span data-stu-id="f7441-124">You also need:</span></span>

* <span data-ttu-id="f7441-125">En dator som kör Windows, Mac eller Linux.</span><span class="sxs-lookup"><span data-stu-id="f7441-125">A computer running Windows, Mac, or Linux.</span></span>
* <span data-ttu-id="f7441-126">En trådlös anslutning för modern att ansluta till.</span><span class="sxs-lookup"><span data-stu-id="f7441-126">A wireless connection for Edison to connect to.</span></span>
* <span data-ttu-id="f7441-127">En Internet-anslutning att hämta verktyget för serverkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="f7441-127">An Internet connection to download configuration tool.</span></span>

## <a name="assemble-your-board"></a><span data-ttu-id="f7441-128">Assemblera kortets</span><span class="sxs-lookup"><span data-stu-id="f7441-128">Assemble your board</span></span>

<span data-ttu-id="f7441-129">Det här avsnittet innehåller steg om du vill bifoga Intel® modern-modulen expansionskort.</span><span class="sxs-lookup"><span data-stu-id="f7441-129">This section contains steps to attach your Intel® Edison module to your expansion board.</span></span>

1. <span data-ttu-id="f7441-130">Placera modulen Intel® modern inom vit kontur på expansionskort, Rada upp tomrum i modulen med skruvar på expansionskort.</span><span class="sxs-lookup"><span data-stu-id="f7441-130">Place the Intel® Edison module within the white outline on your expansion board, lining up the holes on the module with the screws on the expansion board.</span></span>

2. <span data-ttu-id="f7441-131">Tryck ned i modulen nedanför orden `What will you make?` tills du anser att en snapin.</span><span class="sxs-lookup"><span data-stu-id="f7441-131">Press down on the module just below the words `What will you make?` until you feel a snap.</span></span>

   ![Assemblera board 2](media/iot-hub-intel-edison-lessons/lesson1/assemble_board2.jpg)

3. <span data-ttu-id="f7441-133">Använd två hexadecimala nuts (ingår i paketet) för att skydda modulen expansion planen.</span><span class="sxs-lookup"><span data-stu-id="f7441-133">Use the two hex nuts (included in the package) to secure the module to the expansion board.</span></span>

   ![Assemblera board 3](media/iot-hub-intel-edison-lessons/lesson1/assemble_board3.jpg)

4. <span data-ttu-id="f7441-135">Infoga en skruv i någon av fyra hörnet hål på expansionskort.</span><span class="sxs-lookup"><span data-stu-id="f7441-135">Insert a screw in one of the four corner holes on the expansion board.</span></span> <span data-ttu-id="f7441-136">Genom att vrida och öka en vit form mellanrum till skruven.</span><span class="sxs-lookup"><span data-stu-id="f7441-136">Twist and tighten one of the white plastic spacers onto the screw.</span></span>

   ![Assemblera board 4](media/iot-hub-intel-edison-lessons/lesson1/assemble_board4.jpg)

5. <span data-ttu-id="f7441-138">Upprepa för de andra tre hörnet mellanrum.</span><span class="sxs-lookup"><span data-stu-id="f7441-138">Repeat for the other three corner spacers.</span></span>

   ![Assemblera board 5](media/iot-hub-intel-edison-lessons/lesson1/assemble_board5.jpg)

<span data-ttu-id="f7441-140">Nu är kortets monterad.</span><span class="sxs-lookup"><span data-stu-id="f7441-140">Now your board is assembled.</span></span>

   ![monterade board](media/iot-hub-intel-edison-lessons/lesson1/assembled_board.jpg)

## <a name="power-up-edison"></a><span data-ttu-id="f7441-142">Använd modern</span><span class="sxs-lookup"><span data-stu-id="f7441-142">Power up Edison</span></span>

1. <span data-ttu-id="f7441-143">Anslut strömförsörjningen.</span><span class="sxs-lookup"><span data-stu-id="f7441-143">Plug in the power supply.</span></span>

   ![Plugin-strömförsörjning](media/iot-hub-intel-edison-lessons/lesson1/plug_power.jpg)

2. <span data-ttu-id="f7441-145">En grön Indikator (heter DS1 på expansion-kort Arduino *) ska lysa upp och hålla upplysta.</span><span class="sxs-lookup"><span data-stu-id="f7441-145">A green LED(labeled DS1 on the Arduino* expansion board) should light up and stay lit.</span></span>

3. <span data-ttu-id="f7441-146">Vänta en minut för board att avsluta startar.</span><span class="sxs-lookup"><span data-stu-id="f7441-146">Wait one minute for the board to finish booting up.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f7441-147">Om du inte har en DC-strömförsörjning kan du fortfarande power board via en USB-port.</span><span class="sxs-lookup"><span data-stu-id="f7441-147">If you do not have a DC power supply, you can still power the board through a USB port.</span></span> <span data-ttu-id="f7441-148">Se `Connect Edison to your computer` information.</span><span class="sxs-lookup"><span data-stu-id="f7441-148">See `Connect Edison to your computer` section for details.</span></span> <span data-ttu-id="f7441-149">Startar din board på detta sätt kan resultera i oväntade funktionssätt från dina ändringar, särskilt när med hjälp av Wi-Fi eller drivande motorer.</span><span class="sxs-lookup"><span data-stu-id="f7441-149">Powering your board in this fashion may result in unpredictable behavior from your board, especially when using Wi-Fi or driving motors.</span></span>

## <a name="connect-edison-to-your-computer"></a><span data-ttu-id="f7441-150">Ansluta modern till din dator</span><span class="sxs-lookup"><span data-stu-id="f7441-150">Connect Edison to your computer</span></span>

1. <span data-ttu-id="f7441-151">Växla ned microswitch mot de två micro USB-portarna så att modern enhet-läge.</span><span class="sxs-lookup"><span data-stu-id="f7441-151">Toggle down the microswitch towards the two micro USB ports, so that Edison is in device mode.</span></span> <span data-ttu-id="f7441-152">Skillnader mellan enheten och värden läge referera [här](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span><span class="sxs-lookup"><span data-stu-id="f7441-152">For differences between device mode and host mode, please reference [here](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span></span>

   ![Växla ned microswitch](media/iot-hub-intel-edison-lessons/lesson1/toggle_down_microswitch.jpg)

2. <span data-ttu-id="f7441-154">Anslut micro USB-kabel till översta micro USB-port.</span><span class="sxs-lookup"><span data-stu-id="f7441-154">Plug the micro USB cable into the top micro USB port.</span></span>

   ![Övre micro USB-port](media/iot-hub-intel-edison-lessons/lesson1/top_usbport.jpg)

3. <span data-ttu-id="f7441-156">Anslut den andra änden av USB-kabel till din dator.</span><span class="sxs-lookup"><span data-stu-id="f7441-156">Plug the other end of USB cable into your computer.</span></span>

   ![Datorn USB](media/iot-hub-intel-edison-lessons/lesson1/computer_usb.jpg)

4. <span data-ttu-id="f7441-158">Vet du kortets initieras fullständigt när datorn monterar en ny enhet (ungefär som att lägga till ett SD-kort i datorn).</span><span class="sxs-lookup"><span data-stu-id="f7441-158">You will know that your board is fully initialized when your computer mounts a new drive (much like inserting a SD card into your computer).</span></span>

## <a name="download-and-run-the-configuration-tool"></a><span data-ttu-id="f7441-159">Hämta och kör verktyget configuration</span><span class="sxs-lookup"><span data-stu-id="f7441-159">Download and run the configuration tool</span></span>
<span data-ttu-id="f7441-160">Hämta den senaste konfiguration för från [länken](https://software.intel.com/en-us/iot/hardware/edison/downloads) visas under den `Installers` rubrik.</span><span class="sxs-lookup"><span data-stu-id="f7441-160">Get the latest configuration tool from [this link](https://software.intel.com/en-us/iot/hardware/edison/downloads) listed under the `Installers` heading.</span></span> <span data-ttu-id="f7441-161">Kör verktyget och följ dess på skärmen instruktioner, klicka på Nästa om det behövs</span><span class="sxs-lookup"><span data-stu-id="f7441-161">Execute the tool and follow its on-screen instructions, clicking Next where needed</span></span>

### <a name="flash-firmware"></a><span data-ttu-id="f7441-162">Flash inbyggd programvara</span><span class="sxs-lookup"><span data-stu-id="f7441-162">Flash firmware</span></span>
1. <span data-ttu-id="f7441-163">På den `Set up options` klickar du på `Flash Firmware`.</span><span class="sxs-lookup"><span data-stu-id="f7441-163">On the `Set up options` page, click `Flash Firmware`.</span></span>
2. <span data-ttu-id="f7441-164">Välj avbildningen som flash till kortets genom att göra något av följande:</span><span class="sxs-lookup"><span data-stu-id="f7441-164">Select the image to flash onto your board by doing one of the following:</span></span>
   - <span data-ttu-id="f7441-165">Du kan hämta och flash din board med den senaste firmware-avbildningen från Intel, Välj `Download the latest image version xxxx`.</span><span class="sxs-lookup"><span data-stu-id="f7441-165">To download and flash your board with the latest firmware image available from Intel, select `Download the latest image version xxxx`.</span></span>
   - <span data-ttu-id="f7441-166">För att flash dina ändringar med en avbildning som redan har sparats på datorn, Välj `Select the local image`.</span><span class="sxs-lookup"><span data-stu-id="f7441-166">To flash your board with an image you already have saved on your computer, select `Select the local image`.</span></span> <span data-ttu-id="f7441-167">Bläddra till och välj den avbildning du vill flash din planen.</span><span class="sxs-lookup"><span data-stu-id="f7441-167">Browse to and select the image you want to flash to your board.</span></span>
3. <span data-ttu-id="f7441-168">Verktyget installationsprogrammet försöker flash kortets.</span><span class="sxs-lookup"><span data-stu-id="f7441-168">The setup tool will attempt to flash your board.</span></span> <span data-ttu-id="f7441-169">Hela blinkande processen kan ta upp till 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="f7441-169">The entire flashing process may take up to 10 minutes.</span></span>

### <a name="set-password"></a><span data-ttu-id="f7441-170">Ange ett lösenord</span><span class="sxs-lookup"><span data-stu-id="f7441-170">Set password</span></span>
1. <span data-ttu-id="f7441-171">På den `Set up options` klickar du på `Enable Security`.</span><span class="sxs-lookup"><span data-stu-id="f7441-171">On the `Set up options` page, click `Enable Security`.</span></span>
2. <span data-ttu-id="f7441-172">Du kan ange ett eget namn för Intel® modern-kort.</span><span class="sxs-lookup"><span data-stu-id="f7441-172">You can set a custom name for your Intel® Edison board.</span></span> <span data-ttu-id="f7441-173">Det här är valfritt.</span><span class="sxs-lookup"><span data-stu-id="f7441-173">This is optional.</span></span>
3. <span data-ttu-id="f7441-174">Ange ett lösenord för dina ändringar och klicka sedan på `Set password`.</span><span class="sxs-lookup"><span data-stu-id="f7441-174">Type a password for your board, then click `Set password`.</span></span>
4. <span data-ttu-id="f7441-175">Anteckna lösenordet som används senare.</span><span class="sxs-lookup"><span data-stu-id="f7441-175">Mark down the password, which is used later.</span></span>

### <a name="connect-wi-fi"></a><span data-ttu-id="f7441-176">Ansluta Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="f7441-176">Connect Wi-Fi</span></span>
1. <span data-ttu-id="f7441-177">På den `Set up options` klickar du på `Connect Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="f7441-177">On the `Set up options` page, click `Connect Wi-Fi`.</span></span> <span data-ttu-id="f7441-178">Vänta i upp till en minut som datorn söker efter tillgängliga Wi-Fi-nätverk.</span><span class="sxs-lookup"><span data-stu-id="f7441-178">Wait up to one minute as your computer scans for available Wi-Fi networks.</span></span>
2. <span data-ttu-id="f7441-179">Från den `Detected Networks` listrutan väljer du nätverket.</span><span class="sxs-lookup"><span data-stu-id="f7441-179">From the `Detected Networks` drop-down list, select your network.</span></span>
3. <span data-ttu-id="f7441-180">Från den `Security` listrutan väljer du nätverkstyp säkerhet.</span><span class="sxs-lookup"><span data-stu-id="f7441-180">From the `Security` drop-down list, select the network's security type.</span></span>
4. <span data-ttu-id="f7441-181">Ange din information inloggningsnamn och lösenord och klicka sedan på `Configure Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="f7441-181">Provide your login and password information, then click `Configure Wi-Fi`.</span></span>
5. <span data-ttu-id="f7441-182">Anteckna den IP-adressen som används senare.</span><span class="sxs-lookup"><span data-stu-id="f7441-182">Mark down the IP address, which is used later.</span></span>

> [!NOTE]
> <span data-ttu-id="f7441-183">Kontrollera att modern är ansluten till samma nätverk som datorn.</span><span class="sxs-lookup"><span data-stu-id="f7441-183">Make sure that Edison is connected to the same network as your computer.</span></span> <span data-ttu-id="f7441-184">Datorn ansluter till din modern med hjälp av IP-adress.</span><span class="sxs-lookup"><span data-stu-id="f7441-184">Your computer connects to your Edison by using the IP address.</span></span>

<span data-ttu-id="f7441-185">Grattis!</span><span class="sxs-lookup"><span data-stu-id="f7441-185">Congratulations!</span></span> <span data-ttu-id="f7441-186">Du har konfigurerat modern.</span><span class="sxs-lookup"><span data-stu-id="f7441-186">You've successfully configured Edison.</span></span>

## <a name="summary"></a><span data-ttu-id="f7441-187">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f7441-187">Summary</span></span>
<span data-ttu-id="f7441-188">Du har lärt dig hur du montera modern board, flash dess inbyggd programvara, installationsprogrammet lösenord och ansluter till Wi-Fi med hjälp av verktyget för serverkonfiguration i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f7441-188">In this article, you’ve learned how to assemble the Edison board, flash its firmware, setup password and connect it to Wi-Fi by using configuration tool.</span></span> <span data-ttu-id="f7441-189">Observera att Indikatorn ännu inte lysa upp.</span><span class="sxs-lookup"><span data-stu-id="f7441-189">Note that the LED doesn't yet light up.</span></span> <span data-ttu-id="f7441-190">Nästa uppgift är att installera verktyg och program som förberedelse för att köra ett exempelprogram på modern.</span><span class="sxs-lookup"><span data-stu-id="f7441-190">The next task is to install the necessary tools and software in preparation for running a sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f7441-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f7441-191">Next steps</span></span>
<span data-ttu-id="f7441-192">[Skaffa dig verktyg][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="f7441-192">[Get the tools][get-the-tools]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md