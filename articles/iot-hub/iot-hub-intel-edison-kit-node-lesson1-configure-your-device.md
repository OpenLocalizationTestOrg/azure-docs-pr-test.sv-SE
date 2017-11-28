---
title: 'Ansluta Intel modern (nod) tooAzure IoT - lektionen 1: Konfigurera enhet | Microsoft Docs'
description: "Konfigurera Intel modern för första gången."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino ställer in, ansluta arduino toopc, installationsprogrammet arduino, arduino-kort"
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
ms.openlocfilehash: c0609542a49924c979f71297d808c09acefca0bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-intel-edison"></a><span data-ttu-id="007bb-104">Konfigurera Intel-modern</span><span class="sxs-lookup"><span data-stu-id="007bb-104">Configure your Intel Edison</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="007bb-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="007bb-105">What you will do</span></span>
<span data-ttu-id="007bb-106">Konfigurera Intel modern för första gången som samlar in hello ändringar, startar upp och installera configuration tool tooyour skrivbord OS tooflash Moderns inbyggd programvara, ange sitt lösenord och ansluta den tooWi-Fi.</span><span class="sxs-lookup"><span data-stu-id="007bb-106">Configure Intel Edison for first-time use by assembling hello board, powering it up and installing configuration tool tooyour desktop OS tooflash Edison's firmware, set its password and connect it tooWi-Fi.</span></span> <span data-ttu-id="007bb-107">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="007bb-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="007bb-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="007bb-108">What you will learn</span></span>
<span data-ttu-id="007bb-109">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="007bb-109">In this article, you will learn:</span></span>

* <span data-ttu-id="007bb-110">Hur tooassemble modern board och slår upp.</span><span class="sxs-lookup"><span data-stu-id="007bb-110">How tooassemble Edison board and power it up.</span></span>
* <span data-ttu-id="007bb-111">Hur tooflash modern inbyggd programvara, ange lösenordet och Anslut Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="007bb-111">How tooflash Edison's firmware, set password and connect Wi-Fi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="007bb-112">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="007bb-112">What you need</span></span>
<span data-ttu-id="007bb-113">toocomplete den här åtgärden måste följande delar från din Intel modern startpaket hello:</span><span class="sxs-lookup"><span data-stu-id="007bb-113">toocomplete this operation, you need hello following parts from your Intel Edison Starter Kit:</span></span>

* <span data-ttu-id="007bb-114">Intel® modern modul</span><span class="sxs-lookup"><span data-stu-id="007bb-114">Intel® Edison module</span></span>
* <span data-ttu-id="007bb-115">Arduino expansion-kort</span><span class="sxs-lookup"><span data-stu-id="007bb-115">Arduino expansion board</span></span>
* <span data-ttu-id="007bb-116">En GIF-staplar eller skruvar som ingår i hello paketering, inklusive två skruvar toofasten hello modulen toohello expansionskort och fyra uppsättningar av skruvar och form mellanrum.</span><span class="sxs-lookup"><span data-stu-id="007bb-116">Any spacer bars or screws included in hello packaging, including two screws toofasten hello module toohello expansion board and four sets of screws and plastic spacers.</span></span>
* <span data-ttu-id="007bb-117">Micro B-tooType en USB-kabel</span><span class="sxs-lookup"><span data-stu-id="007bb-117">A Micro B tooType A USB cable</span></span>
* <span data-ttu-id="007bb-118">En strömförsörjning direct aktuella (DC).</span><span class="sxs-lookup"><span data-stu-id="007bb-118">A direct current (DC) power supply.</span></span> <span data-ttu-id="007bb-119">Din strömförsörjning bör klassificeras enligt följande:</span><span class="sxs-lookup"><span data-stu-id="007bb-119">Your power supply should be rated as follows:</span></span>
  - <span data-ttu-id="007bb-120">7 15V DC</span><span class="sxs-lookup"><span data-stu-id="007bb-120">7-15V DC</span></span>
  - <span data-ttu-id="007bb-121">Minst 1500mA</span><span class="sxs-lookup"><span data-stu-id="007bb-121">At least 1500mA</span></span>
  - <span data-ttu-id="007bb-122">hello center/inre PIN-kod måste vara positiva hello Polen av hello-strömförsörjning</span><span class="sxs-lookup"><span data-stu-id="007bb-122">hello center/inner pin should be hello positive pole of hello power supply</span></span>

  ![Saker i din startpaket](media/iot-hub-intel-edison-lessons/lesson1/kit.png)

