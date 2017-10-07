---
title: 'Ansluta Arduino tooAzure IoT - lektionen 1: distribuera appen | Microsoft Docs'
description: "Klona hello Arduino exempelprogrammet från GitHub och kör det här programmet tooyour Adafruit ludd M0 WiFi för gulp toodeploy. Det här exempelprogrammet blinkar hello GPIO"
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
ms.openlocfilehash: 5bf8e4ae88e070aeacf34bfc43b8d2daeeb1a2fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a>Skapa och distribuera hello blinka program
## <a name="what-you-will-do"></a>Vad du ska göra
Klona hello Arduino exempelprogrammet från GitHub och använda hello gulp verktyget toodeploy hello exempel programmet tooyour Adafruit ludd M0 WiFi Arduino-kort. hello exempel programmet blinkningar hello GPIO #13 på barod VÄGLEDS varannan sekund.

Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting-page].

## <a name="what-you-will-learn"></a>Vad får du lära dig
* Hur toodeploy och kör hello exempelprogram på Arduino-kort.

## <a name="what-you-need"></a>Vad du behöver
Du måste ha slutfört hello följande åtgärder:

* [Konfigurera din enhet][configure-your-device]
* [Hämta hello-verktyg][get-the-tools]

## <a name="open-hello-sample-application"></a>Öppna hello exempelprogrammet
tooopen hello exempelprogram, gör du följande:

1. Klona hello exempel databasen från GitHub genom att köra följande kommando hello:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-feather-m0-getting-started.git
   ```
2. Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:

   ```bash
   cd iot-hub-c-feather-m0-getting-started
   cd Lesson1
   code .
   ```

   ![Lagringsplatsen struktur][repo-structure]

Hej `app.ino` filen i hello `app` undermapp är hello källa fil som innehåller hello kod toocontrol hello Indikator.

### <a name="install-application-dependencies"></a>Installera beroenden
Installera hello bibliotek och andra moduler som du behöver för hello exempelprogrammet genom att köra följande kommando hello:

```bash
npm install
```

## <a name="configure-hello-device-connection"></a>Konfigurera hello enhetsanslutning
tooconfigure Hej enhetsanslutning, gör du följande:

1. Hämta hello serieport av hello-enhet med hello enheten identifiering cli:

   ```bash
   devdisco list --usb
   ```

   Du bör se utdata som är liknande toohello följande och hitta hello usb COM-port för din Arduino board: ![identifiering av nätverksenheter][device-discovery]

2. Öppna hello filen `config.json` i hello lektionen mappen och Lägg till hello värdet hello hitta COM-portnummer:

   ```json
   {
       "device_port" : "COM1"
   }
   ```
   ![Config.JSON][config-json]
   > [!NOTE]
   > För hello COM-port på Windows-plattformen, den har hello format för `COM1, COM2, ...`. I macOS eller Ubuntu det börjar med `/dev/`.

## <a name="deploy-and-run-hello-sample-application"></a>Distribuera och köra hello exempelprogrammet
### <a name="install-hello-required-tools-for-your-arduino-board"></a>Installera hello som krävs för Arduino-kort

Installera hello Azure IoT-hubb SDK för din Arduino board genom att köra följande kommando hello:

```bash
gulp install-tools
```

Den här uppgiften kan ta en lång tid toocomplete, beroende på nätverksanslutningen.

> [!NOTE]
> Avsluta hello körs Arduino IDE instans gulp uppgifter: `install-tools`, `run`.

### <a name="deploy-and-run-hello-sample-app"></a>Distribuera och köra hello sample-appen
Distribuera och köra hello exempelprogrammet genom att köra följande kommando hello:

```bash
gulp run

# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

### <a name="verify-hello-app-works"></a>Verifiera hello appen fungerar
Om du inte ser hello Indikator blinkar, se hello [felsökningsguide för] [ troubleshooting-page] för lösningar toocommon problem.

![Blinka Indikator][led-blinking]

## <a name="summary"></a>Sammanfattning
Du har installerat hello krävs verktyg toowork med Arduino-kort och distribuerat en exempel programmet tooyour Arduino board tooblink hello Indikator. Du kan nu skapa, distribuera, och kör en annan exempelprogram som ansluter din Arduino board tooAzure IoT-hubb toosend och ta emot meddelanden.

## <a name="next-steps"></a>Nästa steg
[Hämta hello Azure-verktyg][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting-page]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-blink-arduino-mac.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[led-blinking]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md