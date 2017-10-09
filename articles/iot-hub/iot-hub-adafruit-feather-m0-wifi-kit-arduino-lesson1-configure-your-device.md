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
# <a name="configure-your-device"></a>Konfigurera din enhet
## <a name="what-you-will-do"></a>Vad du ska göra
Konfigurera Adafruit ludd M0 WiFi Arduino-kort för första gången som samlar in hello ändringar, startar upp. Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).

## <a name="what-you-need"></a>Vad du behöver
toocomplete den här åtgärden måste hello följande delar för din startpaket för Adafruit ludd M0 Wi-Fi:

* Hej Adafruit ludd M0 WiFi-kort
* Micro B-tooType en USB-kabel

![Kit][kit]

Du behöver också:

* En dator som kör Windows, Mac eller Linux.
* En trådlös anslutning för din Arduino board tooconnect till.
* En Internet-anslutning toodownload-konfigurationsverktyget.

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:

* Hur tooassemble din Arduino kort och power den för hello följande lektioner.
* Hur tooadd serieport behörigheter på Ubuntu.

## <a name="connect-your-arduino-board-tooyour-computer"></a>Anslut datorn Arduino board tooyour

1. Anslut hello micro USB-kabel till hello översta micro USB-port.

   ![Övre micro USB-port][top-micro-usb-port]

2. Plug hello andra änden av USB-kabel till datorn.

   ![Datorn USB][computer-usb]

## <a name="add-serial-port-permissions-on-ubuntu"></a>Lägg till behörigheter för seriell port på Ubuntu

Du kan hoppa över det här avsnittet om du använder Windows eller macOS. För Ubuntu behöver du följande steg toomake till hello normal linux användaren har hello behörigheter toooperate på hello USB-port för Arduino-kortet hello.

1. Nu som normal användare från terminalen:

   ```bash
   ls -l /dev/ttyUSB*
   # Or
   ls -l /dev/ttyACM*
   ```

   Du får något som liknar:

   ```bash
   crw-rw---- 1 root uucp 188, 0 5 apr 23.01 ttyUSB0
   # Or
   crw-rw---- 1 root dialout 188, 0 5 apr 23.01 ttyACM0
   ```

   Hej ”0” kan vara ett annat nummer eller flera poster kan returneras. I hello första case hello data som vi behöver `uucp`, i hello andra är `dialout`, vilket är hello gruppägare hello-filen.

2. Lägg till användargrupp toohello toohello:

   ```bash
   sudo usermod -a -G group-name username
   ```

   Där `group-name` är hello data hittades i hello första steget, och `username` är ditt linux-användarnamn.

3. Du behöver toolog ut och in igen för den här ändringen tootake effekt och fullständig hello inställningar.

## <a name="summary"></a>Sammanfattning
I den här artikeln har du lärt dig hur tooconfigure Arduino-kort. hello nästa uppgift är tooinstall hello nödvändiga verktyg och program som förberedelse för att köra ett exempelprogram på Arduino-kort.

![Maskinvara är klar][hardware-is-ready]

## <a name="next-steps"></a>Nästa steg
[Hämta hello-verktyg][get-the-tools]
<!-- Images and links -->

[kit]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/kit.png
[top-micro-usb-port]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/top_usbport.jpg
[computer-usb]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/computer_usb.jpg
[hardware-is-ready]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/hardware_ready.jpg
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md