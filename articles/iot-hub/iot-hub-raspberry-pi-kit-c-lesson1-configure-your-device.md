---
title: 'Connect Raspberry PI (C) till Azure IoT - lektionen 1: Konfigurera enhet | Microsoft Docs'
description: "Konfigurera hallon Pi 3 för första gången och installera Raspbian OS, ett ledigt operativsystem som är optimerad för hallon Pi-maskinvara."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "installera raspbian, raspbian download hur pi ansluta för att installera raspbian, raspbian inställningar, raspberry pi installera raspbian, raspberry pi installera os, raspberry pi sd-kort installera, hallon, ansluta till raspberry pi, raspberry pi-anslutning"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 8ee9b23c-93f7-43ff-8ea1-e7761eb87a6f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 2a380f78d67db47a0dcab5b90843404921510528
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-device"></a><span data-ttu-id="c6fa0-104">Konfigurera din enhet</span><span class="sxs-lookup"><span data-stu-id="c6fa0-104">Configure your device</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="c6fa0-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="c6fa0-105">What you will do</span></span>
<span data-ttu-id="c6fa0-106">Konfigurera Pi för första gången och installera operativsystemet Raspbian.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-106">Configure Pi for first-time use and install the Raspbian operating system.</span></span> <span data-ttu-id="c6fa0-107">Raspbian är ett kostnadsfritt operativsystem som är optimerad för hallon Pi-maskinvara.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-107">Raspbian is a free operating system that is optimized for the Raspberry Pi hardware.</span></span> <span data-ttu-id="c6fa0-108">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="c6fa0-108">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="c6fa0-109">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="c6fa0-109">What you will learn</span></span>
<span data-ttu-id="c6fa0-110">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="c6fa0-110">In this article, you will learn:</span></span>

* <span data-ttu-id="c6fa0-111">Hur du installerar Raspbian på Pi.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-111">How to install Raspbian on Pi.</span></span>
* <span data-ttu-id="c6fa0-112">Så här uppstart Pi med hjälp av en USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-112">How to power up Pi by using a USB cable.</span></span>
* <span data-ttu-id="c6fa0-113">Hur du ansluter Pi till nätverket med hjälp av en Ethernet-kabel eller trådlöst nätverk.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-113">How to connect Pi to the network by using an Ethernet cable or wireless network.</span></span>
* <span data-ttu-id="c6fa0-114">Så här lägger du till en Indikator på breadboard och ansluta till Pi.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-114">How to add an LED to the breadboard and connect it to Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="c6fa0-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="c6fa0-115">What you need</span></span>
<span data-ttu-id="c6fa0-116">För att slutföra den här åtgärden behöver du följande delar från din startpaket för hallon Pi 3:</span><span class="sxs-lookup"><span data-stu-id="c6fa0-116">To complete this operation, you need the following parts from your Raspberry Pi 3 Starter Kit:</span></span>

* <span data-ttu-id="c6fa0-117">Hallon Pi 3-kort</span><span class="sxs-lookup"><span data-stu-id="c6fa0-117">The Raspberry Pi 3 board</span></span>
* <span data-ttu-id="c6fa0-118">16 GB microSD-kort</span><span class="sxs-lookup"><span data-stu-id="c6fa0-118">The 16-GB microSD card</span></span>
* <span data-ttu-id="c6fa0-119">V 5-2-amp strömavbrott med 6 fotavtryck micro USB-kabel</span><span class="sxs-lookup"><span data-stu-id="c6fa0-119">The 5-volt 2-amp power supply with the 6-foot micro USB cable</span></span>
* <span data-ttu-id="c6fa0-120">Breadboard</span><span class="sxs-lookup"><span data-stu-id="c6fa0-120">The breadboard</span></span>
* <span data-ttu-id="c6fa0-121">Kopplingen kablar</span><span class="sxs-lookup"><span data-stu-id="c6fa0-121">Connector wires</span></span>
* <span data-ttu-id="c6fa0-122">En 560 ohm resistor</span><span class="sxs-lookup"><span data-stu-id="c6fa0-122">A 560-ohm resistor</span></span>
* <span data-ttu-id="c6fa0-123">En indirekt Indikator för 10 mm</span><span class="sxs-lookup"><span data-stu-id="c6fa0-123">A diffused 10-mm LED</span></span>
* <span data-ttu-id="c6fa0-124">Ethernet-kabel</span><span class="sxs-lookup"><span data-stu-id="c6fa0-124">The Ethernet cable</span></span>

