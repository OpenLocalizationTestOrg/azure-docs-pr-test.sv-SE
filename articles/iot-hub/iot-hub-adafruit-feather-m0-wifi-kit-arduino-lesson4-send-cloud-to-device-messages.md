---
title: 'Connect Arduino (C) tooAzure IoT - lektionen 4: moln till enhet | Microsoft Docs'
description: "Ett exempelprogram som körs på Adafruit ludd M0 WiFi och Övervakare inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden tooAdafruit ludd M0 WiFi från din IoT-hubb tooblink hello Indikator."
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
ms.openlocfilehash: dcddd61ff684f49436103675938d719cb227c409
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a>Kör ett exempel programmet tooreceive meddelanden moln till enhet
I den här artikeln får distribuera du ett exempelprogram på Adafruit ludd M0 WiFi Arduino-kort.

hello exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb. Du också köra en aktivitet med gulp på din dator toosend meddelanden tooyour Arduino board från din IoT-hubb. När hello exempelprogrammet får hälsningsmeddelande, blinkar hello-Indikator. Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].

## <a name="what-you-will-do"></a>Vad du ska göra
* Ansluta hello exempel programmet tooyour IoT-hubb.
* Distribuera och köra hello exempelprogrammet.
* Skicka meddelanden från din IoT-hubb tooyour Arduino board tooblink hello Indikator.

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Hur toomonitor inkommande meddelanden från din IoT-hubb.
* Hur toosend moln till enhet meddelanden från din IoT-hubb tooyour Arduino-kort.

## <a name="what-you-need"></a>Vad du behöver
* Kort din Arduino som ställts in för användning. hur tooset in Arduino-kort Se toolearn [konfigurera din enhet][configure-your-device].
* En IoT-hubb som skapas i din Azure-prenumeration. toolearn hur toocreate din IoT-hubb finns [skapa Azure IoT Hub][create-your-azure-iot-hub].

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a>Ansluta hello exempel programmet tooyour IoT-hubb

1. Kontrollera att du arbetar i hello lagringsplatsen mappen `iot-hub-c-feather-m0-getting-started`.

   Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:

   ```bash
   cd Lesson4
   code .
   ```

   Meddelande hello `app.ino` filen i hello `app` undermappen. Hej `app.ino` filen är hello källa som innehåller hello toomonitor inkommande meddelanden från hello IoT-hubb. Hej `blinkLED` funktionen blinkar hello-Indikator.

   ![Lagringsplatsen strukturen i hello exempelprogram][repo-structure]

2. Hämta hello serieport av hello-enhet med hello enheten identifiering cli:

   ```bash
   devdisco list --usb
   ```

   Du bör se utdata som är liknande toohello följande och hitta hello usb COM-port för Arduino-skiva:

   ![Identifiering av nätverksenheter][device-discovery]

3. Öppna hello filen `config.json` i hello lektionen mapp och inkommande hello värdet för hello hitta COM-portnummer:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > För hello COM-port på Windows-plattformen, den har hello format för `COM1, COM2, ...`. I macOS eller Ubuntu startas med `/dev/`.

4. Initiera hello konfigurationsfilen genom att köra följande kommandon hello:

   ```bash
   # For Windows command prompt
   npm install
   gulp init
   gulp install-tools
   ```

5. Se följande ersättningar i hello hello `config-arduino.json` fil:

   Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på den här datorn alla konfigurationer av hello ärvs, så du kan hoppa över hello steg toohello aktiviteten för att distribuera och Kör hello exempelprogrammet. Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på en annan dator måste tooreplace hello platshållare i hello `config-arduino.json` fil. Hej `config-arduino.json` filen har hello undermapp i arbetsmappen.

   ![Innehållet i hello arduino.json config-fil][config-arduino-json]

   * Ersätt **[Wi-Fi SSID]** med din Wi-Fi-SSID som anslutna toohello Internet.
   * Ersätt **[Wi-Fi-lösenord]** med Wi-Fi-lösenord. Ta bort hello sträng om din Wi-Fi inte kräver lösenord.
   * Ersätt **[anslutningssträngen för IoT-enhet]** med anslutningssträngen för hello enhet som du får genom att köra hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` kommando.
   * Ersätt **[anslutningssträngen för IoT-hubb]** med hello anslutningssträngen för IoT-hubb som är tillgängliga genom att köra hello `az iot hub show-connection-string --name {my hub name}` kommando.

## <a name="deploy-and-run-hello-sample-application"></a>Distribuera och köra hello exempelprogrammet
Distribuera och köra hello exempelprogrammet på Arduino-kort genom att köra följande kommandon hello:

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

Hej gulp kommandot distribuerar hello exempel programmet tooyour Arduino kort. Sedan körs programmet hello på Arduino-kort och en separat åtgärd på din värd datorn toosend 20 blinka meddelanden tooyour Arduino board från din IoT-hubb.

När hello exempelprogrammet körs, startas lyssnar toomessages från IoT-hubb. Under tiden skickar hello gulp aktivitet flera ”blinkar” meddelanden från din IoT-hubb tooyour Arduino-kort. För varje blinka meddelande som hello board får hello exempelprogrammet anropar hello `blinkLED` funktionen tooblink hello Indikator.

Du bör se hello Indikator blinkar varannan sekund som hello gulp aktivitet skickar 20 meddelanden från din IoT-hubb tooyour Arduino-kort. hello senast är en ett ”stop” visas som stoppar hello program från att köras.

![Exempelprogrammet med gulp kommandot och blinkar meddelanden][sample-application]

## <a name="summary"></a>Sammanfattning
Du har skickat meddelanden från din IoT-hubb tooyour Arduino board tooblink hello Indikator. hello nästa uppgift är valfritt: ändra hello och inaktivera beteendet för hello Indikator.

## <a name="next-steps"></a>Nästa steg
[Ändra hello och inaktivera beteendet för hello Indikator][change-the-on-and-off-led-behavior]


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