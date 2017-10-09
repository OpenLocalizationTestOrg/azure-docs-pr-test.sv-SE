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
# <a name="configure-your-device"></a>Konfigurera din enhet
## <a name="what-you-will-do"></a>Vad du ska göra
Konfigurera Pi för första gången och installera hello Raspbian operativsystem. Raspbian är ett kostnadsfritt operativsystem som är optimerad för hello hallon Pi maskinvara. Om du har några problem du söker efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:

* Hur tooinstall Raspbian på Pi.
* Hur toopower in Pi med hjälp av en USB-kabel.
* Hur tooconnect Pi toohello nätverk med hjälp av en Ethernet-kabel eller trådlöst nätverk.
* Hur tooadd en Indikator toohello breadboard och anslut den tooPi.

## <a name="what-you-will-need"></a>Vad du behöver
toocomplete den här åtgärden måste hello följande delar från din startpaket för hallon Pi 3:

* hello hallon Pi 3-kort
* hello 16 GB microSD-kort
* hello v 5-2-amp strömförsörjning med hello 6 fotavtryck micro USB-kabel
* Hej breadboard
* Kopplingen kablar
* En 560 ohm resistor
* En indirekt Indikator för 10 mm
* hello Ethernet-kabel

![Saker i din startpaket](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Du behöver också:

* En kabelansluten eller trådlös anslutning för Pi tooconnect till.
* En USB-SD-kort eller miniSD kort tooburn hello operativsystemavbildning på hello microSD-kort.
* En dator som kör Windows, Mac eller Linux. hello datorn är används tooinstall Raspbian på hello microSD-kort.
* En Internet-anslutning toodownload hello nödvändiga verktyg och program.

## <a name="install-raspbian-on-hello-microsd-card"></a>Installera Raspbian på hello microSD-kort
Förbered hello microSD-kort för installation av hello Raspbian bild.

1. Hämta Raspbian.
   1. [Hämta](https://www.raspberrypi.org/downloads/raspbian/) hello ZIP-filen för Raspbian Jessie med Pixel.
   2. Extrahera hello Raspbian bild tooa mapp på datorn.
2. Installera Raspbian toohello microSD-kort.
   1. [Hämta](https://www.etcher.io) och installera hello Etcher SD-kort brännare verktyget.
   2. Kör Etcher och välj hello Raspbian avbildning som du extraherade i steg 1.
   3. Välj enhet för hello microSD-kort.
      Observera att Etcher kanske redan har valt hello rätt enhet.
   4. Klicka på **Flash** tooinstall Raspbian toohello microSD-kort.
   5. Ta bort hello microSD-kort från datorn när installationen är klar.
      Det är säker tooremove hello microSD-kort direkt eftersom Etcher automatiskt matar ut eller demonterar hello microSD-kort när åtgärden har slutförts.
   6. Infoga hello microSD-kort i Pi.

![Infoga hello SD-kort](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="turn-on-pi"></a>Aktivera Pi
Aktivera Pi med hjälp av hello micro USB-kabel och hello strömförsörjning.

![Aktivera](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [!NOTE]
> Det är viktigt toouse hello strömförsörjning i hello kit som är minst 2A toomake till att din hallon har tillräckligt med power toowork korrekt.

## <a name="enable-ssh"></a>Aktivera SSH
Från och med hello November 2016-versionen har Raspbian hello SSH-server inaktiverad som standard. Du behöver tooenable den manuellt. Du kan se toohello [officiella instruktioner](https://www.raspberrypi.org/documentation/remote-access/ssh/) eller ansluta en bildskärm och gå för**Inställningar -> hallon Pi Configuration** tooenable SSH.

## <a name="connect-raspberry-pi-3-toohello-network"></a>Ansluta hallon Pi 3 toohello nätverk
Du kan ansluta Pi tooa kabelanslutna nätverk eller tooa trådlösa nätverk. Se till att Pi är anslutna toohello samma nätverk som datorn. Du kan till exempel ansluta Pi toohello samma växel att datorn är ansluten till.

### <a name="connect-tooa-wired-network"></a>Ansluta tooa kabelanslutet nätverk
Använd hello Ethernet-kabel tooconnect Pi tooyour kabelanslutna nätverket. hello aktivera två indikatorer på Pi om hello anslutningen har upprättats.

![Ansluta med hjälp av en Ethernet-kabel](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="connect-tooa-wireless-network"></a>Ansluta tooa trådlöst nätverk
Följ hello [instruktioner](https://www.raspberrypi.org/learning/software-guide/wifi/) från hello hallon Pi Foundation tooconnect Pi tooyour trådlöst nätverk. Dessa instruktioner kräver att du toofirst ansluta en bildskärm och ett tangentbord tooPi.

## <a name="connect-hello-led-toopi"></a>Ansluta hello Indikator tooPi
toocomplete den här uppgiften, Använd hello [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), hello connector kablar, hello Indikator och hello resistor. Anslut dem toohello [allmänna o](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO)-portar för Pi.

![Breadboard Indikator och Resistor](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Ansluta hello kortare ben hello Indikator för**GPIO GND (PIN-kod 6)**.
2. Ansluta hello längre del av hello Indikator tooone ben hello resistor.
3. Ansluta hello andra del av hello resistor för**GPIO 4.7 PIN-kod**.

Observera att hello Indikator polaritet är viktigt. Den här inställningen polaritet kallas brukar aktivt lågt.

![Pinout](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Grattis! Du har konfigurerat Pi.

## <a name="summary"></a>Sammanfattning
I den här artikeln har du lärt dig hur tooconfigure Pi genom att installera Raspbian anslutande Pi tooa nätverk och ansluter en Indikator tooPi. Observera att hello Indikator ännu inte lysa upp. hello nästa uppgift är tooinstall hello nödvändiga verktyg och program som förberedelse för att köra ett exempelprogram på Pi.

![Maskinvara är klar](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Nästa steg
[Hämta hello-verktyg](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

