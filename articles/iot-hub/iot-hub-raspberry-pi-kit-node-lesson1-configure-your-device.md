---
title: 'Ansluta hallon Pi (nod) tooAzure IoT - lektionen 1: Konfigurera enhet | Microsoft Docs'
description: "Konfigurera hallon Pi 3 för första gången och installera hello Raspbian OS, ett ledigt operativsystem som är optimerad för hello hallon Pi maskinvara."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: installera raspbian, raspbian download hur tooinstall raspbian raspbian installationen raspberry pi installera raspbian, raspberry pi installera os, raspberry pi sd-kort installera, raspberry pi ansluta, ansluta tooraspberry pi, raspberry pi anslutning
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 43f7c2cf-f1a5-4dd5-93f0-7e546c6dc91e
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 504a4d2a3f29717f955530812442cce2a78a6448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-device"></a><span data-ttu-id="f67da-104">Konfigurera din enhet</span><span class="sxs-lookup"><span data-stu-id="f67da-104">Configure your device</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="f67da-105">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="f67da-105">What you will do</span></span>
<span data-ttu-id="f67da-106">Konfigurera Pi för första gången och installera hello Raspbian operativsystem.</span><span class="sxs-lookup"><span data-stu-id="f67da-106">Configure Pi for first-time use and install hello Raspbian operating system.</span></span> <span data-ttu-id="f67da-107">Raspbian är ett kostnadsfritt operativsystem som är optimerad för hello hallon Pi maskinvara.</span><span class="sxs-lookup"><span data-stu-id="f67da-107">Raspbian is a free operating system that is optimized for hello Raspberry Pi hardware.</span></span> <span data-ttu-id="f67da-108">Om du har några problem du söker efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f67da-108">If you have any problems, you can seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f67da-109">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="f67da-109">What you will learn</span></span>
<span data-ttu-id="f67da-110">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="f67da-110">In this article, you will learn:</span></span>

* <span data-ttu-id="f67da-111">Hur tooinstall Raspbian på Pi.</span><span class="sxs-lookup"><span data-stu-id="f67da-111">How tooinstall Raspbian on Pi.</span></span>
* <span data-ttu-id="f67da-112">Hur toopower in Pi med hjälp av en USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="f67da-112">How toopower up Pi by using a USB cable.</span></span>
* <span data-ttu-id="f67da-113">Hur tooconnect Pi toohello nätverk med hjälp av en Ethernet-kabel eller trådlöst nätverk.</span><span class="sxs-lookup"><span data-stu-id="f67da-113">How tooconnect Pi toohello network by using an Ethernet cable or wireless network.</span></span>
* <span data-ttu-id="f67da-114">Hur tooadd en Indikator toohello breadboard och anslut den tooPi.</span><span class="sxs-lookup"><span data-stu-id="f67da-114">How tooadd an LED toohello breadboard and connect it tooPi.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="f67da-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="f67da-115">What you will need</span></span>
<span data-ttu-id="f67da-116">toocomplete den här åtgärden måste hello följande delar från din startpaket för hallon Pi 3:</span><span class="sxs-lookup"><span data-stu-id="f67da-116">toocomplete this operation, you need hello following parts from your Raspberry Pi 3 Starter Kit:</span></span>

* <span data-ttu-id="f67da-117">hello hallon Pi 3-kort</span><span class="sxs-lookup"><span data-stu-id="f67da-117">hello Raspberry Pi 3 board</span></span>
* <span data-ttu-id="f67da-118">hello 16 GB microSD-kort</span><span class="sxs-lookup"><span data-stu-id="f67da-118">hello 16-GB microSD card</span></span>
* <span data-ttu-id="f67da-119">hello v 5-2-amp strömförsörjning med hello 6 fotavtryck micro USB-kabel</span><span class="sxs-lookup"><span data-stu-id="f67da-119">hello 5-volt 2-amp power supply with hello 6-foot micro USB cable</span></span>
* <span data-ttu-id="f67da-120">Hej breadboard</span><span class="sxs-lookup"><span data-stu-id="f67da-120">hello breadboard</span></span>
* <span data-ttu-id="f67da-121">Kopplingen kablar</span><span class="sxs-lookup"><span data-stu-id="f67da-121">Connector wires</span></span>
* <span data-ttu-id="f67da-122">En 560 ohm resistor</span><span class="sxs-lookup"><span data-stu-id="f67da-122">A 560-ohm resistor</span></span>
* <span data-ttu-id="f67da-123">En indirekt Indikator för 10 mm</span><span class="sxs-lookup"><span data-stu-id="f67da-123">A diffused 10-mm LED</span></span>
* <span data-ttu-id="f67da-124">hello Ethernet-kabel</span><span class="sxs-lookup"><span data-stu-id="f67da-124">hello Ethernet cable</span></span>

