---
title: 'Ansluta Arduino (C) till Azure IoT - lektionen 4: moln till enhet | Microsoft Docs'
description: "Ett exempelprogram som körs på Adafruit ludd M0 WiFi och Övervakare inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden till Adafruit ludd M0 WiFi från din IoT-hubb blinkar på Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "arduino kontroll ledde från webben, arduino kontroll ledde via webben"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: a0bf53fb-29fb-485f-ba4a-6c715057b1a2
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b4f4c1d86b10b64c104ac812d1f650d723322be8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a>Kör ett exempelprogram som tar emot meddelanden moln till enhet
I den här artikeln får distribuera du ett exempelprogram på Adafruit ludd M0 WiFi Arduino-kort.

Exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb. Du kan också köra en aktivitet med gulp på datorn för att skicka meddelanden till Arduino-kort från din IoT-hubb. När exempelprogrammet som tar emot meddelanden, blinkar på Indikator. Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].

## <a name="what-you-will-do"></a>Vad du ska göra
* Ansluta till din IoT-hubb exempelprogrammet.
* Distribuera och köra exempelprogrammet.
* Skicka meddelanden från din IoT-hubb din Arduino planen blinkar på Indikator.

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Så här övervakar du inkommande meddelanden från din IoT-hubb.
* Hur du skickar meddelanden moln till enhet från din IoT-hubb Arduino-planen.

## <a name="what-you-need"></a>Vad du behöver
* Kort din Arduino som ställts in för användning. Information om hur du ställer in Arduino-kort finns [konfigurera din enhet][configure-your-device].
* En IoT-hubb som skapas i din Azure-prenumeration. Information om hur du skapar din IoT-hubb finns [skapa Azure IoT Hub][create-your-azure-iot-hub].

## <a name="connect-the-sample-application-to-your-iot-hub"></a>Ansluta exempelprogrammet till din IoT-hubb

1. Kontrollera att du är i mappen lagringsplatsen `iot-hub-c-feather-m0-getting-started`.

   Öppna exempelprogrammet i Visual Studio Code genom att köra följande kommandon:

   ```bash
   cd Lesson4
   code .
   ```

   Observera den `app.ino` filen i den `app` undermappen. Den `app.ino` filen är viktiga källfilen som innehåller koden för övervakning av inkommande meddelanden från IoT-hubben. Den `blinkLED` funktionen blinkar på Indikator.

   ![Lagringsplatsen strukturen i exempelprogrammet][repo-structure]

2. Hämta den seriella porten på enheten med enheten identifiering cli:

   ```bash
   devdisco list --usb
   ```

   Du bör se utdata som liknar följande och hitta usb COM-port för Arduino-skiva:

   ![Identifiering av nätverksenheter][device-discovery]

3. Öppna filen `config.json` i mappen lektionen och inmatade värdet för hittade COM-portnummer:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > COM-porten på Windows-plattformen, den har formatet för `COM1, COM2, ...`. I macOS eller Ubuntu startas med `/dev/`.

4. Initiera konfigurationsfilen genom att köra följande kommandon:

   ```bash
   # For Windows command prompt
   npm install
   gulp init
   gulp install-tools
   ```

5. Se följande ersättningar i den `config-arduino.json` filen:

   Om du har slutfört stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på den här datorn alla konfigurationer ärvs, så du kan hoppa över steg för att distribuera och köra exempelprogrammet. Om du har slutfört stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på en annan dator som du behöver ersätta platshållare i den `config-arduino.json` filen. Den `config-arduino.json` filen finns i undermappen arbetsmappen.

   ![Innehållet i filen config arduino.json][config-arduino-json]

   * Ersätt **[Wi-Fi SSID]** med din Wi-Fi-SSID ansluten till Internet.
   * Ersätt **[Wi-Fi-lösenord]** med Wi-Fi-lösenord. Ta bort strängen om din Wi-Fi inte kräver lösenord.
   * Ersätt **[anslutningssträngen för IoT-enhet]** med den anslutningssträng för enheten som du får genom att köra den `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` kommando.
   * Ersätt **[anslutningssträngen för IoT-hubb]** med IoT-hubb anslutningssträngen som du får genom att köra den `az iot hub show-connection-string --name {my hub name}` kommando.

## <a name="deploy-and-run-the-sample-application"></a>Distribuera och köra exempelprogrammet
Distribuera och köra exempelprogrammet på Arduino-kort genom att köra följande kommandon:

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

Kommandot gulp distribuerar exempelprogrammet Arduino-planen. Därefter körs programmet på Arduino-kort och en separat åtgärd på värddatorn för att skicka 20 blinka meddelanden till Arduino-kort från din IoT-hubb.

När exempelprogrammet som körs, startas lyssnar på meddelanden från din IoT-hubb. Gulp-aktivitet skickar under tiden kan flera ”blinkar” meddelanden från din IoT-hubb Arduino-planen. För varje meddelande blinka som tar emot planen exempelprogrammet anropar den `blinkLED` funktionen blinkar på Indikator.

Du bör se Indikator blinka varannan sekund som aktiviteten gulp 20 meddelanden skickas från din IoT-hubb till Arduino-kort. Den sista som är en ”stoppa” meddelande som förhindrar att program körs.

![Exempelprogrammet med gulp kommandot och blinkar meddelanden][sample-application]

## <a name="summary"></a>Sammanfattning
Du har skickat meddelanden från din IoT-hubb till din Arduino board blinkar på Indikator. Nästa uppgift är valfritt: ändra på och av beteendet för Indikatorn.

## <a name="next-steps"></a>Nästa steg
[Ändra på och av beteendet för Indikatorn][change-the-on-and-off-led-behavior]


<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/repo_structure_arduino.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[create-an-azure-function-app-and-storage-account]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/config-arduino.png
[sample-application]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_blink_arduino.png
[change-the-on-and-off-led-behavior]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-change-led-behavior.md