<span data-ttu-id="007bb-124">Du behöver också:</span><span class="sxs-lookup"><span data-stu-id="007bb-124">You also need:</span></span>

* <span data-ttu-id="007bb-125">En dator som kör Windows, Mac eller Linux.</span><span class="sxs-lookup"><span data-stu-id="007bb-125">A computer running Windows, Mac, or Linux.</span></span>
* <span data-ttu-id="007bb-126">En trådlös anslutning för modern tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="007bb-126">A wireless connection for Edison tooconnect to.</span></span>
* <span data-ttu-id="007bb-127">En Internet-anslutning toodownload-konfigurationsverktyget.</span><span class="sxs-lookup"><span data-stu-id="007bb-127">An Internet connection toodownload configuration tool.</span></span>

## <a name="assemble-your-board"></a><span data-ttu-id="007bb-128">Assemblera kortets</span><span class="sxs-lookup"><span data-stu-id="007bb-128">Assemble your board</span></span>

<span data-ttu-id="007bb-129">Det här avsnittet innehåller steg tooattach din Intel® modern modulen tooyour expansionskort.</span><span class="sxs-lookup"><span data-stu-id="007bb-129">This section contains steps tooattach your Intel® Edison module tooyour expansion board.</span></span>

1. <span data-ttu-id="007bb-130">Placera hello Intel® modern modul i hello vit disposition på expansionskort, Rada upp hello hål på hello modulen med hello skruvar på hello expansion-kort.</span><span class="sxs-lookup"><span data-stu-id="007bb-130">Place hello Intel® Edison module within hello white outline on your expansion board, lining up hello holes on hello module with hello screws on hello expansion board.</span></span>

2. <span data-ttu-id="007bb-131">Tryck ned på hello modulen nedanför hello ord `What will you make?` tills du anser att en snapin.</span><span class="sxs-lookup"><span data-stu-id="007bb-131">Press down on hello module just below hello words `What will you make?` until you feel a snap.</span></span>

   ![Assemblera board 2](media/iot-hub-intel-edison-lessons/lesson1/assemble_board2.jpg)

3. <span data-ttu-id="007bb-133">Använd hello två hexadecimala nuts (ingår i hello-paket) toosecure hello modulen toohello expansionskort.</span><span class="sxs-lookup"><span data-stu-id="007bb-133">Use hello two hex nuts (included in hello package) toosecure hello module toohello expansion board.</span></span>

   ![Assemblera board 3](media/iot-hub-intel-edison-lessons/lesson1/assemble_board3.jpg)

4. <span data-ttu-id="007bb-135">Infoga en skruv i de hello fyra hörnet hål på hello expansion-kort.</span><span class="sxs-lookup"><span data-stu-id="007bb-135">Insert a screw in one of hello four corner holes on hello expansion board.</span></span> <span data-ttu-id="007bb-136">Genom att vrida och öka en vit hello form mellanrum till hello skruv.</span><span class="sxs-lookup"><span data-stu-id="007bb-136">Twist and tighten one of hello white plastic spacers onto hello screw.</span></span>

   ![Assemblera board 4](media/iot-hub-intel-edison-lessons/lesson1/assemble_board4.jpg)

5. <span data-ttu-id="007bb-138">Upprepa för hello andra tre hörnet mellanrum.</span><span class="sxs-lookup"><span data-stu-id="007bb-138">Repeat for hello other three corner spacers.</span></span>

   ![Assemblera board 5](media/iot-hub-intel-edison-lessons/lesson1/assemble_board5.jpg)

