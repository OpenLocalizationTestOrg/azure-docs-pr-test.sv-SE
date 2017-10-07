---
title: "Connect Arduino (C) tooAzure IoT - lektionen 3: köra exemplet | Microsoft Docs"
description: "Distribuera och köra en exempel programmet tooAdafruit ludd M0 WiFi som skickar meddelanden tooyour IoT-hubb och blinkar hello-Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-Molntjänsten, arduino skicka data toocloud"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 92cce319-2b17-4c9b-889d-deac959e3e7c
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ddca015a3655f8a1a9de2a00e718ec0d28a5affb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a>Kör ett exempel programmet toosend meddelanden från enhet till moln
## <a name="what-you-will-do"></a>Vad du ska göra
Den här artikeln visar hur toodeploy och kör ett exempelprogram på din Adafruit ludd M0 WiFi Arduino board som skickar meddelanden tooyour IoT-hubb.

Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
Du kommer lära dig hur toouse hello gulp verktyget toodeploy och köra hello exempelprogrammet Arduino på Arduino-kort.

## <a name="what-you-need"></a>Vad du behöver
* Innan du börjar den här uppgiften måste har slutförts [skapa en funktionsapp i Azure-och en storage-konto tooprocess och lagra IoT-hubb meddelanden][process-and-store-iot-hub-messages].

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Hämta din IoT-hubb och enheten anslutningssträngar
Hej enheten anslutningssträngen är används tooconnect din Arduino board tooyour IoT-hubb. anslutningssträngen för hello IoT hub är används tooconnect din IoT-hubb toohello enhetsidentitet som representerar din Arduino board i hello IoT-hubb.

* Lista alla IoT hubs i resursgruppen genom att köra hello följande Azure CLI-kommando:

```bash
az iot hub list -g iot-sample --query [].name
```

Använd `iot-sample` som hello värde för `{resource group name}` om du inte ändrar hello-värdet.

* Hämta hello IoT hub-anslutningssträng genom att köra hello följande Azure CLI-kommando:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}`är hello-namn som du angav när du skapade din IoT-hubb och registrerat Arduino-kort.

* Hämta anslutningssträngen för hello enheten genom att köra följande kommando hello:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id mym0wifi
```

Använd `mym0wifi` som hello värde för `{device id}` om du inte ändrar hello-värdet.
## <a name="configure-hello-device-connection"></a>Konfigurera hello enhetsanslutning
tooconfigure Hej enhetsanslutning, gör du följande:

1. Hämta hello serieport av hello-enhet med hello enheten identifiering cli:

   ```bash
   devdisco list --usb
   ```

   Du bör se utdata som är liknande toohello följande och hitta hello usb COM-port för Arduino-skiva:

   ![Identifiering av nätverksenheter][device-discovery]

2. Öppna hello filen `config.json` i hello lektionen mappen och Lägg till hello värdet hello hitta COM-portnummer:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > För hello COM-port på Windows-plattformen, den har hello format för `COM1, COM2, ...`. I macOS eller Ubuntu det börjar med `/dev/`.

3. Initiera hello konfigurationsfilen genom att köra följande kommandon hello:

   ```bash
   npm install
   gulp init
   gulp install-tools
   ```
4. Öppna hello enheten konfigurationsfilen `config-arduino.json` i Visual Studio-koden genom att köra följande kommando hello:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```

   ![config-arduino.json][config-arduino-json]

5. Se följande ersättningar i hello hello `config-arduino.json` fil:

   * Ersätt **[Wi-Fi SSID]** med din Wi-Fi-SSID som anslutna toohello Internet.
   * Ersätt **[Wi-Fi-lösenord]** med Wi-Fi-lösenord. Ta bort hello sträng om din Wi-Fi inte kräver lösenord.
   * Ersätt **[anslutningssträngen för IoT-enhet]** med hello `device connection string` du fick.
   * Ersätt **[anslutningssträngen för IoT-hubb]** med hello `iot hub connection string` du fick.

   > [!NOTE]
   > Du behöver inte `azure_storage_connection_string` i den här artikeln. Se till att den är.

## <a name="deploy-and-run-hello-sample-application"></a>Distribuera och köra hello exempelprogrammet
Distribuera och köra hello exempelprogrammet på Arduino-kort genom att köra följande kommando hello:

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

> [!NOTE]
> hello standard gulp aktiviteten körs `install-tools` och `run` aktiviteter sekventiellt. När du [distribuerat hello blinka app][deployed-the-blink-app], du körde dessa uppgifter separat.

## <a name="verify-that-hello-sample-application-works"></a>Kontrollera att det fungerar hello exempelprogrammet
Du bör se hello GPIO #0 inbyggd Indikator blinkande varannan sekund. Varje gång hello Indikator blinkar hello exempelprogrammet skickar ett meddelande tooyour IoT-hubb och verifierar att hello-meddelande har skickats tooyour IoT-hubb. Dessutom kan ut varje meddelande tas emot av hello IoT-hubb i hello konsolfönstret. hello exempelprogrammet avbryter automatiskt efter 20 meddelanden skickas.

![Exempelprogrammet med skickade och mottagna meddelanden][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Sammanfattning
Du har distribuerat och köra hello nya blinka exempelprogrammet på din Arduino board toosend meddelanden från enhet till moln tooyour IoT-hubb. Du kan nu övervaka dina meddelanden som de är skrivna toohello storage-konto.

## <a name="next-steps"></a>Nästa steg
[Läs meddelandena kvar i Azure Storage][read-messages-persisted-in-azure-storage]
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/config-arduino.png
[deployed-the-blink-app]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_run_arduino.png
[read-messages-persisted-in-azure-storage]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-read-table-storage.md