![Saker i din startpaket](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

<span data-ttu-id="c6fa0-126">Du behöver också:</span><span class="sxs-lookup"><span data-stu-id="c6fa0-126">You also need:</span></span>

* <span data-ttu-id="c6fa0-127">En kabelansluten eller trådlös anslutning för Pi att ansluta till.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-127">A wired or wireless connection for Pi to connect to.</span></span>
* <span data-ttu-id="c6fa0-128">En USB-SD-kort eller mini-SD-kort att bränna OS-avbildningen till microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-128">A USB-SD adapter or mini-SD card to burn the OS image onto the microSD card.</span></span>
* <span data-ttu-id="c6fa0-129">En dator som kör Windows, Mac eller Linux.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-129">A computer running Windows, Mac, or Linux.</span></span> <span data-ttu-id="c6fa0-130">Datorn används för att installera Raspbian på microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-130">The computer is used to install Raspbian on the microSD card.</span></span>
* <span data-ttu-id="c6fa0-131">En Internet-anslutning att hämta verktyg och program.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-131">An Internet connection to download the necessary tools and software.</span></span>

## <a name="install-raspbian-on-the-microsd-card"></a><span data-ttu-id="c6fa0-132">Installera Raspbian på MicroSD-kort</span><span class="sxs-lookup"><span data-stu-id="c6fa0-132">Install Raspbian on the MicroSD card</span></span>
<span data-ttu-id="c6fa0-133">Förbered microSD-kort för installation av Raspbian bilden.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-133">Prepare the microSD card for installation of the Raspbian image.</span></span>

1. <span data-ttu-id="c6fa0-134">Hämta Raspbian.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-134">Download Raspbian.</span></span>
   1. <span data-ttu-id="c6fa0-135">[Hämta](https://www.raspberrypi.org/downloads/raspbian/) ZIP-filen för Raspbian Jessie med Pixel.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-135">[Download](https://www.raspberrypi.org/downloads/raspbian/) the .zip file for Raspbian Jessie with Pixel.</span></span>
   2. <span data-ttu-id="c6fa0-136">Extrahera Raspbian-avbildning till en mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-136">Extract the Raspbian image to a folder on your computer.</span></span>
2. <span data-ttu-id="c6fa0-137">Installera Raspbian microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-137">Install Raspbian to the microSD card.</span></span>
   1. <span data-ttu-id="c6fa0-138">[Hämta](https://www.etcher.io) och installera verktyget brännare Etcher SD-kort.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-138">[Download](https://www.etcher.io) and install the Etcher SD card burner utility.</span></span>
   2. <span data-ttu-id="c6fa0-139">Kör Etcher och välj Raspbian bilden som du extraherade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-139">Run Etcher and select the Raspbian image that you extracted in step 1.</span></span>
   3. <span data-ttu-id="c6fa0-140">Välj enhet för microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-140">Select the microSD card drive.</span></span>
      <span data-ttu-id="c6fa0-141">Observera att Etcher kanske redan har valt rätt enhet.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-141">Note that Etcher may have already selected the correct drive.</span></span>
   4. <span data-ttu-id="c6fa0-142">Klicka på **Flash** att installera Raspbian microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-142">Click **Flash** to install Raspbian to the microSD card.</span></span>
   5. <span data-ttu-id="c6fa0-143">Ta bort microSD-kort från datorn när installationen är klar.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-143">Remove the microSD card from your computer when installation is complete.</span></span>
      <span data-ttu-id="c6fa0-144">Det är säkert att ta bort microSD-kort direkt eftersom Etcher automatiskt matar ut eller demonterar microSD-kort när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-144">It is safe to remove the microSD card directly because Etcher automatically ejects or unmounts the microSD card upon completion.</span></span>
   6. <span data-ttu-id="c6fa0-145">Infoga microSD-kort i din Pi.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-145">Insert the microSD card into your Pi.</span></span>

![Infoga SD-kort](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="turn-on-pi"></a><span data-ttu-id="c6fa0-147">Aktivera Pi</span><span class="sxs-lookup"><span data-stu-id="c6fa0-147">Turn on Pi</span></span>
<span data-ttu-id="c6fa0-148">Aktivera Pi med hjälp av micro USB-kabel och strömförsörjningen.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-148">Turn on Pi by using the micro USB cable and the power supply.</span></span>

![Aktivera](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [!NOTE]
> <span data-ttu-id="c6fa0-150">Det är viktigt att använda strömförsörjningen i paketet som är minst 2A till kontrollerar du att din hallon har tillräckligt med ström för att fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-150">It is important to use the power supply in the kit that is at least 2A to make sure that your Raspberry has enough power to work correctly.</span></span>

## <a name="enable-ssh"></a><span data-ttu-id="c6fa0-151">Aktivera SSH</span><span class="sxs-lookup"><span data-stu-id="c6fa0-151">Enable SSH</span></span>
<span data-ttu-id="c6fa0-152">Från och med November 2016-versionen har Raspbian SSH-server som är inaktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-152">As of the November 2016 release, Raspbian has the SSH server disabled by default.</span></span> <span data-ttu-id="c6fa0-153">Du måste aktivera det manuellt.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-153">You need to enable it manually.</span></span> <span data-ttu-id="c6fa0-154">Du kan referera till den [officiella instruktioner](https://www.raspberrypi.org/documentation/remote-access/ssh/) eller ansluta en bildskärm och gå till **Inställningar -> hallon Pi Configuration** att aktivera SSH.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-154">You can refer to the [official instructions](https://www.raspberrypi.org/documentation/remote-access/ssh/) or connect a monitor and go to **Preferences -> Raspberry Pi Configuration** to enable SSH.</span></span>

## <a name="connect-raspberry-pi-3-to-the-network"></a><span data-ttu-id="c6fa0-155">Ansluta hallon Pi 3 till nätverket</span><span class="sxs-lookup"><span data-stu-id="c6fa0-155">Connect Raspberry Pi 3 to the network</span></span>
<span data-ttu-id="c6fa0-156">Du kan ansluta Pi till ett kabelanslutet nätverk eller till ett trådlöst nätverk.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-156">You can connect Pi to a wired network or to a wireless network.</span></span> <span data-ttu-id="c6fa0-157">Kontrollera att Pi är ansluten till samma nätverk som datorn.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-157">Make sure that Pi is connected to the same network as your computer.</span></span> <span data-ttu-id="c6fa0-158">Du kan till exempel ansluta Pi till samma växel som datorn är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-158">For example, you can connect Pi to the same switch that your computer is connected to.</span></span>

### <a name="connect-to-a-wired-network"></a><span data-ttu-id="c6fa0-159">Ansluta till ett kabelanslutet nätverk</span><span class="sxs-lookup"><span data-stu-id="c6fa0-159">Connect to a wired network</span></span>
<span data-ttu-id="c6fa0-160">Använda Ethernet-kabel för att ansluta Pi till det kabelanslutna nätverket.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-160">Use the Ethernet cable to connect Pi to your wired network.</span></span> <span data-ttu-id="c6fa0-161">Två indikatorer på Pi aktivera om anslutningen har upprättats.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-161">The two LEDs on Pi turn on if the connection is established.</span></span>

![Ansluta med hjälp av en Ethernet-kabel](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="connect-to-a-wireless-network"></a><span data-ttu-id="c6fa0-163">Ansluta till ett trådlöst nätverk</span><span class="sxs-lookup"><span data-stu-id="c6fa0-163">Connect to a wireless network</span></span>
<span data-ttu-id="c6fa0-164">Följ den [instruktioner](https://www.raspberrypi.org/learning/software-guide/wifi/) från hallon Pi Foundation ansluta Pi till det trådlösa nätverket.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-164">Follow the [instructions](https://www.raspberrypi.org/learning/software-guide/wifi/) from the Raspberry Pi Foundation to connect Pi to your wireless network.</span></span> <span data-ttu-id="c6fa0-165">Dessa anvisningar måste du först ansluta en bildskärm och ett tangentbord till Pi.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-165">These instructions require you to first connect a monitor and a keyboard to Pi.</span></span>

## <a name="connect-the-led-to-pi"></a><span data-ttu-id="c6fa0-166">Anslut Indikatorn till Pi</span><span class="sxs-lookup"><span data-stu-id="c6fa0-166">Connect the LED to Pi</span></span>
<span data-ttu-id="c6fa0-167">Slutför aktiviteten genom att använda den [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), connector-kablar och Indikatorn på resistor.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-167">To complete this task, use the [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), the connector wires, the LED, and the resistor.</span></span> <span data-ttu-id="c6fa0-168">Anslut dem till den [allmänna o](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO)-portar för Pi.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-168">Connect them to the [general-purpose input/output](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO) ports of Pi.</span></span>

![Breadboard Indikator och Resistor](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. <span data-ttu-id="c6fa0-170">Ansluta kortare del av Indikator för **GPIO GND (PIN-kod 6)**.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-170">Connect the shorter leg of the LED to **GPIO GND (Pin 6)**.</span></span>
2. <span data-ttu-id="c6fa0-171">Längre del av Indikatorn för att ansluta till en del av resistor.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-171">Connect the longer leg of the LED to one leg of the resistor.</span></span>
3. <span data-ttu-id="c6fa0-172">Ansluta andra del av resistor till **GPIO 4.7 PIN-kod**.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-172">Connect the other leg of the resistor to **GPIO 4 (Pin 7)**.</span></span>

<span data-ttu-id="c6fa0-173">Observera att Indikator polaritet är viktigt.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-173">Note that the LED polarity is important.</span></span> <span data-ttu-id="c6fa0-174">Den här inställningen polaritet kallas brukar aktivt lågt.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-174">This polarity setting is commonly known as Active Low.</span></span>

![Pinout](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

<span data-ttu-id="c6fa0-176">Grattis!</span><span class="sxs-lookup"><span data-stu-id="c6fa0-176">Congratulations!</span></span> <span data-ttu-id="c6fa0-177">Du har konfigurerat Pi.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-177">You've successfully configured Pi.</span></span>

## <a name="summary"></a><span data-ttu-id="c6fa0-178">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c6fa0-178">Summary</span></span>
<span data-ttu-id="c6fa0-179">I den här artikeln har du lärt dig hur du konfigurerar Pi genom att installera Raspbian, ansluta Pi till ett nätverk och ansluter en Indikator till Pi.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-179">In this article, you’ve learned how to configure Pi by installing Raspbian, connecting Pi to a network, and connecting an LED to Pi.</span></span> <span data-ttu-id="c6fa0-180">Observera att Indikatorn ännu inte lysa upp.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-180">Note that the LED doesn't yet light up.</span></span> <span data-ttu-id="c6fa0-181">Nästa uppgift är att installera verktyg och program som förberedelse för att köra ett exempelprogram på Pi.</span><span class="sxs-lookup"><span data-stu-id="c6fa0-181">The next task is to install the necessary tools and software in preparation for running a sample application on Pi.</span></span>

![Maskinvara är klar](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a><span data-ttu-id="c6fa0-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c6fa0-183">Next steps</span></span>
[<span data-ttu-id="c6fa0-184">Skaffa dig verktyg</span><span class="sxs-lookup"><span data-stu-id="c6fa0-184">Get the tools</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

