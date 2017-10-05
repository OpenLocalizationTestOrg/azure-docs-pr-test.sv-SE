---
title: 'Connect Arduino (C) till Azure IoT - lektionen 1: Konfigurera enhet | Microsoft Docs'
description: "Konfigurera Adafruit ludd M0 WiFi för första gången."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino ställer in, ansluta arduino till pc, installationsprogrammet arduino, arduino-kort"
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
ms.openlocfilehash: 9e319292e5d30dea7e45857e435825861aad1c84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-device"></a><span data-ttu-id="8af55-104">Konfigurera din enhet</span><span class="sxs-lookup"><span data-stu-id="8af55-104">Configure your device</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="8af55-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="8af55-105">What you will do</span></span>
<span data-ttu-id="8af55-106">Konfigurera din Adafruit ludd M0 WiFi Arduino board för första gången som samlar in ändringar, startar upp.</span><span class="sxs-lookup"><span data-stu-id="8af55-106">Configure your Adafruit Feather M0 WiFi Arduino board for first-time use by assembling the board, powering it up.</span></span> <span data-ttu-id="8af55-107">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="8af55-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span></span>

## <a name="what-you-need"></a><span data-ttu-id="8af55-108">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="8af55-108">What you need</span></span>
<span data-ttu-id="8af55-109">För att slutföra den här åtgärden behöver du följande delar för din startpaket för Adafruit ludd M0 Wi-Fi:</span><span class="sxs-lookup"><span data-stu-id="8af55-109">To complete this operation, you need the following parts for your Adafruit Feather M0 WiFi Starter Kit:</span></span>

* <span data-ttu-id="8af55-110">Adafruit ludd M0 WiFi-kort</span><span class="sxs-lookup"><span data-stu-id="8af55-110">The Adafruit Feather M0 WiFi board</span></span>
* <span data-ttu-id="8af55-111">En Micro B till typ A USB-kabel</span><span class="sxs-lookup"><span data-stu-id="8af55-111">A Micro B to Type A USB cable</span></span>

![Kit][kit]

<span data-ttu-id="8af55-113">Du behöver också:</span><span class="sxs-lookup"><span data-stu-id="8af55-113">You also need:</span></span>

* <span data-ttu-id="8af55-114">En dator som kör Windows, Mac eller Linux.</span><span class="sxs-lookup"><span data-stu-id="8af55-114">A computer running Windows, Mac, or Linux.</span></span>
* <span data-ttu-id="8af55-115">En trådlös anslutning för Arduino-kort att ansluta till.</span><span class="sxs-lookup"><span data-stu-id="8af55-115">A wireless connection for your Arduino board to connect to.</span></span>
* <span data-ttu-id="8af55-116">En Internet-anslutning att hämta verktyget för serverkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="8af55-116">An Internet connection to download configuration tool.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="8af55-117">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="8af55-117">What you will learn</span></span>
<span data-ttu-id="8af55-118">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="8af55-118">In this article, you will learn:</span></span>

* <span data-ttu-id="8af55-119">Så här montera Arduino-kort och slår upp för följande erfarenheter.</span><span class="sxs-lookup"><span data-stu-id="8af55-119">How to assemble your Arduino board and power it up for the following lessons.</span></span>
* <span data-ttu-id="8af55-120">Hur du lägger till seriell port behörigheter på Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="8af55-120">How to add serial port permissions on Ubuntu.</span></span>

## <a name="connect-your-arduino-board-to-your-computer"></a><span data-ttu-id="8af55-121">Ansluta Arduino-kort till din dator</span><span class="sxs-lookup"><span data-stu-id="8af55-121">Connect your Arduino board to your computer</span></span>

1. <span data-ttu-id="8af55-122">Anslut micro USB-kabel till översta micro USB-port.</span><span class="sxs-lookup"><span data-stu-id="8af55-122">Plug the micro USB cable into the top micro USB port.</span></span>

   ![Övre micro USB-port][top-micro-usb-port]

