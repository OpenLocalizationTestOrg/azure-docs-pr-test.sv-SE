---
title: "Connect Arduino (C) till Azure IoT - lektionen 3: köra exemplet | Microsoft Docs"
description: "Distribuera och köra ett exempelprogram till Adafruit ludd M0 WiFi som skickar meddelanden till din IoT-hubb och blinkar på Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-Molntjänsten, arduino skicka data till molnet"
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
ms.openlocfilehash: 0c17fe74dbd78abca955f7789a1674ed6333367f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Kör ett exempelprogram för att skicka meddelanden från enhet till moln
## <a name="what-you-will-do"></a>Vad du ska göra
Den här artikeln visar hur du distribuera och köra ett exempelprogram på Adafruit ludd M0 WiFi Arduino-kort som skickar meddelanden till din IoT-hubb.

Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
Du lära dig hur du använder verktyget gulp att distribuera och köra Arduino exempelprogrammet på Arduino-kort.

## <a name="what-you-need"></a>Vad du behöver
* Innan du börjar den här uppgiften måste har slutförts [skapa en funktionsapp i Azure-och ett lagringskonto för att bearbeta och lagra IoT-hubb meddelanden][process-and-store-iot-hub-messages].

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Hämta din IoT-hubb och enheten anslutningssträngar
Anslutningssträngen enheten används för att ansluta Arduino-kort till din IoT-hubb. IoT-hubb anslutningssträngen används för att ansluta din IoT-hubb för enhetens identitet som representerar Arduino-kort i IoT-hubben.

* Lista alla IoT hubs i resursgruppen genom att köra följande kommando i Azure CLI:

```bash
az iot hub list -g iot-sample --query [].name
```

Använd `iot-sample` som värde för `{resource group name}` om du inte ändra värdet.

* Hämta anslutningssträngen för IoT-hubb genom att köra följande kommando i Azure CLI:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}`är det namn som du angav när du skapade din IoT-hubb och registrerat Arduino-kort.

* Hämta anslutningssträngen för enheten genom att köra följande kommando:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id mym0wifi
```

Använd `mym0wifi` som värde för `{device id}` om du inte ändra värdet.
## <a name="configure-the-device-connection"></a>Konfigurera enhetsanslutning
Följ dessa steg för att konfigurera enhetsanslutningen:

1. Hämta den seriella porten på enheten med enheten identifiering cli:

   ```bash
   devdisco list --usb
   ```

   Du bör se utdata som liknar följande och hitta usb COM-port för Arduino-skiva:

   ![Identifiering av nätverksenheter][device-discovery]

2. Öppna filen `config.json` i mappen lektionen och lägga till värdet för hittade COM-portnummer:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > COM-porten på Windows-plattformen, den har formatet för `COM1, COM2, ...`. I macOS eller Ubuntu det börjar med `/dev/`.

3. Initiera konfigurationsfilen genom att köra följande kommandon:

   ```bash
   npm install
   gulp init
   gulp install-tools
   ```
4. Öppna konfigurationsfilen enheten `config-arduino.json` i Visual Studio-koden genom att köra följande kommando:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```

   ![config-arduino.json][config-arduino-json]

5. Se följande ersättningar i den `config-arduino.json` filen:

   * Ersätt **[Wi-Fi SSID]** med din Wi-Fi-SSID ansluten till Internet.
   * Ersätt **[Wi-Fi-lösenord]** med Wi-Fi-lösenord. Ta bort strängen om din Wi-Fi inte kräver lösenord.
   * Ersätt **[anslutningssträngen för IoT-enhet]** med den `device connection string` du fick.
   * Ersätt **[anslutningssträngen för IoT-hubb]** med den `iot hub connection string` du fick.

   > [!NOTE]
   > Du behöver inte `azure_storage_connection_string` i den här artikeln. Se till att den är.

## <a name="deploy-and-run-the-sample-application"></a>Distribuera och köra exempelprogrammet
Distribuera och köra exempelprogrammet på Arduino-kort genom att köra följande kommando:

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

> [!NOTE]
> Standard gulp aktiviteten körs `install-tools` och `run` aktiviteter sekventiellt. När du [har distribuerat appen blinka][deployed-the-blink-app], du körde dessa uppgifter separat.

## <a name="verify-that-the-sample-application-works"></a>Kontrollera att det fungerar exempelprogrammet
Du bör se den GPIO #0 inbyggd Indikator blinkande varannan sekunder. Varje gång Indikatorn blinkar exempelprogrammet skickar ett meddelande till din IoT-hubb och verifierar att meddelandet har skickats till din IoT-hubb. Dessutom kan ut varje meddelande tas emot av IoT-hubben i konsolfönstret. Exempelprogrammet avbryter automatiskt efter 20 meddelanden skickas.

![Exempelprogrammet med skickade och mottagna meddelanden][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Sammanfattning
Du har distribuerat och köra den nya blinka exempelprogrammet på Arduino-kort för att skicka meddelanden från enhet till moln till din IoT-hubb. Du kan nu övervaka dina meddelanden när de skrivs till lagringskontot.

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