<span data-ttu-id="007bb-140">Nu är kortets monterad.</span><span class="sxs-lookup"><span data-stu-id="007bb-140">Now your board is assembled.</span></span>

   ![monterade board](media/iot-hub-intel-edison-lessons/lesson1/assembled_board.jpg)

## <a name="power-up-edison"></a><span data-ttu-id="007bb-142">Använd modern</span><span class="sxs-lookup"><span data-stu-id="007bb-142">Power up Edison</span></span>

1. <span data-ttu-id="007bb-143">Anslut hello strömförsörjning.</span><span class="sxs-lookup"><span data-stu-id="007bb-143">Plug in hello power supply.</span></span>

   ![Plugin-strömförsörjning](media/iot-hub-intel-edison-lessons/lesson1/plug_power.jpg)

2. <span data-ttu-id="007bb-145">En grön Indikator (heter DS1 på hello Arduino * expansion board) ska lysa upp och hålla upplysta.</span><span class="sxs-lookup"><span data-stu-id="007bb-145">A green LED(labeled DS1 on hello Arduino* expansion board) should light up and stay lit.</span></span>

3. <span data-ttu-id="007bb-146">Vänta en minut för hello board toofinish startas.</span><span class="sxs-lookup"><span data-stu-id="007bb-146">Wait one minute for hello board toofinish booting up.</span></span>

   > [!NOTE]
   > <span data-ttu-id="007bb-147">Om du inte har en DC-strömförsörjning kan du fortfarande power hello board via en USB-port.</span><span class="sxs-lookup"><span data-stu-id="007bb-147">If you do not have a DC power supply, you can still power hello board through a USB port.</span></span> <span data-ttu-id="007bb-148">Se `Connect Edison tooyour computer` information.</span><span class="sxs-lookup"><span data-stu-id="007bb-148">See `Connect Edison tooyour computer` section for details.</span></span> <span data-ttu-id="007bb-149">Startar din board på detta sätt kan resultera i oväntade funktionssätt från dina ändringar, särskilt när med hjälp av Wi-Fi eller drivande motorer.</span><span class="sxs-lookup"><span data-stu-id="007bb-149">Powering your board in this fashion may result in unpredictable behavior from your board, especially when using Wi-Fi or driving motors.</span></span>

## <a name="connect-edison-tooyour-computer"></a><span data-ttu-id="007bb-150">Ansluta modern tooyour dator</span><span class="sxs-lookup"><span data-stu-id="007bb-150">Connect Edison tooyour computer</span></span>

