---
title: 'Connect Arduino (C) tooAzure IoT - lektionen 1: Konfigurera enhet | Microsoft Docs'
description: "Konfigurera Adafruit ludd M0 WiFi för första gången."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino ställer in, ansluta arduino toopc, installationsprogrammet arduino, arduino-kort"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: f5b334f0-a148-41aa-b374-ce7b9f5b305a
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 30b764e8ff6221995456283a226e79f064b2d74e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-device"></a><span data-ttu-id="fcc98-104">Konfigurera din enhet</span><span class="sxs-lookup"><span data-stu-id="fcc98-104">Configure your device</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="fcc98-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="fcc98-105">What you will do</span></span>
<span data-ttu-id="fcc98-106">Konfigurera Adafruit ludd M0 WiFi Arduino-kort för första gången som samlar in hello ändringar, startar upp.</span><span class="sxs-lookup"><span data-stu-id="fcc98-106">Configure your Adafruit Feather M0 WiFi Arduino board for first-time use by assembling hello board, powering it up.</span></span> <span data-ttu-id="fcc98-107">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="fcc98-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span></span>

## <a name="what-you-need"></a><span data-ttu-id="fcc98-108">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="fcc98-108">What you need</span></span>
<span data-ttu-id="fcc98-109">toocomplete den här åtgärden måste hello följande delar för din startpaket för Adafruit ludd M0 Wi-Fi:</span><span class="sxs-lookup"><span data-stu-id="fcc98-109">toocomplete this operation, you need hello following parts for your Adafruit Feather M0 WiFi Starter Kit:</span></span>

* <span data-ttu-id="fcc98-110">Hej Adafruit ludd M0 WiFi-kort</span><span class="sxs-lookup"><span data-stu-id="fcc98-110">hello Adafruit Feather M0 WiFi board</span></span>
* <span data-ttu-id="fcc98-111">Micro B-tooType en USB-kabel</span><span class="sxs-lookup"><span data-stu-id="fcc98-111">A Micro B tooType A USB cable</span></span>

![Kit][kit]

<span data-ttu-id="fcc98-113">Du behöver också:</span><span class="sxs-lookup"><span data-stu-id="fcc98-113">You also need:</span></span>

* <span data-ttu-id="fcc98-114">En dator som kör Windows, Mac eller Linux.</span><span class="sxs-lookup"><span data-stu-id="fcc98-114">A computer running Windows, Mac, or Linux.</span></span>
* <span data-ttu-id="fcc98-115">En trådlös anslutning för din Arduino board tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="fcc98-115">A wireless connection for your Arduino board tooconnect to.</span></span>
* <span data-ttu-id="fcc98-116">En Internet-anslutning toodownload-konfigurationsverktyget.</span><span class="sxs-lookup"><span data-stu-id="fcc98-116">An Internet connection toodownload configuration tool.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="fcc98-117">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="fcc98-117">What you will learn</span></span>
<span data-ttu-id="fcc98-118">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="fcc98-118">In this article, you will learn:</span></span>

* <span data-ttu-id="fcc98-119">Hur tooassemble din Arduino kort och power den för hello följande lektioner.</span><span class="sxs-lookup"><span data-stu-id="fcc98-119">How tooassemble your Arduino board and power it up for hello following lessons.</span></span>
* <span data-ttu-id="fcc98-120">Hur tooadd serieport behörigheter på Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="fcc98-120">How tooadd serial port permissions on Ubuntu.</span></span>

## <a name="connect-your-arduino-board-tooyour-computer"></a><span data-ttu-id="fcc98-121">Anslut datorn Arduino board tooyour</span><span class="sxs-lookup"><span data-stu-id="fcc98-121">Connect your Arduino board tooyour computer</span></span>

1. <span data-ttu-id="fcc98-122">Anslut hello micro USB-kabel till hello översta micro USB-port.</span><span class="sxs-lookup"><span data-stu-id="fcc98-122">Plug hello micro USB cable into hello top micro USB port.</span></span>

   ![Övre micro USB-port][top-micro-usb-port]

2. <span data-ttu-id="fcc98-124">Plug hello andra änden av USB-kabel till datorn.</span><span class="sxs-lookup"><span data-stu-id="fcc98-124">Plug hello other end of USB cable into your computer.</span></span>

   ![Datorn USB][computer-usb]

