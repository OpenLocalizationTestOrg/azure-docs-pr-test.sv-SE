---
title: 'Ansluta hallon Pi (nod) till Azure IoT - lektionen 1: Konfigurera enhet | Microsoft Docs'
description: Configure Raspberry Pi 3 for first-time use and install the Raspbian OS, a free operating system that is optimized for the Raspberry Pi hardware.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "installera raspbian, raspbian download hur pi ansluta för att installera raspbian, raspbian inställningar, raspberry pi installera raspbian, raspberry pi installera os, raspberry pi sd-kort installera, hallon, ansluta till raspberry pi, raspberry pi-anslutning"
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
ms.openlocfilehash: b848c48157a2310f0eb1d6398f8b9aaa4395d47f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-device"></a>Konfigurera din enhet
## <a name="what-you-will-do"></a>Vad du ska göra
Konfigurera Pi för första gången och installera operativsystemet Raspbian. Raspbian är ett kostnadsfritt operativsystem som är optimerad för hallon Pi-maskinvara. Om du har några problem du söker efter lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:

* Hur du installerar Raspbian på Pi.
* Så här uppstart Pi med hjälp av en USB-kabel.
* Hur du ansluter Pi till nätverket med hjälp av en Ethernet-kabel eller trådlöst nätverk.
* Så här lägger du till en Indikator på breadboard och ansluta till Pi.

## <a name="what-you-will-need"></a>Vad du behöver
För att slutföra den här åtgärden behöver du följande delar från din startpaket för hallon Pi 3:

* Hallon Pi 3-kort
* 16 GB microSD-kort
* V 5-2-amp strömavbrott med 6 fotavtryck micro USB-kabel
* Breadboard
* Kopplingen kablar
* En 560 ohm resistor
* En indirekt Indikator för 10 mm
* Ethernet-kabel

![Saker i din startpaket](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Du behöver också:

* En kabelansluten eller trådlös anslutning för Pi att ansluta till.
* Ett USB-SD-kort eller miniSD kort att bränna operativsystemavbildning på microSD-kort.
* En dator som kör Windows, Mac eller Linux. Datorn används för att installera Raspbian på microSD-kort.
* En Internet-anslutning att hämta verktyg och program.

## <a name="install-raspbian-on-the-microsd-card"></a>Installera Raspbian på microSD-kort
Förbered microSD-kort för installation av Raspbian bilden.

1. Hämta Raspbian.
   1. [Hämta](https://www.raspberrypi.org/downloads/raspbian/) ZIP-filen för Raspbian Jessie med Pixel.
   2. Extrahera Raspbian-avbildning till en mapp på datorn.
2. Installera Raspbian microSD-kort.
   1. [Hämta](https://www.etcher.io) och installera verktyget brännare Etcher SD-kort.
   2. Kör Etcher och välj Raspbian bilden som du extraherade i steg 1.
   3. Välj enhet för microSD-kort.
      Observera att Etcher kanske redan har valt rätt enhet.
   4. Klicka på **Flash** att installera Raspbian microSD-kort.
   5. Ta bort microSD-kort från datorn när installationen är klar.
      Det är säkert att ta bort microSD-kort direkt eftersom Etcher automatiskt matar ut eller demonterar microSD-kort när åtgärden har slutförts.
   6. Infoga microSD-kort i Pi.

![Infoga SD-kort](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="turn-on-pi"></a>Aktivera Pi
Aktivera Pi med hjälp av micro USB-kabel och strömförsörjningen.

![Aktivera](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [!NOTE]
> Det är viktigt att använda strömförsörjningen i paketet som är minst 2A till kontrollerar du att din hallon har tillräckligt med ström för att fungera korrekt.

## <a name="enable-ssh"></a>Aktivera SSH
Från och med November 2016-versionen har Raspbian SSH-server som är inaktiverad som standard. Du måste aktivera det manuellt. Du kan referera till den [officiella instruktioner](https://www.raspberrypi.org/documentation/remote-access/ssh/) eller ansluta en bildskärm och gå till **Inställningar -> hallon Pi Configuration** att aktivera SSH.

## <a name="connect-raspberry-pi-3-to-the-network"></a>Ansluta hallon Pi 3 till nätverket
Du kan ansluta Pi till ett kabelanslutet nätverk eller till ett trådlöst nätverk. Kontrollera att Pi är ansluten till samma nätverk som datorn. Du kan till exempel ansluta Pi till samma växel som datorn är ansluten till.

### <a name="connect-to-a-wired-network"></a>Ansluta till ett kabelanslutet nätverk
Använda Ethernet-kabel för att ansluta Pi till det kabelanslutna nätverket. Två indikatorer på Pi aktivera om anslutningen har upprättats.

![Ansluta med hjälp av en Ethernet-kabel](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="connect-to-a-wireless-network"></a>Ansluta till ett trådlöst nätverk
Följ den [instruktioner](https://www.raspberrypi.org/learning/software-guide/wifi/) från hallon Pi Foundation ansluta Pi till det trådlösa nätverket. Dessa anvisningar måste du först ansluta en bildskärm och ett tangentbord till Pi.

## <a name="connect-the-led-to-pi"></a>Anslut Indikatorn till Pi
Slutför aktiviteten genom att använda den [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), connector-kablar och Indikatorn på resistor. Anslut dem till den [allmänna o](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO)-portar för Pi.

![Breadboard Indikator och Resistor](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Ansluta kortare del av Indikator för **GPIO GND (PIN-kod 6)**.
2. Längre del av Indikatorn för att ansluta till en del av resistor.
3. Ansluta andra del av resistor till **GPIO 4.7 PIN-kod**.

Observera att Indikator polaritet är viktigt. Den här inställningen polaritet kallas brukar aktivt lågt.

![Pinout](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Grattis! Du har konfigurerat Pi.

## <a name="summary"></a>Sammanfattning
I den här artikeln har du lärt dig hur du konfigurerar Pi genom att installera Raspbian, ansluta Pi till ett nätverk och ansluter en Indikator till Pi. Observera att Indikatorn ännu inte lysa upp. Nästa uppgift är att installera verktyg och program som förberedelse för att köra ett exempelprogram på Pi.

![Maskinvara är klar](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Nästa steg
[Skaffa dig verktyg](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

