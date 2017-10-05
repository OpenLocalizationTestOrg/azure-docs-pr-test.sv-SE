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
# <a name="configure-your-device"></a>Konfigurera din enhet
## <a name="what-you-will-do"></a>Vad du ska göra
Konfigurera din Adafruit ludd M0 WiFi Arduino board för första gången som samlar in ändringar, startar upp. Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).

## <a name="what-you-need"></a>Vad du behöver
För att slutföra den här åtgärden behöver du följande delar för din startpaket för Adafruit ludd M0 Wi-Fi:

* Adafruit ludd M0 WiFi-kort
* En Micro B till typ A USB-kabel

![Kit][kit]

Du behöver också:

* En dator som kör Windows, Mac eller Linux.
* En trådlös anslutning för Arduino-kort att ansluta till.
* En Internet-anslutning att hämta verktyget för serverkonfiguration.

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:

* Så här montera Arduino-kort och slår upp för följande erfarenheter.
* Hur du lägger till seriell port behörigheter på Ubuntu.

## <a name="connect-your-arduino-board-to-your-computer"></a>Ansluta Arduino-kort till din dator

1. Anslut micro USB-kabel till översta micro USB-port.

   ![Övre micro USB-port][top-micro-usb-port]

2. Anslut den andra änden av USB-kabel till din dator.

   ![Datorn USB][computer-usb]

## <a name="add-serial-port-permissions-on-ubuntu"></a>Lägg till behörigheter för seriell port på Ubuntu

Du kan hoppa över det här avsnittet om du använder Windows eller macOS. För Ubuntu behöver du följande steg för att kontrollera att den normala linux-användaren har behörighet att använda USB-port för Arduino-kortet.

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

   ”0” kan vara ett annat nummer eller flera poster kan returneras. I det första fallet är de data vi behöver `uucp`, i andra är `dialout`, vilket är gruppägare av filen.

2. Lägg till användaren till den i gruppen:

   ```bash
   sudo usermod -a -G group-name username
   ```

   Där `group-name` är den information som finns i det första steget och `username` är ditt linux-användarnamn.

3. Du måste logga ut och in igen för att ändringen ska börja gälla och slutföra installationen.

## <a name="summary"></a>Sammanfattning
Du har lärt dig hur du konfigurerar Arduino-kort i den här artikeln. Nästa uppgift är att installera verktyg och program som förberedelse för att köra ett exempelprogram på Arduino-kort.

![Maskinvara är klar][hardware-is-ready]

## <a name="next-steps"></a>Nästa steg
[Skaffa dig verktyg][get-the-tools]
<!-- Images and links -->

[kit]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/kit.png
[top-micro-usb-port]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/top_usbport.jpg
[computer-usb]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/computer_usb.jpg
[hardware-is-ready]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/hardware_ready.jpg
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md