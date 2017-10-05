---
title: 'Ansluta Arduino till Azure IoT - lektionen 1: distribuera appen | Microsoft Docs'
description: "Klona Arduino exempelprogrammet från GitHub och kör gulp om du vill distribuera programmet till din Adafruit ludd M0 WiFi. Det här exempelprogrammet blinkar på GPIO"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: arduino ledde projekt, arduino ledde blinka, arduino ledde blinka koden, arduino blinka program, arduino blinka exempel
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: b0a7d076-d580-4686-9f7d-c0712750b615
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4431808ac6182d194e841c087c8f89f1a12b1911
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a>Skapa och distribuera blinkningsprogrammet
## <a name="what-you-will-do"></a>Vad du ska göra
Klona Arduino exempelprogrammet från GitHub och verktyget gulp för att distribuera exempelprogrammet Adafruit ludd M0 WiFi Arduino-planen. Exempel programmet blinkningar den GPIO #13 på-barod VÄGLEDS varannan sekund.

Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting-page].

## <a name="what-you-will-learn"></a>Vad får du lära dig
* Hur du distribuerar och kör exempelprogrammet på Arduino-kort.

## <a name="what-you-need"></a>Vad du behöver
Du måste ha slutfört följande åtgärder:

* [Konfigurera din enhet][configure-your-device]
* [Skaffa dig verktyg][get-the-tools]

## <a name="open-the-sample-application"></a>Öppna exempelprogrammet
Följ dessa steg om du vill öppna exempelprogrammet:

1. Klona lagringsplatsen exempel från GitHub genom att köra följande kommando:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-feather-m0-getting-started.git
   ```
2. Öppna exempelprogrammet i Visual Studio Code genom att köra följande kommandon:

   ```bash
   cd iot-hub-c-feather-m0-getting-started
   cd Lesson1
   code .
   ```

   ![Lagringsplatsen struktur][repo-structure]

Den `app.ino` filen i den `app` undermapp är viktiga källfilen som innehåller koden för att styra Indikatorn.

### <a name="install-application-dependencies"></a>Installera beroenden
Installera bibliotek och andra moduler som du behöver för exempelprogrammet genom att köra följande kommando:

```bash
npm install
```

## <a name="configure-the-device-connection"></a>Konfigurera enhetsanslutning
Följ dessa steg för att konfigurera enhetsanslutningen:

1. Hämta den seriella porten på enheten med enheten identifiering cli:

   ```bash
   devdisco list --usb
   ```

   Du bör se utdata som liknar följande och hitta usb COM-port för din Arduino board: ![identifiering av nätverksenheter][device-discovery]

2. Öppna filen `config.json` i mappen lektionen och lägga till värdet för hittade COM-portnummer:

   ```json
   {
       "device_port" : "COM1"
   }
   ```
   ![Config.JSON][config-json]
   > [!NOTE]
   > COM-porten på Windows-plattformen, den har formatet för `COM1, COM2, ...`. I macOS eller Ubuntu det börjar med `/dev/`.

## <a name="deploy-and-run-the-sample-application"></a>Distribuera och köra exempelprogrammet
### <a name="install-the-required-tools-for-your-arduino-board"></a>Installera nödvändiga för Arduino-kort

Installera Azure IoT-hubb SDK för din Arduino board genom att köra följande kommando:

```bash
gulp install-tools
```

Den här uppgiften kan ta lång tid att slutföra beroende på nätverksanslutningen.

> [!NOTE]
> Avsluta Arduino IDE-instans som körs när du kör gulp uppgifter: `install-tools`, `run`.

### <a name="deploy-and-run-the-sample-app"></a>Distribuera och köra sample-appen
Distribuera och köra exempelprogrammet genom att köra följande kommando:

```bash
gulp run

# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

### <a name="verify-the-app-works"></a>Kontrollera appen fungerar
Om du inte ser Indikator blinkar, finns det [felsökningsguide för] [ troubleshooting-page] efter lösningar på vanliga problem.

![Blinka Indikator][led-blinking]

## <a name="summary"></a>Sammanfattning
Du har installerat de verktyg som krävs för att arbeta med Arduino-kort och distribuerat ett exempelprogram Arduino-planen blinkar på Indikator. Du kan nu skapa, distribuera och köra en annan exempelprogrammet som ansluter Arduino-kort till Azure IoT Hub skicka och ta emot meddelanden.

## <a name="next-steps"></a>Nästa steg
[Hämta Azure-verktyg][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting-page]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-blink-arduino-mac.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[led-blinking]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md