2. <span data-ttu-id="8af55-124">Anslut den andra änden av USB-kabel till din dator.</span><span class="sxs-lookup"><span data-stu-id="8af55-124">Plug the other end of USB cable into your computer.</span></span>

   ![Datorn USB][computer-usb]

## <a name="add-serial-port-permissions-on-ubuntu"></a><span data-ttu-id="8af55-126">Lägg till behörigheter för seriell port på Ubuntu</span><span class="sxs-lookup"><span data-stu-id="8af55-126">Add serial port permissions on Ubuntu</span></span>

<span data-ttu-id="8af55-127">Du kan hoppa över det här avsnittet om du använder Windows eller macOS.</span><span class="sxs-lookup"><span data-stu-id="8af55-127">You can skip this section if you use Windows or macOS.</span></span> <span data-ttu-id="8af55-128">För Ubuntu behöver du följande steg för att kontrollera att den normala linux-användaren har behörighet att använda USB-port för Arduino-kortet.</span><span class="sxs-lookup"><span data-stu-id="8af55-128">For Ubuntu, you need the following steps to make sure the normal linux user has the permissions to operate on the USB port of your Arduino board.</span></span>

1. <span data-ttu-id="8af55-129">Nu som normal användare från terminalen:</span><span class="sxs-lookup"><span data-stu-id="8af55-129">Now as normal user from terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   # Or
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="8af55-130">Du får något som liknar:</span><span class="sxs-lookup"><span data-stu-id="8af55-130">You will get something like:</span></span>

   ```bash
   crw-rw---- 1 root uucp 188, 0 5 apr 23.01 ttyUSB0
   # Or
   crw-rw---- 1 root dialout 188, 0 5 apr 23.01 ttyACM0
   ```

   <span data-ttu-id="8af55-131">”0” kan vara ett annat nummer eller flera poster kan returneras.</span><span class="sxs-lookup"><span data-stu-id="8af55-131">The "0" might be a different number, or multiple entries might be returned.</span></span> <span data-ttu-id="8af55-132">I det första fallet är de data vi behöver `uucp`, i andra är `dialout`, vilket är gruppägare av filen.</span><span class="sxs-lookup"><span data-stu-id="8af55-132">In the first case the data we need is `uucp`, in the second is `dialout`, which is the group owner of the file.</span></span>

2. <span data-ttu-id="8af55-133">Lägg till användaren till den i gruppen:</span><span class="sxs-lookup"><span data-stu-id="8af55-133">Add user to the to the group:</span></span>

   ```bash
   sudo usermod -a -G group-name username
   ```

   <span data-ttu-id="8af55-134">Där `group-name` är den information som finns i det första steget och `username` är ditt linux-användarnamn.</span><span class="sxs-lookup"><span data-stu-id="8af55-134">Where `group-name` is the data found in the first step, and `username` is your linux user name.</span></span>

3. <span data-ttu-id="8af55-135">Du måste logga ut och in igen för att ändringen ska börja gälla och slutföra installationen.</span><span class="sxs-lookup"><span data-stu-id="8af55-135">You will need to log out and in again for this change to take effect and complete the setup.</span></span>

## <a name="summary"></a><span data-ttu-id="8af55-136">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="8af55-136">Summary</span></span>
<span data-ttu-id="8af55-137">Du har lärt dig hur du konfigurerar Arduino-kort i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="8af55-137">In this article, you’ve learned how to configure your Arduino board.</span></span> <span data-ttu-id="8af55-138">Nästa uppgift är att installera verktyg och program som förberedelse för att köra ett exempelprogram på Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="8af55-138">The next task is to install the necessary tools and software in preparation for running a sample application on your Arduino board.</span></span>

![Maskinvara är klar][hardware-is-ready]

## <a name="next-steps"></a><span data-ttu-id="8af55-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8af55-140">Next steps</span></span>
<span data-ttu-id="8af55-141">[Skaffa dig verktyg][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="8af55-141">[Get the tools][get-the-tools]</span></span>
<!-- Images and links -->

[kit]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/kit.png
[top-micro-usb-port]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/top_usbport.jpg
[computer-usb]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/computer_usb.jpg
[hardware-is-ready]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/hardware_ready.jpg
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md