## <a name="add-serial-port-permissions-on-ubuntu"></a><span data-ttu-id="fcc98-126">Lägg till behörigheter för seriell port på Ubuntu</span><span class="sxs-lookup"><span data-stu-id="fcc98-126">Add serial port permissions on Ubuntu</span></span>

<span data-ttu-id="fcc98-127">Du kan hoppa över det här avsnittet om du använder Windows eller macOS.</span><span class="sxs-lookup"><span data-stu-id="fcc98-127">You can skip this section if you use Windows or macOS.</span></span> <span data-ttu-id="fcc98-128">För Ubuntu behöver du följande steg toomake till hello normal linux användaren har hello behörigheter toooperate på hello USB-port för Arduino-kortet hello.</span><span class="sxs-lookup"><span data-stu-id="fcc98-128">For Ubuntu, you need hello following steps toomake sure hello normal linux user has hello permissions toooperate on hello USB port of your Arduino board.</span></span>

1. <span data-ttu-id="fcc98-129">Nu som normal användare från terminalen:</span><span class="sxs-lookup"><span data-stu-id="fcc98-129">Now as normal user from terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   # Or
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="fcc98-130">Du får något som liknar:</span><span class="sxs-lookup"><span data-stu-id="fcc98-130">You will get something like:</span></span>

   ```bash
   crw-rw---- 1 root uucp 188, 0 5 apr 23.01 ttyUSB0
   # Or
   crw-rw---- 1 root dialout 188, 0 5 apr 23.01 ttyACM0
   ```

   <span data-ttu-id="fcc98-131">Hej ”0” kan vara ett annat nummer eller flera poster kan returneras.</span><span class="sxs-lookup"><span data-stu-id="fcc98-131">hello "0" might be a different number, or multiple entries might be returned.</span></span> <span data-ttu-id="fcc98-132">I hello första case hello data som vi behöver `uucp`, i hello andra är `dialout`, vilket är hello gruppägare hello-filen.</span><span class="sxs-lookup"><span data-stu-id="fcc98-132">In hello first case hello data we need is `uucp`, in hello second is `dialout`, which is hello group owner of hello file.</span></span>

2. <span data-ttu-id="fcc98-133">Lägg till användargrupp toohello toohello:</span><span class="sxs-lookup"><span data-stu-id="fcc98-133">Add user toohello toohello group:</span></span>

   ```bash
   sudo usermod -a -G group-name username
   ```

   <span data-ttu-id="fcc98-134">Där `group-name` är hello data hittades i hello första steget, och `username` är ditt linux-användarnamn.</span><span class="sxs-lookup"><span data-stu-id="fcc98-134">Where `group-name` is hello data found in hello first step, and `username` is your linux user name.</span></span>

3. <span data-ttu-id="fcc98-135">Du behöver toolog ut och in igen för den här ändringen tootake effekt och fullständig hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="fcc98-135">You will need toolog out and in again for this change tootake effect and complete hello setup.</span></span>

## <a name="summary"></a><span data-ttu-id="fcc98-136">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="fcc98-136">Summary</span></span>
<span data-ttu-id="fcc98-137">I den här artikeln har du lärt dig hur tooconfigure Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="fcc98-137">In this article, you’ve learned how tooconfigure your Arduino board.</span></span> <span data-ttu-id="fcc98-138">hello nästa uppgift är tooinstall hello nödvändiga verktyg och program som förberedelse för att köra ett exempelprogram på Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="fcc98-138">hello next task is tooinstall hello necessary tools and software in preparation for running a sample application on your Arduino board.</span></span>

![Maskinvara är klar][hardware-is-ready]

## <a name="next-steps"></a><span data-ttu-id="fcc98-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fcc98-140">Next steps</span></span>
<span data-ttu-id="fcc98-141">[Hämta hello-verktyg][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="fcc98-141">[Get hello tools][get-the-tools]</span></span>
<!-- Images and links -->

[kit]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/kit.png
[top-micro-usb-port]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/top_usbport.jpg
[computer-usb]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/computer_usb.jpg
[hardware-is-ready]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/hardware_ready.jpg
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md