![Saker i din startpaket](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

<span data-ttu-id="f67da-126">Du behöver också:</span><span class="sxs-lookup"><span data-stu-id="f67da-126">You also need:</span></span>

* <span data-ttu-id="f67da-127">En kabelansluten eller trådlös anslutning för Pi tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="f67da-127">A wired or wireless connection for Pi tooconnect to.</span></span>
* <span data-ttu-id="f67da-128">En USB-SD-kort eller miniSD kort tooburn hello operativsystemavbildning på hello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="f67da-128">A USB-SD adapter or miniSD card tooburn hello operating system image onto hello microSD card.</span></span>
* <span data-ttu-id="f67da-129">En dator som kör Windows, Mac eller Linux.</span><span class="sxs-lookup"><span data-stu-id="f67da-129">A computer running Windows, Mac, or Linux.</span></span> <span data-ttu-id="f67da-130">hello datorn är används tooinstall Raspbian på hello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="f67da-130">hello computer is used tooinstall Raspbian on hello microSD card.</span></span>
* <span data-ttu-id="f67da-131">En Internet-anslutning toodownload hello nödvändiga verktyg och program.</span><span class="sxs-lookup"><span data-stu-id="f67da-131">An Internet connection toodownload hello necessary tools and software.</span></span>

## <a name="install-raspbian-on-hello-microsd-card"></a><span data-ttu-id="f67da-132">Installera Raspbian på hello microSD-kort</span><span class="sxs-lookup"><span data-stu-id="f67da-132">Install Raspbian on hello microSD card</span></span>
<span data-ttu-id="f67da-133">Förbered hello microSD-kort för installation av hello Raspbian bild.</span><span class="sxs-lookup"><span data-stu-id="f67da-133">Prepare hello microSD card for installation of hello Raspbian image.</span></span>

1. <span data-ttu-id="f67da-134">Hämta Raspbian.</span><span class="sxs-lookup"><span data-stu-id="f67da-134">Download Raspbian.</span></span>
   1. <span data-ttu-id="f67da-135">[Hämta](https://www.raspberrypi.org/downloads/raspbian/) hello ZIP-filen för Raspbian Jessie med Pixel.</span><span class="sxs-lookup"><span data-stu-id="f67da-135">[Download](https://www.raspberrypi.org/downloads/raspbian/) hello .zip file for Raspbian Jessie with Pixel.</span></span>
   2. <span data-ttu-id="f67da-136">Extrahera hello Raspbian bild tooa mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="f67da-136">Extract hello Raspbian image tooa folder on your computer.</span></span>
2. <span data-ttu-id="f67da-137">Installera Raspbian toohello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="f67da-137">Install Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="f67da-138">[Hämta](https://www.etcher.io) och installera hello Etcher SD-kort brännare verktyget.</span><span class="sxs-lookup"><span data-stu-id="f67da-138">[Download](https://www.etcher.io) and install hello Etcher SD card burner utility.</span></span>
   2. <span data-ttu-id="f67da-139">Kör Etcher och välj hello Raspbian avbildning som du extraherade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="f67da-139">Run Etcher and select hello Raspbian image that you extracted in step 1.</span></span>
   3. <span data-ttu-id="f67da-140">Välj enhet för hello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="f67da-140">Select hello microSD card drive.</span></span>
      <span data-ttu-id="f67da-141">Observera att Etcher kanske redan har valt hello rätt enhet.</span><span class="sxs-lookup"><span data-stu-id="f67da-141">Note that Etcher may have already selected hello correct drive.</span></span>
   4. <span data-ttu-id="f67da-142">Klicka på **Flash** tooinstall Raspbian toohello microSD-kort.</span><span class="sxs-lookup"><span data-stu-id="f67da-142">Click **Flash** tooinstall Raspbian toohello microSD card.</span></span>
   5. <span data-ttu-id="f67da-143">Ta bort hello microSD-kort från datorn när installationen är klar.</span><span class="sxs-lookup"><span data-stu-id="f67da-143">Remove hello microSD card from your computer when installation is complete.</span></span>
      <span data-ttu-id="f67da-144">Det är säker tooremove hello microSD-kort direkt eftersom Etcher automatiskt matar ut eller demonterar hello microSD-kort när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="f67da-144">It's safe tooremove hello microSD card directly because Etcher automatically ejects or unmounts hello microSD card upon completion.</span></span>
   6. <span data-ttu-id="f67da-145">Infoga hello microSD-kort i Pi.</span><span class="sxs-lookup"><span data-stu-id="f67da-145">Insert hello microSD card into Pi.</span></span>

![Infoga hello SD-kort](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="turn-on-pi"></a><span data-ttu-id="f67da-147">Aktivera Pi</span><span class="sxs-lookup"><span data-stu-id="f67da-147">Turn on Pi</span></span>
<span data-ttu-id="f67da-148">Aktivera Pi med hjälp av hello micro USB-kabel och hello strömförsörjning.</span><span class="sxs-lookup"><span data-stu-id="f67da-148">Turn on Pi by using hello micro USB cable and hello power supply.</span></span>

![Aktivera](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [!NOTE]
> <span data-ttu-id="f67da-150">Det är viktigt toouse hello strömförsörjning i hello kit som är minst 2A toomake till att din hallon har tillräckligt med power toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="f67da-150">It is important toouse hello power supply in hello kit that is at least 2A toomake sure that your Raspberry has enough power toowork correctly.</span></span>

## <a name="enable-ssh"></a><span data-ttu-id="f67da-151">Aktivera SSH</span><span class="sxs-lookup"><span data-stu-id="f67da-151">Enable SSH</span></span>
<span data-ttu-id="f67da-152">Från och med hello November 2016-versionen har Raspbian hello SSH-server inaktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="f67da-152">As of hello November 2016 release, Raspbian has hello SSH server disabled by default.</span></span> <span data-ttu-id="f67da-153">Du behöver tooenable den manuellt.</span><span class="sxs-lookup"><span data-stu-id="f67da-153">You need tooenable it manually.</span></span> <span data-ttu-id="f67da-154">Du kan se toohello [officiella instruktioner](https://www.raspberrypi.org/documentation/remote-access/ssh/) eller ansluta en bildskärm och gå för**Inställningar -> hallon Pi Configuration** tooenable SSH.</span><span class="sxs-lookup"><span data-stu-id="f67da-154">You can refer toohello [official instructions](https://www.raspberrypi.org/documentation/remote-access/ssh/) or connect a monitor and go too**Preferences -> Raspberry Pi Configuration** tooenable SSH.</span></span>

## <a name="connect-raspberry-pi-3-toohello-network"></a><span data-ttu-id="f67da-155">Ansluta hallon Pi 3 toohello nätverk</span><span class="sxs-lookup"><span data-stu-id="f67da-155">Connect Raspberry Pi 3 toohello network</span></span>
<span data-ttu-id="f67da-156">Du kan ansluta Pi tooa kabelanslutna nätverk eller tooa trådlösa nätverk.</span><span class="sxs-lookup"><span data-stu-id="f67da-156">You can connect Pi tooa wired network or tooa wireless network.</span></span> <span data-ttu-id="f67da-157">Se till att Pi är anslutna toohello samma nätverk som datorn.</span><span class="sxs-lookup"><span data-stu-id="f67da-157">Make sure that Pi is connected toohello same network as your computer.</span></span> <span data-ttu-id="f67da-158">Du kan till exempel ansluta Pi toohello samma växel att datorn är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="f67da-158">For example, you can connect Pi toohello same switch that your computer is connected to.</span></span>

### <a name="connect-tooa-wired-network"></a><span data-ttu-id="f67da-159">Ansluta tooa kabelanslutet nätverk</span><span class="sxs-lookup"><span data-stu-id="f67da-159">Connect tooa wired network</span></span>
<span data-ttu-id="f67da-160">Använd hello Ethernet-kabel tooconnect Pi tooyour kabelanslutna nätverket.</span><span class="sxs-lookup"><span data-stu-id="f67da-160">Use hello Ethernet cable tooconnect Pi tooyour wired network.</span></span> <span data-ttu-id="f67da-161">hello aktivera två indikatorer på Pi om hello anslutningen har upprättats.</span><span class="sxs-lookup"><span data-stu-id="f67da-161">hello two LEDs on Pi turn on if hello connection is established.</span></span>

![Ansluta med hjälp av en Ethernet-kabel](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="connect-tooa-wireless-network"></a><span data-ttu-id="f67da-163">Ansluta tooa trådlöst nätverk</span><span class="sxs-lookup"><span data-stu-id="f67da-163">Connect tooa wireless network</span></span>
<span data-ttu-id="f67da-164">Följ hello [instruktioner](https://www.raspberrypi.org/learning/software-guide/wifi/) från hello hallon Pi Foundation tooconnect Pi tooyour trådlöst nätverk.</span><span class="sxs-lookup"><span data-stu-id="f67da-164">Follow hello [instructions](https://www.raspberrypi.org/learning/software-guide/wifi/) from hello Raspberry Pi Foundation tooconnect Pi tooyour wireless network.</span></span> <span data-ttu-id="f67da-165">Dessa instruktioner kräver att du toofirst ansluta en bildskärm och ett tangentbord tooPi.</span><span class="sxs-lookup"><span data-stu-id="f67da-165">These instructions require you toofirst connect a monitor and a keyboard tooPi.</span></span>

## <a name="connect-hello-led-toopi"></a><span data-ttu-id="f67da-166">Ansluta hello Indikator tooPi</span><span class="sxs-lookup"><span data-stu-id="f67da-166">Connect hello LED tooPi</span></span>
<span data-ttu-id="f67da-167">toocomplete den här uppgiften, Använd hello [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), hello connector kablar, hello Indikator och hello resistor.</span><span class="sxs-lookup"><span data-stu-id="f67da-167">toocomplete this task, use hello [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), hello connector wires, hello LED, and hello resistor.</span></span> <span data-ttu-id="f67da-168">Anslut dem toohello [allmänna o](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO)-portar för Pi.</span><span class="sxs-lookup"><span data-stu-id="f67da-168">Connect them toohello [general-purpose input/output](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO) ports of Pi.</span></span>

![Breadboard Indikator och Resistor](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. <span data-ttu-id="f67da-170">Ansluta hello kortare ben hello Indikator för**GPIO GND (PIN-kod 6)**.</span><span class="sxs-lookup"><span data-stu-id="f67da-170">Connect hello shorter leg of hello LED too**GPIO GND (Pin 6)**.</span></span>
2. <span data-ttu-id="f67da-171">Ansluta hello längre del av hello Indikator tooone ben hello resistor.</span><span class="sxs-lookup"><span data-stu-id="f67da-171">Connect hello longer leg of hello LED tooone leg of hello resistor.</span></span>
3. <span data-ttu-id="f67da-172">Ansluta hello andra del av hello resistor för**GPIO 4.7 PIN-kod**.</span><span class="sxs-lookup"><span data-stu-id="f67da-172">Connect hello other leg of hello resistor too**GPIO 4 (Pin 7)**.</span></span>

<span data-ttu-id="f67da-173">Observera att hello Indikator polaritet är viktigt.</span><span class="sxs-lookup"><span data-stu-id="f67da-173">Note that hello LED polarity is important.</span></span> <span data-ttu-id="f67da-174">Den här inställningen polaritet kallas brukar aktivt lågt.</span><span class="sxs-lookup"><span data-stu-id="f67da-174">This polarity setting is commonly known as Active Low.</span></span>

![Pinout](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

<span data-ttu-id="f67da-176">Grattis!</span><span class="sxs-lookup"><span data-stu-id="f67da-176">Congratulations!</span></span> <span data-ttu-id="f67da-177">Du har konfigurerat Pi.</span><span class="sxs-lookup"><span data-stu-id="f67da-177">You've successfully configured Pi.</span></span>

## <a name="summary"></a><span data-ttu-id="f67da-178">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f67da-178">Summary</span></span>
<span data-ttu-id="f67da-179">I den här artikeln har du lärt dig hur tooconfigure Pi genom att installera Raspbian anslutande Pi tooa nätverk och ansluter en Indikator tooPi.</span><span class="sxs-lookup"><span data-stu-id="f67da-179">In this article, you’ve learned how tooconfigure Pi by installing Raspbian, connecting Pi tooa network, and connecting an LED tooPi.</span></span> <span data-ttu-id="f67da-180">Observera att hello Indikator ännu inte lysa upp.</span><span class="sxs-lookup"><span data-stu-id="f67da-180">Note that hello LED doesn't yet light up.</span></span> <span data-ttu-id="f67da-181">hello nästa uppgift är tooinstall hello nödvändiga verktyg och program som förberedelse för att köra ett exempelprogram på Pi.</span><span class="sxs-lookup"><span data-stu-id="f67da-181">hello next task is tooinstall hello necessary tools and software in preparation for running a sample application on Pi.</span></span>

![Maskinvara är klar](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a><span data-ttu-id="f67da-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f67da-183">Next steps</span></span>
[<span data-ttu-id="f67da-184">Hämta hello-verktyg</span><span class="sxs-lookup"><span data-stu-id="f67da-184">Get hello tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

