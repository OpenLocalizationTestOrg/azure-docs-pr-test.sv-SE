---
title: "SensorTag enhet & Azure IoT-Gateway - lektion 3: kör exempelappen | Microsoft Docs"
description: "Kör en TIVERA exempel tooreceive programdata från TIVERA SensorTag och från din IoT-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Aktivera appen, sensor övervaka app, insamling av sensor, data från sensorer, sensor data toocloud"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: b33e53a1-1df7-4412-ade1-45185aec5bef
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4a8acdeadd402ffc82d3b766e1ec03a77ddcebb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-run-a-ble-sample-application"></a>Konfigurera och köra ett TIVERA exempelprogram

## <a name="what-you-will-do"></a>Vad du ska göra

- Klona hello exempel databas. 
- Ställ in hello anslutningen mellan SensorTag och Intel NUC. 
- Använd hello Azure CLI tooget din IoT-hubb och SensorTag information för ett exempelprogram TIVERA (Bluetooth låg energi). Konfigurera och köra hello TIVERA exempelprogrammet. 

Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här artikeln får du lära dig:

- Hur tooconfigure och kör hello TIVERA exempelprogrammet.

## <a name="what-you-need"></a>Vad du behöver

Du måste ha slutfört

- [Skapa en IoT-hubb och registrera SensorTag](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="clone-hello-sample-repository-toohello-host-computer"></a>Klona hello exempel databasen toohello värddatorn

tooclone hello exempel lagringsplatsen, Följ dessa steg på värddatorn för hello:

1. Öppna en kommandotolk i Windows eller en terminal i macOS eller Ubuntu.
2. Kör följande kommandon hello:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="set-up-hello-connectivity-between-sensortag-and-intel-nuc"></a>Konfigurera hello anslutningar mellan SensorTag och Intel NUC

tooset upp hello anslutningar, Följ dessa steg på värddatorn för hello:

1. Initiera hello konfigurationsfilen genom att köra följande kommandon hello:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

2. Öppna `config-gateway.json` i Visual Studio-koden genom att köra följande kommando hello:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

3. Leta upp följande kodrad hello och ersätta `[device hostname or IP address]` med hello IP-adressen eller värdnamnet namnet Intel NUC.
   ![Skärmbild av config-gateway](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

4. Installera helper verktyg på Intel NUC genom att köra följande kommando hello:

   ```bash
   gulp install-tools
   ```

5. Aktivera SensorTag genom att trycka på strömknappen hello som hello följande bild och hello grön Indikator blinka.

   ![Aktivera Sensor Tag](media/iot-hub-gateway-kit-lessons/lesson3/turn on_off sensortag.jpg)

6. Sök igenom SensorTag-enheter genom att köra följande kommandon hello:

   ```bash
   gulp discover-sensortag
   ```

7. Testa hello anslutningen mellan hello SensorTag och Intel NUC genom att köra följande kommando hello:

   ```bash
   gulp test-connectivity --mac {mac address}
   ```

   Ersätt `{mac address}` med hello MAC-adress som du fick i hello föregående steg.

## <a name="get-hello-connection-string-of-sensortag"></a>Hämta hello anslutningssträngen för SensorTag

tooget hello Azure IoT hub-anslutningssträngen för SensorTag, kör följande kommando på värddatorn för hello hello:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}`är hello IoT-hubbnamn som du använde. Använd iot-gateway som hello värde för `{resource group name}` och använda mydevice som hello värdet för `{device id}` om du inte ändrar hello värdet i lektionen 2.

## <a name="configure-hello-ble-sample-application"></a>Konfigurera hello TIVERA exempelprogrammet

tooconfigure och kör hello TIVERA exempelprogrammet, Följ dessa steg på värddatorn för hello:

1. Öppna `config-sensortag.json` i Visual Studio-koden genom att köra följande kommando hello:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![Skärmbild av config sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_sensortag.png)

2. Att Hej följande ersättningar i hello kod:
   - Ersätt `[IoT hub name]` med hello IoT-hubbnamn som du använde.
   - Ersätt `[IoT device connection string]` med hello anslutningssträngen för SensorTag som du fick.
   - Ersätt `[device_mac_address]` med hello hello SensorTag som du fick MAC-adress.

3. Köra hello TIVERA exempelprogrammet.

   toorun hello TIVERA exempelprogrammet, Följ dessa steg på värddatorn för hello:

   1. Aktivera SensorTag.

   2. Distribuera och köra exempelprogram för hello TIVERA på Intel NUC genom att köra följande kommando hello:
   
      ```bash
      gulp run
      ```

## <a name="verify-that-hello-ble-sample-application-works"></a>Kontrollera att det fungerar hello TIVERA exempelprogrammet

Du bör nu se utdata som liknar hello följande:

![TIVERA exempel på utdata från programmet](media/iot-hub-gateway-kit-lessons/lesson3/BLE_running.png)

hello exempelprogrammet håller temperatur datainsamling och skicka den tooyour IoT-hubb. hello exempelprogrammet avbryter automatiskt när du har skickat 40 sekunder.

## <a name="summary"></a>Sammanfattning

Du har har konfigurera hello anslutningar mellan SensorTag och Intel NUC och kör ett tabell-exempelprogram som samlar in och skickar data från SensorTag tooyour IoT-hubb. Du är klar toolearn hur tooverify som har tagit emot din IoT-hubb hello data.

## <a name="next-steps"></a>Nästa steg
[Läs meddelanden från din IoT-hubb](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)