1. <span data-ttu-id="007bb-151">Växla ned hello microswitch mot hello två micro USB-portar, så att modern enhet-läge.</span><span class="sxs-lookup"><span data-stu-id="007bb-151">Toggle down hello microswitch towards hello two micro USB ports, so that Edison is in device mode.</span></span> <span data-ttu-id="007bb-152">Skillnader mellan enheten och värden läge referera [här](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span><span class="sxs-lookup"><span data-stu-id="007bb-152">For differences between device mode and host mode, please reference [here](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span></span>

   ![Växla ned hello microswitch](media/iot-hub-intel-edison-lessons/lesson1/toggle_down_microswitch.jpg)

2. <span data-ttu-id="007bb-154">Anslut hello micro USB-kabel till hello översta micro USB-port.</span><span class="sxs-lookup"><span data-stu-id="007bb-154">Plug hello micro USB cable into hello top micro USB port.</span></span>

   ![Övre micro USB-port](media/iot-hub-intel-edison-lessons/lesson1/top_usbport.jpg)

3. <span data-ttu-id="007bb-156">Plug hello andra änden av USB-kabel till datorn.</span><span class="sxs-lookup"><span data-stu-id="007bb-156">Plug hello other end of USB cable into your computer.</span></span>

   ![Datorn USB](media/iot-hub-intel-edison-lessons/lesson1/computer_usb.jpg)

4. <span data-ttu-id="007bb-158">Vet du kortets initieras fullständigt när datorn monterar en ny enhet (ungefär som att lägga till ett SD-kort i datorn).</span><span class="sxs-lookup"><span data-stu-id="007bb-158">You will know that your board is fully initialized when your computer mounts a new drive (much like inserting a SD card into your computer).</span></span>

## <a name="download-and-run-hello-configuration-tool"></a><span data-ttu-id="007bb-159">Hämta och kör konfigurationsverktyget för hello</span><span class="sxs-lookup"><span data-stu-id="007bb-159">Download and run hello configuration tool</span></span>
<span data-ttu-id="007bb-160">Hämta hello senaste konfigurationsverktyget från [länken](https://software.intel.com/en-us/iot/hardware/edison/downloads) visas under hello `Installers` rubrik.</span><span class="sxs-lookup"><span data-stu-id="007bb-160">Get hello latest configuration tool from [this link](https://software.intel.com/en-us/iot/hardware/edison/downloads) listed under hello `Installers` heading.</span></span> <span data-ttu-id="007bb-161">Kör verktyget hello och följ dess på skärmen instruktioner, klicka på Nästa om det behövs</span><span class="sxs-lookup"><span data-stu-id="007bb-161">Execute hello tool and follow its on-screen instructions, clicking Next where needed</span></span>

### <a name="flash-firmware"></a><span data-ttu-id="007bb-162">Flash inbyggd programvara</span><span class="sxs-lookup"><span data-stu-id="007bb-162">Flash firmware</span></span>
1. <span data-ttu-id="007bb-163">På hello `Set up options` klickar du på `Flash Firmware`.</span><span class="sxs-lookup"><span data-stu-id="007bb-163">On hello `Set up options` page, click `Flash Firmware`.</span></span>
2. <span data-ttu-id="007bb-164">Välj hello avbildningen tooflash till dina ändringar genom att göra något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="007bb-164">Select hello image tooflash onto your board by doing one of hello following:</span></span>
   - <span data-ttu-id="007bb-165">toodownload och flash din board med hello senaste firmware-avbildning från Intel, Välj `Download hello latest image version xxxx`.</span><span class="sxs-lookup"><span data-stu-id="007bb-165">toodownload and flash your board with hello latest firmware image available from Intel, select `Download hello latest image version xxxx`.</span></span>
   - <span data-ttu-id="007bb-166">tooflash dina ändringar med en avbildning som redan har sparats på din dator, Välj `Select hello local image`.</span><span class="sxs-lookup"><span data-stu-id="007bb-166">tooflash your board with an image you already have saved on your computer, select `Select hello local image`.</span></span> <span data-ttu-id="007bb-167">Bläddra tooand väljer hello bild tooflash tooyour-kort.</span><span class="sxs-lookup"><span data-stu-id="007bb-167">Browse tooand select hello image you want tooflash tooyour board.</span></span>
3. <span data-ttu-id="007bb-168">hello installationsverktyget försöker tooflash kortets.</span><span class="sxs-lookup"><span data-stu-id="007bb-168">hello setup tool will attempt tooflash your board.</span></span> <span data-ttu-id="007bb-169">hello hela blinkande processen kan ta too10 minuter.</span><span class="sxs-lookup"><span data-stu-id="007bb-169">hello entire flashing process may take up too10 minutes.</span></span>

### <a name="set-password"></a><span data-ttu-id="007bb-170">Ange ett lösenord</span><span class="sxs-lookup"><span data-stu-id="007bb-170">Set password</span></span>
1. <span data-ttu-id="007bb-171">På hello `Set up options` klickar du på `Enable Security`.</span><span class="sxs-lookup"><span data-stu-id="007bb-171">On hello `Set up options` page, click `Enable Security`.</span></span>
2. <span data-ttu-id="007bb-172">Du kan ange ett eget namn för Intel® modern-kort.</span><span class="sxs-lookup"><span data-stu-id="007bb-172">You can set a custom name for your Intel® Edison board.</span></span> <span data-ttu-id="007bb-173">Det här är valfritt.</span><span class="sxs-lookup"><span data-stu-id="007bb-173">This is optional.</span></span>
3. <span data-ttu-id="007bb-174">Ange ett lösenord för dina ändringar och klicka sedan på `Set password`.</span><span class="sxs-lookup"><span data-stu-id="007bb-174">Type a password for your board, then click `Set password`.</span></span>
4. <span data-ttu-id="007bb-175">Anteckna hello lösenord, som används senare.</span><span class="sxs-lookup"><span data-stu-id="007bb-175">Mark down hello password, which is used later.</span></span>

### <a name="connect-wi-fi"></a><span data-ttu-id="007bb-176">Ansluta Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="007bb-176">Connect Wi-Fi</span></span>
1. <span data-ttu-id="007bb-177">På hello `Set up options` klickar du på `Connect Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="007bb-177">On hello `Set up options` page, click `Connect Wi-Fi`.</span></span> <span data-ttu-id="007bb-178">Vänta upp tooone minut som din dator sökningar efter tillgängliga Wi-Fi-nätverk.</span><span class="sxs-lookup"><span data-stu-id="007bb-178">Wait up tooone minute as your computer scans for available Wi-Fi networks.</span></span>
2. <span data-ttu-id="007bb-179">Från hello `Detected Networks` listrutan väljer du nätverket.</span><span class="sxs-lookup"><span data-stu-id="007bb-179">From hello `Detected Networks` drop-down list, select your network.</span></span>
3. <span data-ttu-id="007bb-180">Från hello `Security` listrutan, Välj hello nätverket säkerhetstyp.</span><span class="sxs-lookup"><span data-stu-id="007bb-180">From hello `Security` drop-down list, select hello network's security type.</span></span>
4. <span data-ttu-id="007bb-181">Ange din information inloggningsnamn och lösenord och klicka sedan på `Configure Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="007bb-181">Provide your login and password information, then click `Configure Wi-Fi`.</span></span>
5. <span data-ttu-id="007bb-182">Anteckna hello IP-adress som används senare.</span><span class="sxs-lookup"><span data-stu-id="007bb-182">Mark down hello IP address, which is used later.</span></span>

> [!NOTE]
> <span data-ttu-id="007bb-183">Se till att modern är anslutna toohello samma nätverk som datorn.</span><span class="sxs-lookup"><span data-stu-id="007bb-183">Make sure that Edison is connected toohello same network as your computer.</span></span> <span data-ttu-id="007bb-184">Datorn ansluter tooyour modern med hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="007bb-184">Your computer connects tooyour Edison by using hello IP address.</span></span>

<span data-ttu-id="007bb-185">Grattis!</span><span class="sxs-lookup"><span data-stu-id="007bb-185">Congratulations!</span></span> <span data-ttu-id="007bb-186">Du har konfigurerat modern.</span><span class="sxs-lookup"><span data-stu-id="007bb-186">You've successfully configured Edison.</span></span>

## <a name="summary"></a><span data-ttu-id="007bb-187">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="007bb-187">Summary</span></span>
<span data-ttu-id="007bb-188">I den här artikeln har du lärt dig hur tooassemble hello modern board, flash dess inbyggd programvara, installationsprogrammet lösenord och ansluta tooWi-Fi med hjälp av verktyget för serverkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="007bb-188">In this article, you’ve learned how tooassemble hello Edison board, flash its firmware, setup password and connect it tooWi-Fi by using configuration tool.</span></span> <span data-ttu-id="007bb-189">Observera att hello Indikator ännu inte lysa upp.</span><span class="sxs-lookup"><span data-stu-id="007bb-189">Note that hello LED doesn't yet light up.</span></span> <span data-ttu-id="007bb-190">hello nästa uppgift är tooinstall hello nödvändiga verktyg och program som förberedelse för att köra ett exempelprogram på modern.</span><span class="sxs-lookup"><span data-stu-id="007bb-190">hello next task is tooinstall hello necessary tools and software in preparation for running a sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="007bb-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="007bb-191">Next steps</span></span>
<span data-ttu-id="007bb-192">[Hämta hello-verktyg][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="007bb-192">[Get hello tools][get-the-tools]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md